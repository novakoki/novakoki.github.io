---
title: CSS中外边距层叠的问题
date: 2016-03-31
locale: zh_CN
---

what:
----

Q:


```html
<div></div>
<p></p>
```





```css
div {
  position: fixed;
  height: 35px;
}

p {
  margin-top: 15px;
}

```

问div距离窗口上边的距离是多少？


A: 15px



why:
----

1. 在没有指定left，top等的情况下，它们的默认值为auto，意味着div的位置应该放在其静态位置，
即在其正常文档流中的位置。
2. 那么问题转变为，div的静态位置应该在哪？这里涉及到一个外边距层叠的问题，p元素和body有15px的margin，
在没有清除样式的情况下，浏览器默认body与html之间有不定大小的margin，这两个垂直距离上的margin会发生层叠，
即最终body与html之间有了15px的margin，所以div就和窗口上边有了15px的margin


how:
----

BFC: [深入理解BFC和Margin Collapse](http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)


[如何解决外边距叠加的问题？](https://www.zhihu.com/question/19823139)
