---
title: "Programming Languages Course Notes (Part One)"
date: 2017-05-01
---

## Introduction

These notes are compiled from my studies of the Programming Languages course offered by the University of Washington. The course primarily introduces programming language concepts by exploring three different languages, examining how various language characteristics influence program writing. For developers already familiar with common languages like Java/C/C++/Python/JavaScript, this course offers broader insights into language expressiveness and limitations.

The course is currently available on Coursera: [https://www.coursera.org/learn/programming-languages](https://www.coursera.org/learn/programming-languages)

These notes won't delve deeply into basic syntax, but will focus on significant features. For additional reference, you can check the course lectures and assignments: [https://github.com/novakoki/Programming-Language](https://github.com/novakoki/Programming-Language)

### Categories of Programming Languages

The professor categorizes languages based on their primary characteristics:

||Functional|Object-Oriented|
|-|-|-|
|Static|ML|Java|
|Dynamic|Racket|Ruby|

### Essential Parts of Programming Languages

- Syntax
- Semantics
- Idioms: Common patterns leveraging language features, such as C++'s RAII
- Libraries: Readily available libraries and functions difficult to implement from scratch
- Tools: Compiler, REPL (Read-Eval-Print Loop), Debugger, etc.

Let's take a closer look at the difference between syntax and semantics. Often, people claim that a language's new feature is merely "syntactic sugar" without substantial impact.

## ML Section

### Static Type

ML is a statically typed language, meaning type checking occurs before runtime.

```sml
val x = e (* Data binding, where e is an expression *)
```

We want to understand not just the syntactic description, but the underlying semantics, including type checking and evaluation.

```sml
val x = 1 (* x is of type int, value is 1 *)
(* Each data binding updates the current environment (scope), which will be discussed in detail later *)
val y = x + 2 (* y is of type int, value is 3 *)
val z = y + "3" (* Type checking fails, + cannot be used between string and int *)
val z = w (* Type checking fails, w is not found in the current environment *)
```

#### Type Inference

ML is a statically typed language that rarely requires explicit type specification, thanks to its powerful type inference capabilities.

```sml
val x = 1 + 1 (* x is type int, because + operator returns int *)
fun return_self xs = xs (* Cannot precisely determine function type, so type is 'a -> 'a, where 'a is any type *)
(* For example, length function returns a list's length, its type is 'a list -> int *)
```

A type inference process example:

```sml
fun f (x, y, z) =
  if true
  then (x, y, z)
  else (y, x, z)
(*
1. Let x's type be T1, y be T2, z be T3, return type be T4
2. If statement's two branches must have consistent types, i.e., T4=T1*T2*T3=T2*T1*T3, yielding constraint T1=T2
3. Final function type is 'a*'a*'b -> 'a*'a*'b, meaning x and y must have the same type, while z can be of any type
*)
```

#### Option

Option provides a way to handle boundary cases, where an operation might yield a desired result (SOME) or no result (NONE).

Example of finding the maximum value in an int list:

```sml
(* Without Option *)
fun find_greatest (xs: int list) =
  if null xs (* Considering empty list boundary case *)
  then 0 (* Returning 0 is a poor style *)
  else greatest_number

(* With Option *)
fun find_greatest (xs: int list) =
  if null xs then NONE (* No result, cannot find max value *)
  else SOME (greatest_number) (* Wrap result in Option *)
```

### Immutability

One of the most important features of functional languages is that data is immutable by default.

```sml
val x = 1 (* Once bound, x's value cannot be changed *)
x = 2 (* In ML, this compares equality, not assignment *)
val x = 2 (* New data binding, previous x still exists but is shadowed *)
```

Advantages:

- Eliminates reference (alias) hazards. If data can't be modified, whether a "variable" is a reference becomes irrelevant.
- Data structures can share storage, saving memory overhead.

```sml
val x = [1, 2, 3]
val y = [4, 5, 6]
val z = x::y (* z is a concatenation of x and y lists *)
(* With immutable data, modifying x or y won't affect z, 
   and z can even share x and y's memory space *)
```

- Beneficial for parallel and concurrent programming

With immutable data, focus shifts from typical CRUD operations to construction and deconstruction.

### Building New Types

Languages typically provide basic types like `int, bool, float`, etc.

They also offer ways to construct custom types.

Custom types can be categorized into three kinds:

1. Each Of: Data type containing multiple type values, like tuple `(1, "string")` of type `(int * string)`
   ML uses `Records` to represent such types

```sml
val my_records = {bar: true, foo: 1, baz: (1, 2)}
(1, 2, 3) = {1: 1, 2: 2, 3: 3} (* Tuples are a special kind of Records *)
```

2. One Of: Data type containing one of multiple types, like `int option`
   ML uses `Datatype` to represent such types

```sml
datatype mytype = TwoInts of int * int
                | Str of string
                | Pizza
(* TwoInts, Str, Pizza are type tags, also functions for construction and deconstruction *)

val mydata = TwoInts(1, 2) (* mydata is of type mytype *)

datatype my_int_option = SOME of int
                      | NONE
```

3. Self Reference: Recursive data types, like `int list` which can be empty or a combination of an `int` and another `int list`

```sml
datatype my_int_list = Empty
                    | Cons of int * my_int_list

[1,2,3,4,5] = [1, [2, [3, [4, [5, []]]]]] (* Like Russian nesting dolls *)
```

### Pattern Matching

Pattern matching provides powerful and elegant capabilities for extracting data from custom data types, essentially a deconstruction mechanism.

#### Case Expressions

```sml
fun getOrElse op =
  case op of
      NONE => 0
    | SOME x => x (* Extracting the wrapped value from Option *)

fun find_greatest xs =
  case xs of
    head::tail => (* Extracting the list head *)
    head::(body::tail) => (* Nested Pattern, matching lists with at least one element *)

(* Deconstructing from previous Datatype *)
  case mydata of
    TwoInts(a, b) => a + b (* Extracting a = 1, b = 2 *)
  | _ => 0
(* TwoInts serves both as a constructor and extractor *)
```

#### Function Parameter Deconstruction

```sml
fun is_greater (x, y) = (* Pattern matching occurs here. All ML functions are single-parameter functions, actually taking a tuple (x, y) *)
  x > y (* Within the function body, values are already extracted from the tuple *)
(* Another ML method for multi-parameter functions is Currying *)

(* Let and Val Deconstruction *)
fun sum_triple (triple: int * int * int) =
  let val (x,y,z) = triple (* Extracting values from the tuple *)
  in x + y + z
  end
```

### Tail Recursion

Most functional languages lack the procedural loop statements common in imperative languages, instead using recursion to achieve similar results.

> This is an "idiom" brought about by immutable data, where traditional iterator-based loops become impossible

```sml
fun sum1 xs =
  case xs of
    [] => 0
  | head::tail => head + sum1 tail (* Bad style prone to stack overflow *)

(* Maintains an O(n) call stack
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
    | head::tail => helper(tail, acc + head) (* Tail recursion *)
  in helper(xs, 0)
  end

(* With tail recursion optimization, no need to maintain entire call stack, space complexity O(1)
  helper([1,2,3], 0)
  helper([2,3], 1)
  helper([3], 3)
  helper([], 6)
  6
*)
```

### Functions as First-Class Values

Another crucial feature of functional languages is the ability to pass functions as values - functions are first-class citizens.

#### Functions as Arguments

Functions that take other functions as arguments are called higher-order functions, which enable better code abstraction and reuse. (An idiom)

A typical higher-order function is `map`.

```sml
fun map (f, xs) =
  case xs of
      [] => []
    | x::xs' => (f x)::map(f, xs')

map(fn x => x * 2, [1,2,3]) (* Produces [2,4,6] *)
```

#### Lexical Scope and Closure

> Function (closure) = Code (function body) + Environment (lexical scope)

A function can access not just its parameters and internally defined variables, but also variables from its lexical scope.

Examples include callbacks, currying, and function composition.

(The extensive literature on closures is beyond the scope of this document)

### Equivalence and Side-Effect-Free Implementations

Separating interface definition from implementation is a critical programming practice. ML uses modules to manage namespaces, separate interfaces from implementations, and hide private methods.

```sml
(* Interface Definition *)
signature RATIONAL_A =
sig
type rational
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end

(* Implementation *)
structure Rational_1 :> RATIONAL_A =
struct
datatype rational = ...
fun make_frac (x, y) = ...
fun add (r1, r2) = ...
fun reduce r = ... (* Private module method *)
fun toString r = ...
end

(* Invocation *)
Rational_1.make_frac(1, 2) (* Rational_1 is a namespace *)
```

The same interface can have different implementations. A function can have various implementations that present identical behavior to the caller, implying the concept of equivalence.

When considering equivalent implementations, consider:

- Maintenance: Simplifying or reorganizing code without changing other code's behavior
- Backward Compatibility: Adding new features without altering existing functionality
- Performance Optimization: Providing more efficient time or space implementations
- Abstraction: Would clients using this interface notice internal implementation changes?

Different equivalence definitions:

1. Language-level Equivalence: Same input produces same output (including side effects, exceptions, termination)
2. Asymptotic Equivalence: Algorithmic complexity remains the same, considering only sufficiently large inputs
3. System-level Equivalence: Constant-level performance considerations

Eliminating side effects is the simplest way to ensure implementation equivalence. Functions without side effects are called pure functions. Side effects include modifying references, input/output operations, etc.

> While ML allows reference value modifications, it's not a purely functional language. Haskell represents a typical purely functional language.


- [ML Section]({{< ref "Programming-Language-ML" >}})
- [Racket Section]({{< ref "Programming-Language-Racket" >}})
- [Ruby Section]({{< ref "Programming-Language-Ruby" >}})
- [Summary]({{< ref "Programming-Language-Summary" >}})
