---
title: "Summary: You don’t know JS - Book 1, Chapter 2"
date: "2019-08-02T23:46:37.121Z"
template: "post"
draft: false
slug: "/posts/summary-you-dont-know-js-book-1-chapter-2/"
category: "Javascript"
tags:
  - "Programming"
  - "Javscript"
  - "You Don't know JS"
description: "In this chapter we would specifically focus on the Javascript concepts that will not fully explored in this chapter. You can think of this as an overview of the topics that covered in details in throughout the rest of the series."
---
![You Don't know JS](/media/post2-image1.png)

## Values and Types 
As this book asserted in chapter 1, Javascript doesn't have typed-variables but typed-values. Here are the built-In types available -
- `string`
- `number`
- `boolean`
- `object`
- `null`
- `undefined` and
- `symbol` (introduced in es6)

Javascript has a function called `typeof`, which examine the value and tell what type of data contain by the value. Here we have a couple of examples:
```js
var x;
typeof x     // undefined

x = 'How are you doing'
typeof x     // "string"

x = true
typeof x     // "boolean"

x = null
typeof x     // "object" -- weird, bug
```
You may have noticed, how is this snippet `x` variable holds a different type of value, and despite appearances, `typeof x` is asking for the variable type but value inside that variable it holds.

##Objects
The `object` typically refers to a compound value where you can set properties(named locations) that each holds its value and type.
```js
var a = {
  name: 'Tina',
  age: '22'
}

a['name']   // "Tina"
a.age       // "22"
```
Object properties can either be accessed with ***dot notation*** (i.e. `a.name`) or ***bracket notation*** (i.e., `a['age']`). Dot notation is shorter and generally easier to read and is thus preferred when possible.

Bracket notation is useful if you have a property name that has the special character inside `a["hello world"]` or property name value is stored in the variable (i.e., `a[variable1]`)

##Arrays
An array is an `object` that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions. For example:
```js
var arr = [
  'Test',
  42,
  true
]

arr[0]       // "Test"
arr[2]       // true
```
Languages that start counting at zero, like JS does, use `0` index to access the first element in the array.

##Functions
The other `object` subtype you will use all over your JS programs is Function.
```js
function foo() {
  return 22
}

foo.bar = "What is a lovely variable."

typeof foo          // "function"
typeof foo()        // "number"
typeof foo.bar      // "string"
```
Again, functions are subtype of `objects`-- `typeof` return `"function"`, which implies that a `function` is main type --and can thus have properties, but you typically use these function type properties in limited cases.

##Built-In Type Methods
The built-in types and subtypes we've just discussed have behaviors exposed as properties and methods that are quite powerful and useful.
```js
var a = "Welcome"
var b = 3.323232

a.length              // 7
a.toUpperCase()       // "WELCOME"
b.toFixed(4)          // 3.3232
```
The "how" behind being able to `a.toUpperCase()` is more complicated than just method existing on the value.

Briefly, there is a `String` object wrapper form, typically called a `native`, that pair with primitive `string` type; it's this object wrapper that defines the `toUpperCase()` method on its prototype. As for `number` type we have `Number` object wrapper and the same goes for other types.

##Comparing Values
There are two types of value comparison that you need to make in your JS programs: ***equality*** and ***inequality***. The result of any comparison is strictly `boolean` value (`true` and `false`), regardless of what value types are being compared.

###Coercion
Coercion comes in two forms in Javascript: ***explicit*** and ***implicit***. Explicit coercion is simply that you can see obviously from the code that conversion from one type to another will occur, whereas the implicit coercion is when one type conversion happens as more of a non-obvious side effect of some other operation.
```js
// Explicit coercion
var x = '12'
var y = Number(x)

typeof x         // "string"
typeof y         // "number"

// Implicit coercion
var a = '12'
var b = a * 2

a            // "12"
b            // 24
```

###Truthy & Falsy
The specific list of "falsy" values in Javascript is as follows:
- `""` (empty string)
- `0`, `-0`, `NaN` (invalid `number`)
- `null`, `undefined`
- `false`
Any value that is not in the "falsy" list is "truthy". Here are some examples:
- `"Hello"`
- `54`
- `[]`, `[1, 3, 4]`
- `{}`, `{ a: 2 }`
- `function foo() { ... }`
It's important to remember that a non-`boolean` value only follows this "truthy/falsy" coercion if it's actually coerced to `boolean`.

###Equality 
There are four type of equality operators: `===`, `==`, `!==` and `!=`. The difference between `===` and `==` is usually characterized that `==` checks for value equality comparison and `===` checks for both value and type equality. Same goes for these `!==` and `!=` operators as well.

###Inequality
The `>`, `<`, `>=` and `<=` operators are used for inequality. Typically they will use with ordinally comparable values like `number`s. It's easy to understand that 3 < 4.
Notably, there are no "strict inequality" operators that would disallow coercion the same way `===` "strict equally" does.
Consider
```js
var a = 23
var b = "34"
var c = "35"

a < b    // true, b variable value get coerced to number.
c < b    // false, both values coerced to number as numeric comparison occured.
``` 

##Variables
In Javascript, variable names (including function name) must be valid identifiers. An identifier must start with `a-z`, `A-Z`, `$` and `_`. It can then contain any those characters plus the numerals `0-9`.

Generally, the same rule applies to the property name to be a valid identifier. However, certain word can't be used as variables, but are okay as a property name. These words are called "revered words", includes the JS keyword(`for`, `if`, `in`, etc.) as well as `null`, `true` and `false`.

##Function Scopes
We use `var` keyword to declare a variable name that belongs to function scope or global scope if it defined at the top level outside any function.

###Hoisting
Wherever a `var` appears inside the scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.

Metaphorically, this behavior is called ***hoisting***, when a `var` declaration conceptually moved to the top its enclosing scope. Technically, this process is more accurately explained by how the code is compiled.
Consider:
```js
var x = 'hello world'

foo()               // foo() works because the declaration is "hoisted"

function foo() {
  console.log('World is great', a)     // "World is great with you." happens because of "hoisted"

  var a = 'with you.'
}
```
###Nested Scope
Whenever a variable is declare, it is avaible in it's scope as well as any lower/inside scopes. For example:
```js
function foo() {
  var x = 'I am new to programming.'

  function bar() {
    var y = ' Me too.'
    
    console.log(x + y)       // "I am new to programming. Me too"
  }
  bar()
}

foo()
```

##Conditionals
Javascript provides a few other conditionals mechanisms that we should take a look at.

Sometimes you may find yourself writing a series of `if..else...if` statements like this:
```js
var a = 33

if (a === 34) {
  // Do this
}
else if (a === 12) {
  // Do another thing
}
else if (a === 33) {
  // Do yet another thing
} else {
  // Else fallback here
}
```
This structure works, but it's kind of little verbose here because you need to specify the `a` condition in each case. Here is another option, the `switch` statement:
```js
switch(a) {
  case 34:
    // Do this
    break;
  case 12:
    // Do another thing
    break;
  case 33:
    // Do yet another thing
    break;
  default:
    // Else fallback to here
}
```
The `break` statement is important if you want only one statement to run.

There is another form of a conditional statement is called "ternary operator". It's like a more concise form of write `if..else` statement, such as:
```js
var a = 23

var b = (a === 34) ? "You're right." : "You're wrong."

console.log(b)      // "You're wrong."
```

##Strict Mode
ES5 added a "strict mode" to the language, which tightens the rules for certain behaviors. Generally, these restrictions are seen as keeping the code to a safer and more appropriate set of guidelines. Also, adhering to the strict mode makes your code more optimizable by the engine.
```js
"use strict"

function foo() {
  a = 3              // `var` missing, ReferenceError
}
```
Not only will strict mode keep your code to the safer path and not only will it make your code more optimizable, but it represents the future directions of the language. It'd be easier for you to used to strict mode now than keep putting it off, it'll only get harder to convert later

##Functions As Values
So far, we've discussed the functions as a primary mechanism of scope in Javascript. You recall typical `function` declaration syntax as follows:
```js
function foo() {
  // ..
}
```
Though it may not seem obvious from that syntax, `foo` is variable from outer enclosing scope that's given a reference to the `function` being declared. That is, `function` itself is a value, just like `42` or `[1, 2, 3]` would be.

Function itself is can be value that's assigned to variables, or passed to or returned from other functions.
```js
var foo = function() {      // Anonymous function has assigned to variable foo.
  // ..
}
var bar = function bar() {   // Named function expression.
  // ..
} 
```

###Immediately Invoked Function Expressions (IIFEs)
In the previous snippet, neither of the function expression is executed -- we could if we had included `foo()` or `bar()`.

There is another way to execute a function expression, which is typically referred to ***Immediately Invoked Function expression (IIFEs)"***.
```js
(function IIFEs() {
  console.log('Immediately Invoked Function Expressions')
})()
```
###Closure
***Closure*** is one of the most important and often least understood, a concept in Javascript. You can think of closure as a way to remember and continue to access the function scope(it's variables) once the function has been finished running.
Consider:
```js
function makeAdder(x) {

  function add(y) {
    return y + x
  }

  return add
}

var add = makeAdder(10)
add(40)                // 50
```
###Modules
The most common usage of closure in the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as the public API that is accessible from outside.
Consider:
```js
function User() {
  var username, password;
  
  function doLogin(user, pass) {
    username = user
    password = pass

    // Do the rest
  }

  var publicAPI = {
    login: doLogin
  }

  return publicAPI
}

// Create a `User` module instance
var fred = User()

fred.login("fred" "12Password01")
```
The `User()` function serves as an outer variables `username` and `password` as well as `doLogin()` function; these all are private inner details of this `User` module that cannot be accessed from the outer world. 

##`this` Identifier
Another very commonly misunderstood concept in JS is the `this` identifier. While it may often seem that `this` is related to "object-oriented patterns," in JS `this` is a different mechanism.
If a function has a `this` inside it, actually is referring to an `object`. But which depends on how the function was called.
It's important to realize that `this` does not refer to the function itself, as is the most common misconception.
Here's is the quick illustration:
```js
function foo() {
  console.log(this.bar)
}

var bar = "global"

var obj1 = {
  bar: "obj1"
  foo: foo
}

var obj2 = {
  bar: "obj2"
}

foo();        // "global"
obj1.foo()    // "obj1"
foo.call(ob2) // "obj2"
new foo()     // undefined  
```
There are four rules for now `this` gets set, and they are shown in the last four line of the snippet.
- `foo()` ends up setting `this` to the global object in non-strict mode -- in strict mode, this world be `undefined` and you'd get an error accessing the `bar` property -- so "global" is the value found for `this.bar`.
- `obj1.foo()` sets `this` to the `obj1` object.
- `obj1.call(obj2)` sets `this` to the `obj2` object.
- `new foo()` sets `this` to the whole brand new empty object.

##Prototypes
The propTypes mechanism in Javascript is quite complicated. When you reference a property on an object, if that property doesn't exist, JS will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost a fallback if the property is missing.
```js
var foo = {
  a: 42
}

var bar = Object.create(foo)

bar.b = 'hello world'

bar.b              // "hello world"
bar.a              // 42   <--- delegated to 'foo'
```
The `a` property doesn't exist on `bar` object, but because `bar` prototype-linked to `foo`, JS will automatically fallback to `foo` if it doesn't find the property on `bar` object, where it's found.

##Polyfilling
The word "polyfill" is invented term (by Remy Sharp)(https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of newer feature and producing a piece of code that's equivalent to the behavior but can run in older JS browser.

Consider:
```js
if (!Number.isNaN) {
  Number.isNaN = function isNan(x) {
    return x !== x
  }
}
```
The `if` statement guards agained applying the polyfill defination in ES6 browser where it will already exists. If it's not already present, we define `Number.isNaN(...)`.

##Transpiling
There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.
So the better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "Transpilling," a term for transforming + compiling.
Here's is a quick example of transpiling. ES6 adds a feature called "default parameter values". It looks like this:
```js
function foo(a = 2) {
  console.log(a)
}
foo()         // 2
foo(42)       // 42
```
Simple, right? Helpful, too! But it's new syntax that's invalid in pre-ES6 engines. So what will a transpiler do with that code to make it run in older enviroments?
```js
function foo() {
  var a = arguments[0] !== (void 0) ? arguments[0] : 2
  console.log(a)
}
```
As you can see, it checks to see if the `arguments[0]` value is `void 0` (aka `undefined`), and if so provides the `2` default value; otherwise, it assigns whatever was passed.

All these concepts are explained in chapter 2 of book 1 in you don't know JS in full detail. This was summary of each concept of chapter 2 in little detail 🙂.

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
