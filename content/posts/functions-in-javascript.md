---
title: "Functions in Javascript"
date: "2019-08-08T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/functions-in-javascript/"
category: "Javascript"
tags:
  - "Programming"
  - "Javscript"
  - "Functions"
description: "In this I'll be taking you through functions, what is function, different ways of defining a function and difference between arguments and parameters. Also explaining you different types of functions eg. anonymous function, higher-order functions, pure functions and IIFE (Immediately invoked function expression)."
---
![Functions](/media/post3-image1.png)

##What is a function?
A function is a code of block designed to perform a particular task. They are just objects and they are executed when they are invoked. We can pass parameters to the function and use them within the function to do operations.
Functions always return a value, if the return statement is not specified within the function expression then Javascript returns `undefined`.

##Define a function.
There are a few different ways to define a function.
###Function Declaration
A function declaration defines a named function. To create a function declaration we use `function` keyword followed by the name of the function, a function declaration is hoisted and can be invoked before its definition. A function is created with a function declaration is a `Function` object that has all the methods, properties and behavior of the `Function` object.

Let's declare a function.
```js
function greeting() {
  console.log('Hello, Stranger!!')
}

greeting()        // "Hello, Stranger!!"
```
###Function Expression
There is not much of a difference between the function declaration and function expression. The only difference is that a function declaration can be named function or an anonymous function. An anonymous function is a function that has no name. A function expression is not hoisted, and therefore they can't be invoked before they are defined. 

Let's declare a function expression.
```js
var greeting = function() {
  console.log('Hello again, Stranger!!')
}

greeting()            // "Hello again, Stranger!!"
```

###Arrow Function Expression
An arrow function expression is a shorter syntax for defining a function and they don't create there own `this` like other regular function expressions.

Let's declare an arrow function expression.
```js
var greeting = () => {
  console.log('Hello, how are you doing?')
}

greeting()           // "Hello, how are you doing?"
```
###IIFE (Immediately Invoked Function Expression)
IIFE (Immediately Invoked Function Expression) is one of the ways of defining a function expression in javascript that run as soon as they are defined. It is a design pattern which also known as ***Self-Executing Anonymous Function.
Here is an example of IIFE:
```js
(function(firstName, lastName) {
  console.log(`Hello, ${firstName} ${lastName}`)
})('Pankaj', `Thakur`)

// "Hello, Pankaj Thakur"
```

##Parameters vs. Arguments
If you're new to Javascript, you may have heard the terms Parameters and Arguments used interchangeably. There is an important distinction to make between these two keywords.

###Parameters
Parameters are variables used while defining a function, they are ***names*** created in the function definition. Parameters are separated by commas in the `()`. We can pass up to 255 parameters in the function definition. Here is an example of parameters (`firstName` and `lastName`):

```js
var firstName = 'Tina'
var lastName = 'Gupta'

function greeting(firstName, lastName) {
  return `Hello, ${firstName} ${lastName}!`
}

console(greeting(firstName, lastName))      // "Hello, Tina Gupta!"
```
###Arguments
Arguments are the actual values that are supplied while invoking a function. In the above function it is ***Tina*** and ***Gupta*** arguments.

##Anonymous Functions
I have found the term ***anonymous*** pretty confusing in Javascript. The more general explanation is a function without a name. In javascript words we could say, the anonymous function is a function which has no identifier. An anonymous function expression is not hoisted that mean we can't invoke them before they are defined.
Here is an example of ***anonymous function***:
```js
var greeting = function() {
  console.log('Hello, Stranger')
}

greeting()                // "Hello, Stranger"
```
In the above example, `greeting` variable is getting assigned by ***anonymous*** function.

##Higher-order Functions
Higher-order functions are functions which operate on the other functions, either by taking them in the arguments or returning them from the function expression. Higher-order functions help us to create abstractions over actions and as well as values. Javascript provides built-in higher-order functions abstractions examples `map`, `filter`, `reduce`, `forEach` etc.

```js
function getMaxNumber(f) {
  return function(...arg) {
    var result = f(...arg)
    return result
  }
}

getMaxNumber(Math.max)(1, 6, 9)        // 9
```

##Pure Functions
Pure functions produce the output on the given input. Pure functions are deterministic, this means, given the same input, the function will always return the same output. If these functions are dependant on the external scope then they are no longer be called a `Pure Functions`, they become `Impure Functions`. The pure function always returns a value.
Here is an example of Pure function:
```js
var greeting = function(firstName, lastname) {
  return `Hello, ${firstName} ${lastName}`
}

console.log(greeting('Pankaj', 'Thakur'))     // "Hello, Pankaj Thakur"
```

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)