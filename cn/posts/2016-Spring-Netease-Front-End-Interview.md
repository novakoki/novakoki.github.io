---
title: 网易前端暑期实习生笔试面经（2016春）
date: 2016-04-15
locale: zh_CN
---

笔试
---

选择题忽略


问答题：


1. 用原生JS实现一个接口，能够用Ajax上传文件并显示上传进度，上传完成后接收一个来自服务器的json数据
2. 实现一个三列布局
3. 如何防范CSRF（跨站请求伪造）
4. 请列举减少HTTP请求数和资源文件大小的方法
[Reference](https://csspod.com/frontend-performance-best-practices/)
5. 列举实现跨域请求的方法


一面
--

一上来先是自我介绍，在这个过程中，面试官会看你的简历


Q：CSS和JS熟悉哪个？


A：JS


Q：浏览器端的JS包含哪几个部分？


A：ECMAScript+DOM


Q：DOM包含哪些对象？


A：Node对象，然后继承下来的有Document,Element,Text，还有想不起来了


(常用的：Node(Document, Element, CharacterData(Text, Comment, CDATASection), DocumentFragment), Window, Attr, NodeList, Event, etc. )


Q：JS有哪些基本类型？


A：Number, String, undefined, null, 还有引用类型的


( 这里我少答了Boolean，然后我以为问的是数据类型。ES6 新增了 Symbol )


Q：基本类型和引用类型有什么区别？


A：赋值的时候基本类型按值，引用类型按引用，就是基本类型会复制一份，引用类型就是一个新的指针。函数传参的时候都是按值传递.



Q：



```javascript
var obj = {a : 1};
(function (obj) {
    obj = {a : 2};
})(obj);
//问obj怎么变？ ```
```


A：外面的obj不变，因为里面等于让局部的obj指向了一个新的对象


// 下面两题是因为我简历有写我会C++


Q：C++ 的引用类型是怎么样的？


A：C++ 里面的引用相当于一个变量的别名，对引用做操作也会影响该变量


Q：JS 和 C++ 有什么区别？


A：面向对象不一样，C++ 是类式继承，JS 是原型链式。C++ 在函数式方面没有JS来的强。JS 没有 C++ 的一些高级特性，比如模板、泛型。


Q：实现一个左边定宽右边自适应的两列布局，要求使用浮动和flex两种方法


A：




```html
<div class="root">
    <div class="left"></div>
    <div class="right"></div>
</div>
```




```css
.left {  float: left;  width: 320px; //这个是面试官给的，面试官画了图}

/* 下面这段我少写了，然后面试官就问你上面这样能够让右边自适应吗？囧 */
.right {  margin-left: 320px;}

/* flex 憋了很久没写出来。。。 */
.root {  display: flex;}
.left {  width: 320px;}
.right {  flex: 1;}
```


Q：position有哪些属性？各自有什么特点？


A：


* static：正常文档流
* relative：相对于正常文档流中的位置定位
* absolute：相对于第一个不是static的父元素定位
* fixed：相对于浏览器定位


Q：画一下标准盒模型


A：// 感觉这里已经质疑我CSS是不是一点不会了，囧


Q：闭包是什么？有什么用？


A：// 这里我讲得很不清楚，大致说了就是函数里面套函数，可以保存变量


Q：ES5中，除了函数，什么能够产生作用域？


A：对象？
// 然后被回问对象有作用域吗？然后我说对象里面this会变。。。完全把作用域和执行环境弄混了


( with 和 catch 都能产生作用域 )


Q：函数有哪几种调用方式？


A：


* 直接调用
* 作为对象的方法调用
* apply, call


Q：




```javascript
var obj = {
    a : 1,
    func : function () {
        (function () {
            a = 2;
        })();
    }
};
obj.func();
```


问 **a** 怎么变，匿名函数里的 **this** 是什么？怎么改变里面的 **this** ？匿名函数不能传参怎么改变 **obj.a** 的值？


A: obj.a 不变，匿名函数里的 this 指向全局对象(window)，相当于给 window 加了一个名为 a 的属性。




```javascript
(function () {
    this.a = 2;
}).call(this);
//或者apply
//或者
func: function () {
    var self = this;
    (function () {
        self.a = 2;
    })();
}
```


Q：描述一下事件模型？IE的事件模型是怎样的？事件代理是什么？事件代理中怎么定位实际事件产生的目标？


A：捕获->处于目标->冒泡，IE应该是只有冒泡没有捕获。
事件代理就是在父元素上绑定事件来处理，通过 event 对象的 target 来定位。


Q：JS动画有哪些实现方法？


A：不太清楚


( setTimeout/setInterval, requestAnimactionFrame )


Q：那你知道还有什么实现动画的方法？


A：CSS3的animation，还有用canvas做的算吗？


[CSS Versus JavaScript Animations](https://developers.google.com/web/fundamentals/design-and-ui/animations/css-vs-javascript)


Q：你最近有用过什么框架或库？


A：用React Native做安卓，还在学 // 然后面试官说那你现在还是主要在原生JS的层面咯


Q：你主要有哪些学习渠道？


A：MDN,Udacity,慕课网


Q：node.js有用过吗？


A：有，主要用一些工具，比如gulp


Q：你有用过什么代码管理工具？


A：// 然后面试官看到简历上有GitHub就不问了


Q：你还有什么问题吗？


A：// 这里我问了这个部门主要是面向技术人员还是普通用户开发，回答是都有，还回问了我一句你想做哪方面


最后面试官说你先去休息一下，待会儿还有个二面 // 意思是一面居然过了。。。


二面
--

二面没让自我介绍，直接看到简历上有写项目经历就开问了


Q：说说你做过的项目以及从项目中学到了什么


A：// balabala… 感觉很紧张，说得不是很清楚


Q：函数声明和函数表达式有什么区别？


A：函数声明会将那个函数提升到最前面，成为全局函数。函数声明要指定函数名，而函数表达式不用，可以用作匿名函数。


Q：作用域链是什么？


A：// 说的不是很清楚


Q：面向对象有哪几个特点？


A：继承、多态。。。想不起来了


( 封装、继承、多态、抽象 )


Q：JS怎么实现继承？


A：将父对象的一个实例赋值给子对象的原型


Q：怎么判断属性来自对象自身还是原型链？


A：hasOwnProperty


Q：双向链表怎么找中点？


A：头尾指针都往中间走，两个指针相等或交替的时候为中点


Q：单向链表呢？


A：先走到尾记下有几个元素，然后再走到一半的地方
//面完查了下可以用快慢指针，一个指针每次走一步，另外个走两步，快指针到尾部时慢指针在中点


Q：上次笔试之后有没有学到什么？


A：了解了下跨域安全和性能相关的问题


Q：那你描述一下跨域安全问题吧


A：//balabala…最后发现根本讲不清楚，我还说考完之后去翻了《HTTP权威指南》，囧


( CSRF（跨站请求伪造）： 诱骗用户去访问曾经访问过的保有凭据的网站。 检查 Referer 头， 或者 URL 增加校验 token )


Q：怎么实现跨域请求？


A：JSONP，http自定义origin头部


( JSONP, CORS, iframe(window.name or document.domain), postMessage )


Q：只写origin就够了吗？


A：不太清楚


Q：解释下TCP三次握手


A：客户端发一个SYN，服务器回一个ACK，客户端再回一个ACK


Q：HTTP头中哪些是和缓存相关的？


A：ETag，cache-control。。。想不起来了


Q：cookie和session有什么区别？


A：cookie在客户端，session在服务端


Q：浏览器在发送cookie时会发送哪几个部分？


A：不太清楚


( value )


Q：那你知道cookie有哪几个组成部分吗？


A：不太清楚


( name, value, domain, path, expires/max-age, size, http-only, secure, samesite )


Q：你有用开发者工具看过cookie吗？


A：有


Q：那cookie有哪几个组成部分？


A：// 已死，我的内心是崩溃的


Q：我没有问题了，你还有什么问题吗？


A：// 崩溃的我居然问了刚刚那个cookie的问题可以告诉我答案吗。。。估计被定位成伸手党了


面完之后去前台问被告知二面挂了，然后愉快（误）地滚回了学校


总结
--

* 一面比较注重基础知识，这方面听到有的面试官建议多看几遍《JavaScript高级程序设计》的，现场也有看到有人随身带着这本书
(我算是粗略地读过三遍左右，也确实大多数上面问到的问题都能在书中找到答案)
* 二面注重能力考查，无论是学习能力（问项目，问笔试收获），和后端的合作（问很多网络原理），还是编程能力（会问算法，听到有人说问二叉树翻转的）都有涉及。
面试官也明显更高端了，应该是Team Leader级别的
* 表达能力很重要，有些概念知道却没法清楚地表达出来，影响会很大
* 不要把话题引向自己不那么有自信的地方（比如上面的跨域问题）
