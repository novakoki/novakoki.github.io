---
title: "Programming Language Course Notes (Part Three)"
date: 2017-05-03
---

## Ruby Section

Ruby has the following important characteristics:

- Pure Object-Oriented Programming (pure OOP): In Ruby, all values are objects, and every expression results in an object (unlike some object-oriented languages like Java, where primitive types like integers, booleans, and floating-point literals are not objects)
- Class-Based Object-Oriented Programming: Every object is an instance of a class. Classes determine the methods an object possesses. (Some languages are not class-based, such as JavaScript, which is prototype-based)
- Mixins: A compromise between multiple inheritance (C++) and interfaces (Java). In Ruby, each class has only one parent class, but can have any number of mixins. Unlike interfaces, mixins can define methods (not just declare method signatures)
- Dynamic: Ruby allows calling any method with any arguments on any object and can change class and object attributes at runtime
- Reflection: The ability to determine an object's class and its methods during runtime
- Closures (block and closure): Blocks are almost closures and can be conveniently used with higher-order functions in libraries. Due to numerous iteration functions, Ruby rarely uses explicit loops. Ruby also supports complete closures (proc)
- Scripting Language: While there's no precise definition, it implies the ability to quickly write short programs, easily manipulate files and strings, with less emphasis on performance
- Popular in Web Application Development: Ruby on Rails is an extremely popular server-side framework

### Class-Based Object-Oriented Programming

Object-oriented programming follows these rules:

1. All variables are references to objects
2. Communication with an object is done by calling its methods, also known as "sending messages"
3. Each object has its private state, which can only be accessed and modified by the object's own methods
4. Every object is an instance of a class
5. Classes determine an object's behavior by containing method definitions that decide how objects process received calls (messages)

In other object-oriented languages like Java/C#, most of these rules are implemented, but Ruby provides a more complete implementation. 

For example:
1. Some literals in Java/C# are not objects, violating rule 1
2. Java/C# have `public` attributes, violating rule 3

#### Calling Methods

```ruby
e0.m(e1, e2, ...)
```

Similar to other languages, first evaluate expressions `e0, e1, e2…` to get `obj0, obj1, obj2, …`, then call method `m` on `obj0` with `obj1, obj2, …` as arguments. Method calling is also commonly described as message passing—sending a message `m` to `obj0` with `obj1, obj2, …` as parameters. This phrasing is more "object-oriented" because we don't care about how the receiver `obj0` is implemented, only that it can receive and process the message.

#### Instance Variables and Class Variables

An object has a corresponding class that defines methods and its own instance variables that can hold values. This is similar to fields (or properties) in Java/C#/C++, but in Ruby, there's no explicit declaration of instance variables. When you need to add an instance variable, you simply assign a value to it. Correspondingly, Ruby also has class variables and class methods, shared by all instances of that class.

```ruby
# instance variable
@foo # getter
@foo= newValue # setter, will be created if not exist
# class variable
@@foo
```

### Duck Typing

> If it walks like a duck and quacks like a duck, then it's a duck.

This means there's no need to consider whether it's actually a duck. In Ruby, as long as an object responds to messages as expected (e.g., can quack, can walk), the object's class is irrelevant. Object-oriented programming is more concerned with "what can be done" rather than "what something is".

Duck typing facilitates code reuse, allowing clients to use "fake" ducks while still using your code. However, its drawback is that we only know a method's implementation, especially regarding what messages to send to what objects. For example, if we assume `i` is a number, without being overridden, we know `i + i` can be replaced by `i * 2` or `2 * i`. But if we only assume `i` has a `+` method that can accept itself as a parameter, we can't make such substitutions because we don't know if `i` or `2` have this method or can accept the other as a parameter.

### Override and Dynamic Dispatch
TODO

### Mixin
TODO

- [ML Section]({{< ref "Programming-Language-ML" >}})
- [Racket Section]({{< ref "Programming-Language-Racket" >}})
- [Ruby Section]({{< ref "Programming-Language-Ruby" >}})
- [Summary Section]({{< ref "Programming-Language-Summary" >}})
