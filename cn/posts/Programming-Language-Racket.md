---
title: "编程语言课程笔记（二）"
date: 2017-05-02
---

## Racket 部分

Racket 是一门由 Scheme 而来的动态类型、函数式的语言

> P.S. 其实比起 Scheme，它有类和对象

### Syntax 语法

Racket 在语法方面比较特别，有两个特点：括号和前缀表达，例子见后文。

Racket 里的所有东西都可以分为两种情况：

- 原子类型 (atom)：
  - 字面量： `#t, 11, “hi”, null`, etc.
  - 变量名：`x`
  - 关键字： `define, lambda, if`, etc.
- 一个在括号中的序列
  - 每个序列中的第一个元素会对后面的元素产生作用
  - 如果第一个元素不是关键字且整个序列是表达式的一部分，那就把它作为函数来调用 （包括 `+, -, *, /` 也都是函数）
  - 整个序列表示了对应的抽象语法树且没有二义性

### Delayed Evaluation And Thunk 延迟计算

语言设计上的一个关键语义：子表达式什么时候被求值

对于 Racket, ML 以及大部分语言来说，当调用函数时，传入的参数表达式在执行函数体前被求值。但如果这个参数是一个函数，那在被调用之前，函数体的代码是不会被执行的。

```racket
(define (my-if-bad x y z) (if x y z))
; 无论 x 真假，y 和 z 都会被求值
(define (my-if x y z) (if x (y) (z)))
; 当 y 和 z 是函数时，只会求值其中一个
```

这种用来延迟计算的函数称为 `thunk`

#### Lazy Evaluation And Memoization

假设有一个很耗时的计算，但是并不知道最终会不会用到这个结果。这时我们可以用`thunk`来延迟这个计算。
> 一些语言有内建的`call-by-name`机制，比如 Scala

假设用到了这个结果，也并不知道是不是会多次使用。即使用了`thunk`来延迟这个计算，每一次需要用到结果还是需要做一次相同的计算。（这里的前提是无副作用）为了解决这个问题，我们需要记录这个计算是否执行过，以及它执行得到的结果。（和 ML 一样， Racket 也是允许`mutation`的）
> 在一些语言中，比如 Haskell，有内建的这种机制，即参数要么不被求值，要么只被求值一次，称为`call by need`。一般常见的语言都是`call by value`。

#### Stream 流

如何产生一个无穷的序列，无论在空间上还是时间上都无法做到显式产生，但是我们可以用递推的方式描述它，以及编写方法来取得我们需要的部分。这样的方法称为流，典型的流就是 linux 的管道。

```racket
(define ones (lambda () (cons 1 ones))) ; 产生无限个 1
; 每次得到一个值和产生它后继的 thunk
```

### Macros 宏

宏允许语言的使用者自定义语法，即可以扩展语言。宏展开在程序最开始执行，即在类型检查、编译、运行之前。

Racket VS C/C++：

- Tokenization： 不是简单的替换，而是会区分单词。比如定义 head 为 car 的宏，headt 不会被替换成 cart
- Parenthesization： C/C++ 里需要用很多括号来确保宏替换后的正确执行，Racket 没有这个问题，因为括号在 Racket 里意味着一个语法序列的边界。
- Scope： Racket 不会在变量定义处进行宏替换
- Hygiene：
  - 在宏里面定义局部变量不会影响外部的同名变量

```racket
(define-syntax double4
(syntax-rules ()
    [(double4 e)
    (let* ([zero 0]
            [x e])
    (+ x x zero))]))

(let ([zero 17])
(double4 zero)) ; 结果为 34
```

  - 在宏里面定义的局部变量永远指向宏定义处的环境，而非宏调用时的环境

```racket
(define-syntax double
(syntax-rules ()
    [(double e)
    (let ([x e])
    (+ x x))]))

(let ([+ *])
(double 17)) ; 结果为 34
```

### Eval 执行代码

很多语言都提供了执行自身代码的函数，比如 JavaScript, Python 都有。但 Racket 的区别在于，它以列表的形式表示要执行的代码而非字符串。

```racket
(define my-code
  (list '+ 5 3)) ; '+ 是一个 Symbol

(eval my-code) ; 8
```

- [ML 部分]({{< ref "Programming-Language-ML" >}})
- [Racket 部分]({{< ref "Programming-Language-Racket" >}})
- [Ruby 部分]({{< ref "Programming-Language-Ruby" >}})
- [总结部分]({{< ref "Programming-Language-Summary" >}})
