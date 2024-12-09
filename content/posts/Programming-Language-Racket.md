---
title: "Programming Languages Course Notes (Part Two)"
date: 2017-05-02
---

## Racket Section

Racket is a dynamically typed, functional language derived from Scheme.

> P.S. In fact, compared to Scheme, it does have classes and objects.

### Syntax

Racket's syntax is particularly distinctive, characterized by two main features: parentheses and prefix notation, with examples to follow.

In Racket, everything can be classified into two categories:

- Atomic Types (atoms):
  - Literals: `#t, 11, "hi", null`, etc.
  - Variable names: `x`
  - Keywords: `define, lambda, if`, etc.
- A sequence within parentheses
  - The first element in each sequence affects the subsequent elements
  - If the first element is not a keyword and the sequence is part of an expression, it is called as a function (including `+, -, *, /` which are all functions)
  - The entire sequence represents the corresponding abstract syntax tree with no ambiguity

### Delayed Evaluation and Thunks

A key semantic design in language: when are sub-expressions evaluated?

In Racket, ML, and most languages, when calling a function, argument expressions are evaluated before executing the function body. However, if an argument is a function, its body will not be executed until it is called.

```racket
(define (my-if-bad x y z) (if x y z))
; Regardless of x's truth value, y and z will be evaluated
(define (my-if x y z) (if x (y) (z)))
; When y and z are functions, only one will be evaluated
```

Functions used to delay computation are called `thunks`.

#### Lazy Evaluation and Memoization

Imagine a computationally expensive calculation where you're uncertain about using the result. In such cases, we can use a `thunk` to defer the computation.
> Some languages have built-in `call-by-name` mechanisms, like Scala

If the result is used, you might not know if it will be used multiple times. Even with a `thunk` delaying computation, each time the result is needed, the same calculation would be performed. (This assumes no side effects) To solve this, we need to record whether the computation has been executed and its result. (Like ML, Racket also allows `mutation`)
> In some languages like Haskell, there's a built-in mechanism where parameters are either not evaluated or evaluated only once, called `call by need`. Most common languages use `call by value`.

#### Streams

How can we generate an infinite sequence that cannot be explicitly produced in space or time? We can describe it recursively and write methods to obtain the parts we need. This approach is called a stream, with Linux pipes being a typical example.

```racket
(define ones (lambda () (cons 1 ones))) ; Generates infinite 1s
; Each time, obtain a value and a thunk for generating its successor
```

### Macros

Macros allow language users to define custom syntax, effectively extending the language. Macro expansion occurs at the very beginning of program execution, before type checking, compilation, and runtime.

Racket VS C/C++:

- Tokenization: Not simple replacement, but distinguishing words. For example, if defining a macro for head as car, 'headt' won't be replaced with 'cart'
- Parenthesization: C/C++ requires many parentheses to ensure correct macro replacement. Racket avoids this because parentheses represent syntax sequence boundaries.
- Scope: Racket doesn't perform macro replacement at variable definition
- Hygiene:
  - Local variables defined in a macro don't affect external variables with the same name

```racket
(define-syntax double4
(syntax-rules ()
    [(double4 e)
    (let* ([zero 0]
            [x e])
    (+ x x zero))]))

(let ([zero 17])
(double4 zero)) ; Result is 34
```

  - Local variables defined in a macro always point to the environment where the macro is defined, not the environment where the macro is called

```racket
(define-syntax double
(syntax-rules ()
    [(double e)
    (let ([x e])
    (+ x x))]))

(let ([+ *])
(double 17)) ; Result is 34
```

### Eval: Executing Code

Many languages provide functions to execute their own code, such as JavaScript and Python. Racket's difference is that it represents code to be executed as a list, not a string.

```racket
(define my-code
  (list '+ 5 3)) ; '+ is a Symbol

(eval my-code) ; 8
```

- [ML Section]({{< ref "Programming-Language-ML" >}})
- [Racket Section]({{< ref "Programming-Language-Racket" >}})
- [Ruby Section]({{< ref "Programming-Language-Ruby" >}})
- [Summary]({{< ref "Programming-Language-Summary" >}})
