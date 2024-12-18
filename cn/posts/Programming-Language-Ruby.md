---
title: "编程语言课程笔记（三）"
date: 2017-05-03
---

## Ruby 部分

Ruby 有如下重要特性

- 纯面向对象(pure OOP)：Ruby 中的所有值都是对象，每个表达式执行后都得到一个对象（有些面向对象语言不是这样，比如 Java 中的整型、布尔型、浮点数字面量都不是对象）
- 基于类的面向对象(class-based)：每个对象都是类的实例。类决定了对象所拥有的方法。（有些语言也不是基于类的，典型的如 JavaScript 是基于原型的）
- 混入(mixin)：多重继承（C++）与接口（Java）之间的一个折中。Ruby 中每个类都只有一个父类，但是可以有任意数量的混入，不像接口，它们可以定义方法（而不只是要求某个方法要存在）。
- 动态：Ruby 允许在任意对象上以任意参数调用任意方法，还可以在运行时改变类和对象的属性
- 反射(reflection)：可以在运行时知道对象的类是什么，有哪些方法
- 闭包（block and closure）：block 几乎就是闭包并且可以方便地用来使用库中的高阶函数，由于众多迭代函数的存在，Ruby 中也几乎不会显示使用循环。Ruby 中也有完整的闭包（proc）
- 脚本语言（script language）：脚本语言没有一个精确的定义，它意味着能够快速地写出简短的程序，方便地操作文件和字符串，较少考虑性能
- 流行于 Web 应用开发：Ruby on Rails 是一个非常流行的服务端框架

### Class-Based OOP 基于类的面向对象

面向对象有如下几条规则：

1. 所有变量都是对象的引用
2. 给定一个对象，可以通过调用它的方法与之交流，也可以被称为发送消息
3. 每个对象都有其私有的状态，只有对象自身的方法才能访问和修改状态
4. 每个对象都是类的实例
5. 类决定了对象的行为，类包含了方法的定义，它决定了对象如何处理它收到的调用（消息）

在另一些面向对象语言比如 Java/C# 中，大部分的规则都被实现了，然而 Ruby 给出了一个更完整的实现。
例如：

  1. 一些字面量在 Java/C# 中不是对象，违反了规则1
  2. Java/C# 中有 `public` 的属性，违反了规则3

#### Calling Methods 调用方法

```ruby
e0.m(e1, e2, ...)
```

与其他语言类似，首先得到表达式 `e0, e1, e2…` 的结果 `obj0, obj1, obj2, …` ，然后以 `obj1, obj2, …` 调用 `obj0` 的方法 `m`。方法调用另一种普遍的说法是消息传递，即向 `obj0` 传递了消息 `m`，附带 `obj1, obj2, …` 为参数，这种说法“更面向对象”，因为我们不关心接受者 `obj0` 是怎样实现的，只关心它能够接受并处理消息

#### Instance Variables 实例变量与类变量

一个对象有对应的定义了方法的类，也有它自己的实例变量，可以持有值。这在 Java/C#/C++ 中称为域（fields）（或者属性？），不同的是，在 Ruby 中没有专门的声明一个对象所拥有的实例变量，当需要添加一个实例变量的时候，只要相应地赋值就可以。对应的，Java/C#/C++ 中有静态方法与静态变量，Ruby 中也有类变量与类方法，即被所有该类的实例共享。

```ruby
# instance varible
@foo # getter
@foo= newValue # setter, will be created if not exist
# class varible
@@foo
```

### Duck Typing 鸭子类型

> If it walks like a duck and quacks like a duck, then it’s a duck.

言外之意是，没有理由去考虑那究竟是不是一只鸭子。在 Ruby 中，只要对所有消息的反应符合预期（比如会叫、会走），消息接受者（调用方法的对象）的类（比如 `Duck` 类）就不重要。面向对象更关心的是，能够“做什么”，而不在于“是什么”。

鸭子类型利于代码的重用，使得客户端可以使用“假”鸭子而仍然使用你的代码。但它的缺陷在于，我们只知道某个方法的实现，尤其是要传递什么消息给什么对象。比如，如果假定 `i` 是一个数，在没有被重写的情况下，我们知道 `i + i` 可以替换成 `i * 2` 或者 `2 * i`，但是如果只假定 `i` 有一个名叫 `+` 的方法，它可以接受自身作为参数，那我们就无法做出上述替换，因为我们不知道 `i` 有没有 这个方法，也不知道 `2` 有没有 这个方法，是不是能接受 `i` 作为参数。

### Override and Dynamic Dispatch 重写与动态分配
TODO

### Mixin 混入
TODO

- [ML 部分]({{< ref "Programming-Language-ML" >}})
- [Racket 部分]({{< ref "Programming-Language-Racket" >}})
- [Ruby 部分]({{< ref "Programming-Language-Ruby" >}})
- [总结部分]({{< ref "Programming-Language-Summary" >}})
