---
title: "编程语言课程笔记（一）"
date: 2017-05-01
---

## Introduction 背景简介

这份笔记是我学习华盛顿大学开设的《编程语言》时记录整理，这门课程主要通过介绍三门语言来讲授编程语言的方方面面，以及不同的语言特性对编写程序的影响。对于熟悉常见编程语言比如 Java/C/C++/Python/JavaScript 的开发者来说，这门课能够开阔眼界，从而了解语言的表达力和局限性。

目前该课程仍然在 Coursera 上有开设：[https://www.coursera.org/learn/programming-languages](https://www.coursera.org/learn/programming-languages)

笔记中不会过多介绍基本语法，只谈一些重要的特性。如有需要也可参考我学习时的课堂讲义和完成的作业：[https://github.com/novakoki/Programming-Language](https://github.com/novakoki/Programming-Language)

### Categories 编程语言分类

教授将语言按照主要的特性分成四类，其他的特性会穿插介绍

||函数式 (Functional)|面向对象 (Object-Oriented)|
|-|-|-|
|静态 (Static)|ML|Java|
|动态 (Dynamic)|Racket|Ruby|

### Essential Parts 编程语言组成部分

- 语法 (Syntax)
- 语义 (Semantic)
- 习惯（Idioms）: 常用的一些利用语言特性的套路，比如 C++ 的 RAII
- 库（Libraries）: 现成可以用的库，以及如操作文件这种没法自己实现的功能
- 工具（Tools）: 编译器（Compiler）, 命令行（REPL）, 调试器（Debugger）, etc.

这里重点看一下语法和语义的差别。经常看到有人会说某个语言的新特性只是语法糖而已，没有什么实质性作用。

## ML 部分

### Static Type 静态类型

ML 是一门静态语言，静态是指它在运行前进行类型检查

```sml
val x = e (* 称之为一次数据绑定， e 为一个表达式 *)
然而我们要看到的不仅仅是这样一句语法描述，还要了解背后的语义，即类型检查和求值。

val x = 1 (* x 的类型为 int，值为 1 *)
(* 每做一次数据绑定，当前环境都会更新，这个环境即作用域，后面会详细讨论 *)
val y = x + 2 (* y 的类型为 int, 值为 3 *)
val z = y + "3" (* 类型检查失败， + 不能用于 string 与 int 之间 *)
val z = w (* 类型检查失败，当前环境中找不到 w *)
```

关于静态与动态，之后将与 Racket 对比来看，这里只提一些 ML 的特性。

#### Type Inference 类型推导

ML 是一门静态语言，但却是一门大多数时候不显式指定类型的语言，这得益于它强大的类型推导能力。

```sml
val x = 1 + 1 (* x 的类型为 int，是因为 + 运算符的返回类型为 int *)
fun return_self xs = xs (* 不能精确地确定这个函数的类型，所以类型为 'a -> 'a，'a 为任意类型 *)
(* 再如 length 函数返回一个列表的长度，它的类型就为 'a list -> int *)

一个类型推导的过程例子如下

```sml
fun f (x, y, z) =
  if true
  then (x, y, z)
  else (y, x, z)
(*
1. 令 x 的类型为 T1， y 为 T2， z 为 T3，返回类型为 T4
2. if 语句的两个分支得到的类型必须一致，即 T4=T1*T2*T3=T2*T1*T3，得到约束 T1=T2
3. 最终函数类型为 'a*'a*'b -> 'a*'a*'b，即 x 和 y 的类型必须一致，而 z 可以是任意类型
*)
```

#### Option

Option 提供了一种处理边界情况的能力,
即运算得到的可能是某种你需要的结果 (SOME)，也可能没有结果 (NONE)。

比如写一个找出一个 int list 中最大值的函数

```sml
(* Without Option *)
fun find_greatest (xs: int list) =
  if null xs (* 考虑空列表这种边界情况 *)
  then 0 (* 这里如果返回 0 就是一种很不好的风格 *)
  else greatest_number
```

在一门静态语言中，函数返回值的类型是确定的，在没有 Option 的情况下只能返回一个特定的数

```sml
(* With Option *)
fun find_greatest (xs: int list) =
  if null xs then NONE (* 没有结果，即找不到最大值 *)
  else SOME (greatest_number) (* 有最大值，则包装成 Option 返回 *)
```

### Immutable 不可变

函数式语言最重要的特性之一就是数据默认是不可变的

```sml
val x = 1 (* 一旦绑定了就无法再改变这个 x 的值 *)
x = 2 (* ML 中没有赋值的概念，这个表达式比较是否相等 *)
val x = 2 (* 新的数据绑定，之前的 x 还在但是会被新的 x 遮蔽(shadow) *)
```

优点：

- 一个比较明显的好处是排除了引用（别名）的危害。
  如果数据不会被修改，那一个“变量”是否是引用就并不重要。
  甚至于数据结构的存储是可以共享的，以此节省空间的开销。

```sml
val x = [1, 2, 3]
val y = [4, 5, 6]
val z = x::y (* z 为 x 和 y 两个列表拼接而成 *)
(* 如果 x 和 y 是引用， 那么更改 x 或 y 都会使得 z 的值发生变化
但是如果数据是不可变的就没有这个问题，甚至在内存分配上 z 其实只要共享 x 和 y 的空间就可以了 *)
```

- 利于并行和并发。这个好处使得函数式语言（或者理念）焕发新生。

在数据不可变的基础上建立数据结构，所关注的不再是常见的增删改查，而是构造和解构，即如何构造新的数据结构和从数据结构中取得数据。

### Build New Types 自定义类型

一门语言中一般会提供一些基本类型，如 `int, bool, float`, etc.

同时也会提供构造自定义类型的能力

自定义类型可以分为下面三种

- Each Of : 即数据类型同时包含多个类型的值，比如元组 `(1, “string”)` 类型是 `(int * string)`
  ML 中使用 `Records` 来表示这样的类型

```sml
val my_records = {bar: true, foo: 1, baz: (1, 2)}
(1, 2, 3) = {1: 1, 2: 2, 3: 3} (* 元组是一种特殊的 Records *)
```

- One Of : 数据类型包含多个类型中的一个，比如 `int option`，可能包含一个 `int` 值，也可能没有
  ML 中使用 `Datatype` 来表示这样的类型

```sml
datatype mytype = TwoInts of int * int
                | Str of string (* | 是或的意思 *)
                | Pizza
(* TwoInts, Str, Pizza 相当于给某种类型加上了标签，同时它们也是函数可以用来构造与解构 *)

val mydata = TwoInts(1, 2) (* mydata 的类型为 mytype *)

datatype my_int_option = SOME of int (* Option 是一种特殊的 Datatype *)
                      | NONE
```

- Self Reference : 自指的类型，用于描述递归的数据类型，
  比如 `int list` 可能是空的，也可能是一个 `int` 值和另一个 `int list` 的组合（递归地定义）
  （事实上 `int list` 融合了以上三种）

```sml
datatype my_int_list = Empty
                    | Cons of int * my_int_list

[1,2,3,4,5] = [1, [2, [3, [4, [5, []]]]]] (* 类似俄罗斯套娃 *)
```

### Pattern Match 模式匹配

模式匹配提供了强大优雅的从各种自定义数据类型中取数据的能力，即解构的能力

#### case 表达式

```sml
fun getOrElse op =
  case op of
      NONE => 0
    | SOME x => x (* 从 Option 中解构出包装的值 *)

fun find_greatest xs =
  case xs of
    head::tail => (* 从列表解构出表头 *)
    head::(body::tail) => (* Nested Pattern，匹配至少有一个元素的列表 *)

(* 从上一节的 Datatype 中解构 *)
  case mydata of
    TwoInts(a, b) => a + b (* 解构出 a = 1, b = 2 *)
  | _ => 0
(* TwoInts 既是构造器也是提取器 *)
```

#### 函数参数解构

```sml
fun is_greater (x, y) = (* 这里已经发生了模式匹配， ML 所有的函数都是单参数的， 这里实际的参数是 (x, y) 这个元组 *)
  x > y (* 在函数体内取得的是已经从元组中解构出的值 *)
(* ML 中另一种实现多参函数的方法是柯里化 (Currying) *)
let, val 解构

fun sum_triple (triple: int * int * int) =
  let val (x,y,z) = triple (* 解构出元组中的值 *)
  in x + y + z
  end
```

### Tail Recursion 尾递归

大多数函数式语言中都没有过程式语言中常用的循环语句，而是通过递归的方式来实现。

> 这是一种 `idiom`，是由不可变数据带来的，因为没法实现用来计数的迭代器

```sml
fun sum1 xs =
  case xs of
    [] => 0
  | head::tail => head + sum1 tail (* bad style cause stack overflow *)

(* 需要维护一个 O(n) 的调用栈
  sum1 [1,2,3]
  1 + sum1 [2,3]
  1 + 2 + sum1 [3]
  1 + 2 + 3 + sum1 []
  1 + 2 + 3 + 0
  1 + 2 + 3
  1 + 5
  6
*)

fun sum2 xs =
  let fun helper (xs, acc) =
    case xs of
      [] => acc
    | head::tail => helper(tail, acc + head) (* tail recursion*)
  in helper(xs, 0)
  end

(* 尾递归优化下，不需要维护整个调用栈，空间复杂度 O(1)
  helper([1,2,3], 0)
  helper([2,3], 1)
  helper([3], 3)
  helper([], 6)
  6
*)
```

### Functions As Value 函数作为值传递

函数式语言另一个最重要的特性就是函数可以作为值来传递，即函数是一级公民。（First-Class Function)

#### Take Function As Argument 函数作为参数

以函数为参数的函数称为高阶函数，利用高阶函数可以更好地抽象与重用代码。（idiom）

典型的高阶函数就是 `map` 。

```sml
fun map (f, xs) =
  case xs of
      [] => []
    | x::xs' => (f x)::map(f, xs')

map(fn x => x * 2, [1,2,3]) (* 得到 [2,4,6] *)
```

#### Lexical Scope And Closure 词法作用域和闭包

> function (closure) = code (for function body) + environment (lexical scope)

即函数能访问到的变量不仅仅有参数和函数体内定义的变量，还有它的词法作用域中的变量。

例子：回调、柯里化、函数组合等等

（闭包这部分资料太多太多，不作具体展开了）

### Equivalence and Side-Effect-Free 等价实现和无副作用

接口定义和实现分离是重要的编程实践。ML 使用模块来管理命名空间，分离接口与实现，以及隐藏私有方法。

```sml
(* 接口定义 *)
signature RATIONAL_A =
sig
type rational
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end

(* 实现 *)
structure Rational_1 :> RATIONAL_A =
struct
datatype rational = ...
fun make_frac (x, y) = ...
fun add (r1, r2) = ...
fun reduce r = ... (* 模块私有方法 *)
fun toString r = ...
end

(* 调用 *)
Rational_1.make_frac(1, 2) (* Rational_1 即一个命名空间 *)
```

同一个接口可以有不同的实现，同样的一个函数也可以有各种不同的实现，但是它们对调用者表现出的行为一致，这里隐含着等价的概念。

当考虑等价实现的时候，可能有以下情况：

- 维护：在不改变其他代码行为的前提下简化、重新组织代码
- 向后兼容：在不改变已有功能的前提下增加新的特性
- 性能优化：给出时间或空间上更佳的实现
- 抽象：使用这个接口或者函数的客户端会感觉到内部实现的变化吗？

不同的等价定义：

- 语言层面的等价：同样的输入，得到同样的输出（包括产生同样的副作用、抛出同样的异常、同样终止或不终止）
- 渐进等价：即算法复杂度相同，只考虑输入足够大的情况下，不考虑常数级别
- 系统级别的等价：常数级别的性能也要考虑

要保证两个实现的副作用相同，最简单的方式就是消除副作用。没有副作用的函数称为纯函数，副作用包括改变数据(引用)、输入输出等。

> ML中是可以做改变引用的值这种事的，所以它不是一门纯函数式语言。一门典型的纯函数式语言就是Haskell

- [ML 部分]({{< ref "Programming-Language-ML" >}})
- [Racket 部分]({{< ref "Programming-Language-Racket" >}})
- [Ruby 部分]({{< ref "Programming-Language-Ruby" >}})
- [总结部分]({{< ref "Programming-Language-Summary" >}})
