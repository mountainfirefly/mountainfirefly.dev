---
title: "Summary: You don’t know JS - Book 1, Chapter 1"
date: "2018-09-19T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/summary-you-dont-know-js-book-1-chapter-1/"
category: "Javascript"
tags:
  - "Programming"
  - "Javscript"
  - "You Don't know JS"
description: "A program, often referred to as source code or just code, is a set of special instructions to tell the computer what tasks to perform."
---
![You Don't know JS](/media/post1-image1.png)

##Code
A program, often referred to as source code or just code, is a set of special instructions to tell the computer what tasks to perform.

##Statements
A group of words, numbers, and operators that performs a specific task is a statement.
```js
a = b * 2;
```

##Expressions
This statement has four expressions in it.
- `2` is a literal value expression.
- `b` is a variable expression.
- `b * 2` is an arithmetic expression.
- `a = b * 2` is an assignment expression.

##Console
It is a common object/method provided by debuggers (including the Chrome debugger and Firebug) that allows a script to log data (or objects in most cases) to the **JavaScript console**.
```js
console.log('Hello World');
```

##Input
Working with any dynamic language requires the ability to read, Javascript give a prompt() to take user input from browser.
```js
const age = prompt('What is your age');
console.log(age);
```

##Operators
Operators are how we perform actions on variables and values.
- Assignment: `=` as in `a = 2`.
- Math: `+` (addition), `—` (subtraction), `*` (multiplication), and `/` (division), as in `a * 3`.
- Compound Assignment: `+=`, `-=`, `*=`, and `/=` are compound operators that combine a math operation with assignment, as in `a += 2` (same as `a = a + 2`).
- Increment/Decrement: `++` (increment), `--` (decrement), as in `a++` (same as `a = a + 1`).
- Object Property Access: `.` as in `console.log()`.
- Equality: `==` (loose-equals), `===` (strict-equals), `!=` (loose not-equals), `!==` (strict not-equals), as in `a == b`.
- Comparison: `<` (less than), `>` (greater than), `<=` (less than or loose-equals), `>=`(greater than or loose-equals), as in `a <= b`.
- Logical: `&&` (and), `||` (or), as in `a || b`.
```js
let x = 60;
x = x + 30;
x = x - 20;
x = x / 2;
console.log(x);     // 35
```

##Code Comments
One of the most important lessons you can learn about writing code is that it’s not just for the computer. Code is every bit as much, if not more, for the developer as it is for the compiler. 
— You should strive not just to write programs that work correctly, but programs that make sense when examined.
— These are bits of text in you program that are inserted purely to explain things to a human. The interpreter/compiler will always ignore these comments.
```js
// This is a single line comment.
/* But this is
         a multiline comment
                  comment. */
```

##Variables
Most useful programs need to track a value as it changes over the course of the program, undergoing different operations as called for by your program’s intended tasks. The easiest way to go about that in your program is to assign a value to a symbolic container, called a variable.
```js
var y;    // This how you define a variable.
y = 20    // This is how you initialise a variable.
y = y + 20;
console.log(y)     // 40
```

##Block
Group a series of statements together, which we often call a block. In JavaScript, a block is defined but wrapping one or more statements inside a curly brace pair `{ .. }`.
```js
var x = 100;
// a general block
{
    x = x * 2;
    console.log(x);    // 200
}
```

##Conditional
The if/else statement executes a block of code if a specified condition is true. If the **condition** is false, another block of code can be executed. The if/else statement is a part of **JavaScript’s “Conditional”** Statements, which are used to perform different actions based on different conditions.
```js
var x = 20;
var y = 22;
if (y > x) {
   console.log('Wow! y is greater than x.');
}
```

##Loops
Repeating a set of actions until a certain condition fails — in other worlds, repeating only while the condition holds — is the job of programming loops; loops can take different forms, but they all satisfy this basic behaviour.
```js
var i = 0;
// a `while...true` loop would run forever, right?
while (true) {
     // stop the loop
     if (i >= 9) {
         break;
     }
    
     console.log(i);
     i += 1;
}
for (var a = 0; a <= 9; a++) {
    console.log(a)
}
// 1, 2, 3, 4, 5, 6, 7, 8, 9
```

##Functions
A function is composed of a sequence of statements called the function body. Values can be passed to a function, and a function will return value.
- Your program will almost certainly want to break up the code’s tasks into reusable pieces, instead of repeatedly repeating yourself repetitiously.The way to do this is to define a function.
```js
function multiply(x) {
   return x * 2;
}
var a = 20;
var result = multiply(a);
console.log(result);    // 40
```

##Scope
Scope is a collection of variables as well as the rules for how those variables are accessed by name.
- In Javascript, each function gets its own scope. Only code inside that function can access that function’s scoped variables.
- A variable name has to be unique within the same scope — there can’t be two different a variables sitting right next to each other. But the same variable name a could appear in different scopes.
```js
function one() {
   // This `a` only belongs to the `one()` function
   var a = 1;
   console.log(a);
}
function two() {
   // This `a` only belongs to the `two()` function
   var a = 2;
   console.log(a);
}
one();      // 1
two();      // 2
```

This book is really nice for beginners, explaining everything in detail. I would recommend it for every beginner.

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)