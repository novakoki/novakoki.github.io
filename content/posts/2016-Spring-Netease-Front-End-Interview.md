---
title: Netease Front-End Summer Internship Interview Experience (Spring 2016)
date: 2016-04-15
locale: en_US
---

Written Test
---

Multiple-choice questions omitted

Essay Questions:

1. Implement a native JavaScript interface that can upload files via Ajax, display upload progress, and receive JSON data from the server upon completion
2. Implement a three-column layout
3. How to prevent CSRF (Cross-Site Request Forgery)
4. List methods to reduce HTTP requests and resource file size
[Reference](https://csspod.com/frontend-performance-best-practices/)
5. List methods for cross-domain requests

First Interview
--

Started with self-introduction, during which the interviewer reviews the resume

Q: Are you more familiar with CSS or JS?

A: JS

Q: What parts comprise browser-side JavaScript?

A: ECMAScript + DOM

Q: What objects are in the DOM?

A: Node object, and inherited objects include Document, Element, Text, and some I can't recall

(Common objects: Node (Document, Element, CharacterData (Text, Comment, CDATASection), DocumentFragment), Window, Attr, NodeList, Event, etc.)

Q: What are JavaScript's basic types?

A: Number, String, undefined, null, and reference types

(I missed Boolean, and I thought the question was about data types. ES6 added Symbol)

Q: What's the difference between primitive and reference types?

A: When assigning, primitive types are copied by value, reference types by reference. Primitive types create a copy, reference types create a new pointer. Function parameters are always passed by value.

Q: 
```javascript
var obj = {a : 1};
(function (obj) {
    obj = {a : 2};
})(obj);
// What happens to obj?
```

A: The outer obj remains unchanged because the local obj is redirected to a new object

(Questions following because my resume mentioned C++)

Q: How are reference types in C++?

A: In C++, a reference is like an alias for a variable; operations on the reference also affect the original variable

Q: What are the differences between JS and C++?

A: Different object-oriented approaches: C++ uses class-based inheritance, JS uses prototype chain. JS is stronger in functional programming. JS lacks some C++ advanced features like templates and generics.

Q: Implement a two-column layout with a fixed-width left column and a flexible right column, using both float and flex methods

A:
```html
<div class="root">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

```css
.left {  float: left;  width: 320px; /* specified by interviewer */ }

/* I initially missed this part, which prevents right-side flexibility */
.right {  margin-left: 320px; }

/* flex solution (struggled to write initially) */
.root {  display: flex; }
.left {  width: 320px; }
.right {  flex: 1; }
```

Q: What position attributes exist? What are their characteristics?

A:
* static: Normal document flow
* relative: Positioned relative to its normal position
* absolute: Positioned relative to the first non-static parent element
* fixed: Positioned relative to the browser window

Q: Draw the standard box model

A: (Felt like the interviewer was questioning my CSS knowledge)

Q: What is a closure? What is it used for?

A: (Explained unclearly) Essentially a function within a function that can preserve variables

Q: In ES5, besides functions, what else can create a scope?

A: Objects?
(Asked if objects have scope, I mentioned this about 'this', completely mixing up scope and execution context)

(with and catch can create scopes)

Q: What are the different ways to call a function?

A:
* Direct call
* As an object method
* apply, call

Q:
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
What changes to **a**? What is **this** in the anonymous function? How to change **this**? How to change **obj.a** without passing parameters?

A: obj.a remains unchanged, the anonymous function's this points to the global object (window), essentially adding an 'a' property to window.

```javascript
(function () {
    this.a = 2;
}).call(this);
// or apply
// or
func: function () {
    var self = this;
    (function () {
        self.a = 2;
    })();
}
```

Q: Describe the event model? What is IE's event model? What is event delegation? How to locate the actual event target in event delegation?

A: Capture -> Target -> Bubble, IE probably only has bubbling without capturing.
Event delegation is handling events on a parent element, locating the actual target through the event object's target.

Q: What JavaScript animation methods exist?

A: Not very sure

(setTimeout/setInterval, requestAnimationFrame)

Q: Do you know other animation implementation methods?

A: CSS3 animation, and maybe canvas?

[CSS Versus JavaScript Animations](https://developers.google.com/web/fundamentals/design-and-ui/animations/css-vs-javascript)

Q: What frameworks or libraries have you recently used?

A: Used React Native for Android, still learning

Q: What are your learning channels?

A: MDN, Udacity, Imooc (Chinese online learning platform)

Q: Have you used Node.js?

A: Yes, mainly for tools like gulp

Q: What code management tools have you used?

A: (Interviewer saw GitHub on resume and didn't pursue further)

Q: Do you have any questions?

A: Asked about the department's focus (technical personnel or general users), received a response that they work on both

Interviewer said to take a break, with another interview coming up (surprisingly passed the first round)

Second Interview
--

Started directly by reviewing project experience from the resume

Q: Tell me about your projects and what you learned from them

A: (Felt nervous, explanation was not very clear)

Q: What's the difference between function declaration and function expression?

A: Function declarations are hoisted to the top, becoming global functions. Function declarations require a name, while function expressions can be anonymous.

Q: What is the scope chain?

A: (Explanation was not very clear)

Q: What are the characteristics of object-oriented programming?

A: Inheritance, polymorphism... couldn't recall the rest

(Encapsulation, inheritance, polymorphism, abstraction)

Q: How does JS implement inheritance?

A: Assign an instance of the parent object to the child object's prototype

Q: How to determine if a property comes from the object itself or the prototype chain?

A: hasOwnProperty

Q: How to find the middle point of a doubly-linked list?

A: Move head and tail pointers towards the center, stopping when they meet or alternate

Q: For a singly-linked list?

A: First traverse to the end to count elements, then go halfway
(After the interview, found out about using fast and slow pointers: one pointer moves one step, the other two steps; when the fast pointer reaches the end, the slow pointer is at the midpoint)

Q: What have you learned since the last written test?

A: Learned about cross-domain security and performance issues

Q: Describe cross-domain security issues

A: (Rambled and ultimately couldn't explain clearly, mentioned reading "HTTP: The Definitive Guide" after the test)

(CSRF (Cross-Site Request Forgery): Tricking users to access sites with stored credentials. Check Referer header or add token validation to URL)

Q: How to implement cross-domain requests?

A: JSONP, custom origin HTTP header

(JSONP, CORS, iframe (window.name or document.domain), postMessage)

Q: Is just writing origin sufficient?

A: Not sure

Q: Explain TCP three-way handshake

A: Client sends SYN, server responds with ACK, client responds with another ACK

Q: Which HTTP headers are related to caching?

A: ETag, cache-control... couldn't recall others

Q: What's the difference between cookie and session?

A: Cookies are on the client-side, sessions on the server-side

Q: What parts of a cookie are sent when the browser sends a cookie?

A: Not sure

(value)

Q: Do you know the components of a cookie?

A: Not sure

(name, value, domain, path, expires/max-age, size, http-only, secure, samesite)

Q: Have you used developer tools to view cookies?

A: Yes

Q: So what are the cookie components?

A: (Internally devastated)

Q: I have no more questions. Do you have any?

A: (Devastated, somehow asked for the cookie components answer... likely perceived as overly demanding)

After the interview, went to the front desk and was informed I didn't pass the second round, then happily (not really) returned to school

Summary
--

* First interview focused on foundational knowledge. Some interviewers recommend reading "JavaScript: The Definitive Guide" multiple times; some candidates were seen carrying this book
(I've roughly read it three times, and most questions could indeed be answered from this book)
* Second interview tested comprehensive abilities, including:
  - Learning capacity (asking about projects and test insights)
  - Backend collaboration (many network principle questions)
  - Programming skills (algorithm questions, heard some were asked about binary tree inversion)
  The interviewer seemed more senior, likely a Team Leader

* Communication skills are crucial. Knowing concepts but being unable to articulate them clearly significantly impacts performance
* Avoid steering conversations towards areas where you lack confidence (like the cross-domain issue discussed)
