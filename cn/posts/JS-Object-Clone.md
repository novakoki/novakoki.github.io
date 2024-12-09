---
title: Jade如何用同一模板渲染出不同文章的页面
date: 2016-04-02
locale: zh_CN
---

What:
-----

在写我的[blog-generator](https://github.com/novakoki/blog-generator)的时候，遇到了传说中面试经常考的对象复制的问题。
在用Jade模板的时候，要用同一个模板渲染出不同的文章，就需要有不同的locals。
一开始的想法是，用Jade里面的include markdown，这样只要文件名做变量就可以了，然而事实上Jade并不支持动态include。
于是只能改用先用marked解析markdown文件为字符串然后填充进去，假设有如下内容


post.jade


```js
article !{article} //- 这里是解析markdown后得到的字符串，会带有很多转义符号，所以用 !{}的变量表达
```




locals.json


```json
{
    "articles": {
        "title": "path",
        "title1": "path1"
    },
    "article": "balabala..."
}
```





那么在JavaScript中就可以


```javascript
gulp.task('template', ()=>{
    var locals = require('./locals.json');
    for (var title in locals.articles) {    /*    这里做markdown解析并改变locals中article的值    */
        gulp.src('./post.jade')
            .pipe(jade({
                locals: locals
            }))
            .pipe(rename(`${title}.html`)) //gulp.dest只能指定输出的目录而不能指定文件名，需要rename = require('gulp-rename')
            .pipe(gulp.dest('./posts'));
        }
    }
)
```




然后一切都是由异步引起的问题了，为了符合gulp的风格，我用stream来读取markdown文件的内容


```javascript
var stream = fs.createReadStream(locals.articles[title]);
var data = '';stream.on('data', (chunk) => {  data += chunk;});
stream.on('end', () => {  locals.article = marked(data);});
```




因为显然这个过程是异步的，所以我用一个闭包来存每次迭代的title


```js
gulp.task('template', ()=>{  var locals = require('./locals.json'); //这里我一度以为locals也会是在闭包环境里的
for (var title in locals.articles) {
    ((title, locals) => {
        return () => {
            var stream = fs.createReadStream(locals.articles[title]);
            var data = '';
            stream.on('data', (chunk) => {
                data += chunk;
            });
            stream.on('end', () => {
                locals.article = marked(data); //这里是会影响外部的locals的
                gulp.src('./post.jade')
                    .pipe(jade({
                        locals: locals
                        }))
                    .pipe(rename(`${title}.html`))
                    .pipe(gulp.dest('./posts'));
                }
            );
        }
    })(title, locals);
}
```




事实上这样最终只会产生相同的页面，即最后迭代的那一次。
意识到了其中的问题所在之后，我继续做了些修改


```js
gulp.task('template', ()=>{
    for (var title in locals.articles) {
        ((title) => {
             return () => {
                var stream = fs.createReadStream(locals.articles[title]);
                var data = '';
                var locals = require('./locals.json'); //喂，这样总可以每个闭包环境都有一份locals了吧
                stream.on('data', (chunk) => {
                    data += chunk;        });
                    stream.on('end', () => {
                        locals.article = marked(data);
                        gulp.src('./post.jade')
                        .pipe(jade({
                            locals: locals
                            }))
                            .pipe(rename(`${title}.html`))
                            .pipe(gulp.dest('./posts'));
                        });
                    }
        })(title);
    }
});
```




结果是被第二次打脸了，于是我只好寻求复制多份原始的locals，再分别做设置。
这里我没有用比较常规的写一个递归函数或者用原型来做复制，而是选择直接操作JSON字符串。


```js
stream.on('end', () => {
    gulp.src('./post.jade')
        .pipe(jade({
            locals: JSON.parse(`${JSON.stringify(locals).slice(0,-1)},${marked(data)}}`)
        }))
        .pipe(rename(`${title}.html`))
        .pipe(gulp.dest('./posts'));
    }
);
```




这样就用同一个模板给每篇文章都生成了一个页面，相应的，主页中需要展示一个文章摘要列表也可以做类似的实现。


Why:
----

回到上面的过程中，为什么我说一切问题都是由异步引起的？更确切些，可以说是由异步和引用类型引起的。


首先，无论是node自身的stream还是gulp的stream，在读取或是写入时都是异步执行的，用于迭代的变量最终被用到时循环早已结束，这样的情形在DOM批量绑定事件的时候也早已见怪不怪了。


一个简单的闭包可以解决问题，这么做了之后第一次修改我意识到了问题在于传参的类型，字符串、数字都在闭包中保存了一份拷贝，而引用类型或者说对象并没有，或者说在闭包中只是存了一份该指向对象指针的拷贝而已。


改完之后为什么还是不对呢？确实每个闭包都有了一个locals才对啊。问题在于gulp的stream也是异步的，即gulp用到locals的时候，匿名函数调用已经结束了，locals作为局部变量已经被销毁了。


How:
----

《JavaScript高级程序设计》 P69-71


[深入剖析 JavaScript 的深复制](http://jerryzou.com/posts/dive-into-deep-clone-in-javascript/)
