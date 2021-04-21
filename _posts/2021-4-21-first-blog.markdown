---
layout: post
title:  "Introduction to PromiseKit"
date:   2021-04-21 23:40:16 +0600
categories: iOS
---

PromiseKit is a library that helps with asynchronous programming in swift and Objective-C. It is a wrapper around async task. Instead of using completion and error handler, a method can return a Promise. It is explained as “PromiseKit is a thoughtful and complete implementation of promises” in the official doc.
Advantages of using PromiseKit over traditional iOS async programming:
Cleaner and readable code
Quick learning curve
Very easy to call multiple async functions and managing it
Save us from Callbacks Pyramid of Doom. Nested callbacks are now in the chain.
Error Handling is separated. Duplicate multiple error handlings are in one place.
Example:

Common Syntax:
☐ then and done:
then and done are kinda like completion handlers. But the difference is that we can use them to chain multiple async calls or other operators like error handling. It is also great for readability.
Example:

☐ catch:
catch is closure for error handling. What is interesting here is that the catch closure catches all the errors of the whole chain.
Example:

☐ ensure:
ensure closure is called no matter the promise succeeds or fails. We should put “must be called” codes inside ensure clause.
Example:

☐ when:
Parallel multiple async calls are so much easier with promise. when takes multiple promises and wait for them to end and return a promise containing the results.
Example:

☐ map, compactMap etc:
The PromiseKit also included this and some other functional methods. These methods are used to change the result of a promise. These methods make the code more readable and compact.
Example:

☐ firstly:
firstly does not do anything actually. It is syntactic sugar. It just makes the code more readable.
Example:

☐ Creating a promise:

☐ Threading:
By default, all PromiseKit handlers run on the main queue. All handlers take an on parameter where you can specify the queue.
Example:

Do I need to use [weak self] in PromiseKit block?
“Generally, no. Once a promise completes, all handlers are released and so, any references to self are also released.” Details: https://github.com/mxcl/PromiseKit/blob/master/Documentation/FAQ.md#do-i-need-to-worry-about-retain-cycles
Thanks and additional resources:
https://github.com/mxcl/PromiseKit/tree/master/Documentation (all my examples are copied from this Doc. This doc is fantastic, please take a look)
https://agostini.tech/2018/10/08/using-promisekit/
https://www.raywenderlich.com/9208-getting-started-with-promisekit