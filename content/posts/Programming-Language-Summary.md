---
title: "Programming Language Course Notes (Final)"
date: 2017-05-04
---

## Static vs Dynamic Comparison

A language is termed static or dynamic based on when type checking occurs.

### ML vs Racket

There are two perspectives:

1. ML is somewhat a subset of Racket; Racket supports more ways of writing code because some expressions allowed in Racket would not pass ML's static checks. For instance, an if statement with branches returning different types.
2. Variables in Racket are not "typeless" but have "one type".

```sml
datatype theType = (* All variable types are the same, so no need to annotate or check *)
  Int of int | (* Runtime values have different tags *)
  String of string |
  Pair of theType * theType |
  Fun of theType -> theType |
  ...
```

### Static Checking

Static checking occurs after syntax analysis and before program execution, hence called "compile-time checking" (though this is unrelated to whether the language uses a compiler or interpreter). How static checking works is part of the language definition. Some languages do more, some do less. Given checking rules, you can also implement tools to perform desired checks.

The primary method of implementing static checking is through type systems, but the type system is just a means, not the purpose of static checking. The goal of static checking is error prevention, but this error range is limited. Most static checks won't verify array bounds, and they can't tell you where you should use multiplication instead of addition. What they can prevent are errors like dividing a string by an integer or preventing a type from performing certain operations.

Compared to dynamic checking (adding tags to values and checking at runtime), static checking actually rejects some programs that wouldn't actually cause errors. The interesting discussion here is how to define an "error".

Consider a simple "error": `3 / 0`. You could prevent this "error" at various points:

- Keystroke-time: Preventing it while editing the code
- Compile-time: Preventing it when the static checker "sees" the code
- Link-time: Preventing it when discovering the code will be called
- Run-time: Preventing the division by zero operation
- Even-later
  - The code execution has no error, but using the result (which could be an `undefined` identifier) is an error
  - Even calculating with the result might not be an error. `3 / 0` could represent positive infinity, just like `tan(π/2)`. You can let it participate in calculations (in fact, Racket's `(/ 3.0 0.0)` returns `+inf.0`)

The second point worth discussing is how to define correctness and judge whether type checking complies with the language definition.

### Soundness and Completeness

Let `X` be an event we want type checking to prevent. Defining a type system involves:

- Soundness: Not accepting programs where `X` could happen for some input
- Completeness: Not rejecting programs where `X` would not happen for any input

Modern language type systems are sound but not complete. Soundness means programmers can rely on `X` not happening. Completeness would be nice, but cases of type system misdiagnosis are rare and easily modified to pass type checking.

Why don't modern languages achieve completeness? Static checking cannot simultaneously satisfy:

- Program termination
- Soundness
- Completeness

If one must be discarded, sacrificing completeness is a reasonable choice.

>（Why can't they be simultaneously satisfied? Due to undecidability, which I can't fully explain）

### Weak Typing

If a type system is unreliable for some `X`, at least it should prevent the behavior at runtime. Another option is not preventing the behavior, treating it as a programmer's error that the language need not check. In such cases, unpredictable behaviors might occur, and languages allowing this are called weakly typed. Typical weak-typed languages are C and C++, with array out-of-bounds being a classic `X` behavior. Strong-typed languages like Java throw exceptions at runtime, but weak-typed languages won't prevent such behaviors.

Why did C/C++ design it this way?

- Close to the hardware, prioritizing efficiency. Error checking requires additional time and space, and programmers don't want hidden time/space overhead
- "Strong types for weak minds" - believing humans are smarter than computers and definitely write correct code. Experience proves this thinking wrong; computers are far more reliable

### Flexibility

Beyond different error-checking times, languages treat similar behaviors differently, ranging from strict to flexible. What some languages consider errors, others might not, giving seemingly erroneous behaviors correct meanings.

- Array bounds: Some languages have no out-of-bounds concept, expanding the array when an out-of-bounds index is used. Some allow negative indices from the end.
- Function arguments: Some languages are indifferent to argument count, ignoring extra arguments or using default values for missing ones
- Implicit conversions: Some languages allow adding strings to numbers

(The above examples are not meant to criticize JavaScript)

These features might be unwise, potentially hiding errors and making debugging difficult. However, this flexibility can sometimes be useful. This is unrelated to whether the language is static or dynamic; while it might seem these flexibilities make a language more "dynamic", they merely change the semantics by not preventing certain behaviors.

### Static or Dynamic

Returning to the core question: should you choose a static or dynamic language? What are their pros and cons? (There's never a definitive answer that static is always better than dynamic, or vice versa)

- Convenience? Dynamic languages without type constraints express things more freely, but static languages with defined types can reduce type-checking code
- Do static languages prevent useful programs? Programs blocked by type systems can often be simulated in dynamic languages through runtime checks or in static languages using ML's datatype or object-oriented polymorphism
- "Premature" error checking? A software engineering truth is that earlier bug detection is easier. Static checks help catch low-level errors, letting programmers focus on business logic. Dynamic language advocates argue that some errors remain undetectable by static checks and require testing
- Performance? Dynamic languages have tagging overhead but can be optimized; static languages also sometimes need tags
- Code reuse? Dynamic languages make code reuse easier but also more error-prone
- Prototype design? Dynamic languages don't require type design and allow partial implementation; static languages must pass type checks but can use wildcards to express undesigned parts
- Code maintenance? Dynamic languages make it easy to add function parameters without user awareness. Static languages' type-checking tools highlight all related areas that should be modified when a change is made

## OOP vs Functional Comparison

### Multiple Inheritance, Mixin, Interface

TO BE DONE…

- [ML Section]({{< ref "Programming-Language-ML" >}})
- [Racket Section]({{< ref "Programming-Language-Racket" >}})
- [Ruby Section]({{< ref "Programming-Language-Ruby" >}})
- [Summary Section]({{< ref "Programming-Language-Summary" >}})
