---
title: 内存泄漏
date: 2015-02-04
locale: zh_CN
---

> 不能记住过去的人, 被迫重复过去.


这个标题似乎表示我正准备写一篇技术文章，可惜才疏学浅（操作系统都没学在这儿BB什么！），这只是一篇偷换概念的普通随笔而已。为什么说内存泄漏呢？因为我这个人已经实实在在处于内存泄漏状态了。


自从大一暑假以来，以下代码描述了我的真实状态




```cpp
typedef learning_area T;
while (found_new_interest){
    T* p = new learning_area(); //同学你的指针用一次就找不到了！
    p->learn_three_days();
    p->rest_two_days();    //同学你的delete去哪儿了？
} //同学你醒醒你醒醒
```


然后只能睡一觉重启了



细数一下有多少野指针：


* Web开发（包括HTML、CSS、JavaScript、PHP、Bootstrap…balabala）
* Python（现在也扯上Web了）
* Git（版本控制什么高大上的都没用过，纯粹在当代码仓库）
* Tex（为了以后写论文会用到吗？）
* 网络安全（XSS、sql注入、nmap、burp、密码字典暴力破解，好像都没成功过）
* 数据结构和算法分析（说好的大二上认真学算法的呢？）
* 机器人
* 图形图像(OpenCV大法？）
* 机器学习（寒假没事干看一看Andrew Ng的课）


再算上大一下的安卓，好的，内存已经溢出了。


好的，内存整理第一阶段终于算是完成了。


总结一下问题所在：


* 没有输出，内存不够用还有硬盘啊
* 每次都是三天打渔两天晒网的，没有一个能打的，别人问个问题秒秒钟倒下
* 输入范围太大，资源密集症，这真的是最好的时代，MOOC、文档、博客一堆堆，但对于选择来说也是极其困难的


好了，野指针又乱飞了。得重启下了，明天继续内存整理。

