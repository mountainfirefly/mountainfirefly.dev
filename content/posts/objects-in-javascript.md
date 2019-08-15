---
title: "Objects in Javascript"
date: "2019-08-15T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/objects-in-javascript/"
category: "Javascript"
tags:
  - "Programming"
  - "Javscript"
  - "Objects"
description: "In this post I am going to explain what is an object, how can we add and remove properties from it. Also going to put some light on `this` keyword, prototype and prototypal inheritance."
---
![Objects](/media/post4-image1.png)

##What is an Object?
An object is a collection of related data or/and functionality (which is usually consists of several variables and functions - which are called properties and methods inside the object.)
These objects are quite different from Javascript primitive data-types(`Number`, `String`, `Boolean`, `null`, `undefined` and `symbol`) in the sense that while these primitive data-types all are store a single value each (depending on their data-types). Objects are more complex and each object may contain any combination of primitive data-types as well as reference data-types.

An object is a reference data-type. Variables that are assigned to the reference value has given a pointer to that value. That pointer points to the memory location where the object is stored. The variable doesn't store the value.

We created an object by curly braces `{...}` with an optional list of properties. Each property is key-value pair, where `key` is property name and value can be anything.

Let's look to at an example for better understanding:
```js
let person = {
  name: 'Pankaj Thakur',
  age: 21,
  location: 'HP'
}
```
In above example, `name`, `age` and `location` are keys and `Pankaj Thakur` , `21` and `HP` are values repective to the key.

###How to perform CRUD (Create Read Update Delete) on property values
There are two ways to perform CRUD on properties and each way has its advantages and disadvantages. First one is `Dot Notation` and another one is `Bracket Notation`.

While accessing a property with `Dot Notation`, it has to be a valid Javascript identifier. For example, `object.$1` is valid, where `object.1` is invalid.

Let's see `Dot Notation` example for better understanding:
```js
var person = {}
// Add a property
person.name = 'Pankaj Thakur'
person.age = 22

// Read a property
console.log(person.name)    // "Pankaj Thakur"

// Update a property
person.name = 'Meena Thakur'

// Delete a property
delete person.name

```
Above, I have performed CRUD operations with `Dot Notation` and property has to be a valid identifier.

Now let's look at `Bracket Notation`, In this `property_name` is a string or ***Symbol***. It does not have to be a valid identifier, it can have any values e.g. `"1name"`, `"@test"`, or even `" "`(space).

Let's look at `Bracket Notation` example for better understanding:
```js
var person = {}

// Add a property
person['1name'] = 'Pankaj Thakur'
person['*age'] = 22

// Read a property
console.log(person['1name'])           // "Pankaj Thakur"

// Update a property
person['1name'] = 'Meena Thakur'

// Delete a property
delete person['*age']
```

##What is `prototype`?
Objects in Javascript have an internal property called **prototype**. It is referred to an object which contains properties and methods that are accessible across all the instances of that object. A prototype is a little bit ***magical***. When we want to read a property from an object, if it doesn't find it inside the object then Javascript automatically takes it from the prototype object.
Here is an example:
```js
var rabbit = {
  jumps: true
}
var animal = {
  eats: true
}

rabbit.__proto__ = animal
console.log(rabbit.jumps)    // true
console.log(rabbit.eats)     // true
```
In above example, we set `rabbit` prototype to animal and tries to find `eats` property that doesn't exist in rabbit object then Javascript takes `eats` property from its prototype object and it prints ` true`.

##Prototypal Inheritance
Inheritance refers to an object's ability to access methods and other properties from another object. Objects can inherit things from another object. Inheritance in Javascript works through ***prototypes*** that I have explained above and this form of inheritance is often called ***Prototypal Inheritance***.

All Javascript objects inherit properties and methods from the prototype object:
- `Date` object inherits from `Date.prototype`
- `Array` object inherits from `Array.prototype`
- `Person` object inherits from `Person.prototype` and so on.

In the above example for the prototype section, it already using inheritance, but just for the sake of example, I'll give another example of this as well:
```js
function Person() {}

Person.prototype.greeting = function(firstName, lastName) {
  console.log(`Hello, ${firstName} ${lastName}`)
}

var person = new Person()
person.greeting('Pankaj', 'Thakur')      // "Hello, Pankaj Thakur"
```

##What is `this`?
The `this` keyword refers to the current object the code is being written inside. By default, `this` refers to a global object which is global in case of NodeJs and window object in case of browser. 
- When a method is called as a property of an object, then `this` refers to the parent object.
- When a function is called with `new` operator then `this` refers to the newly created instance.
- When a function is called using `call` and `apply` method then `this` refers to the value passed as the first argument of `call` and `apply` method.

With the above-defined rules, you can reasoning to the value of `this`.
Here is an example, where `this` refering to the new instance.
```js
function person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName

  this.greeting = function() {
    console.log(`Hello, ${this.firstName} ${this.lastName}.`)
  }
}

var person1 = new person('Pankaj', 'Thakur')
var person2 = new person('Meenu', 'Thakur')

person1.greeting()         // "Hello, Pankaj Thakur."
person2.greeting()         // "Hello, Meenu Thakur."
```

Here is an example, where `this` is referring to the passed first argument.
```js
function person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName

  this.greeting = function() {
    console.log(`Hello, ${this.firstName} ${this.lastName}.`)
  }
}

var person1 = new person('Pankaj', 'Thakur')
var person2 = new person('Meenu', 'Thakur')

person1.greeting()                     // "Hello, Pankaj Thakur."
person2.greeting.call(person1)         // "Hello, Pankaj Thakur."
```
I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)