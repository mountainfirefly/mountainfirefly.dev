---
title: "Part 2: Learn state management with ReduxJS"
date: "2019-10-06T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-state-management-with-redux/"
category: "Javascript"
tags:
  - "Programming"
  - "Javascript"
  - "State"
  - "Management"
  - "ReduxJs"
  - "VanillaJs"
description: "In this post, we will get to know what is redux, its benefits, main concepts/components of Redux. At last, for better understanding, we'll create a Counter application with Vanilla Js and Redux."
---
![State management](/media/post11-image1.png)

In my last post, I talked about what is state management and why it is important and what are the tools out there to manage your application state. If you haven't check it out, you can check it out [here](https://mountainfirefly.dev/posts/state-management/).

In this post, we will be exploring one of the state management tools that people are liking in the programming world these days that is called Redux. So let's start with the article.

### What is Redux?
Redux is an open-source javascript library for managing application state. It is a predictable state container for Javascript applications. It is most commonly used with libraries like React or Angular for building user interfaces.

Redux makes it easy to manage the state of your application. In other words, it helps you manage the data you display and respond to the user's actions. It is lightweight around 2KB (including dependencies), so you don't have to worry about your application size gets bigger.

#### What are the benefits of Redux?
- It has predictable state updates that make it easier to understand how is the data flow in the application.
- It uses the reducer to manipulate the state of the store which is just a pure function and pure functions are easier to debug.
- Redux state is immutable which increases the performance of your app.
- Data management is really easy because of reducers.

### Create store in Redux
Create a store that holds the application state, we use `createStore(reducer, [preloadedState], [enhancer])` to create a store.
Let's have details discussion about create store method arguments:
- `reducer` is reducing function that returns the next state, given the current state and action in the parameter.
- `preloadedState` is an initial state that you can pass while creating the store. It is an optional parameter and there is one thing you need to focus on is if your `reducer` is produced from `combineReducers` that make sure your pass same shape as the keys passed to it.
- `enhancer` is a function that enhances the store, it provides the capability to add third-party such a middlewares.

#### There are three main components that I think are important in Redux:
- Store
- Reducers
- Actions

#### Store
A Store is an object that holds the application state and you can access the state by `getState()` function that is provided by the Redux API. To update the data inside the store you can do it by dispatch an action on to the store.

It is important to have a single store in your Redux application. When you want to split your data management logic, you will use reducer composition instead of many stores.

For creating the store, you just need to have a reducer and if you have more than one reducers you can use `combineReducers` to combines several reducers to one.

This is how you can a store:
```jsx
import { createStore } from 'redux'
import GreetingReducer from './reducers'

const store = createStore(GreetingReducer)
```

#### Actions
Actions are objects that send data from your UI to your store. They are the only source of information for the store. You can send them to the store by dispatch function.

Here is an quick example of action object:
```jsx
const SET_NAME = 'set_name'

const setName = {
  type: SET_NAME,
  value: 'Pankaj Thakur'
}

store.dispatch(setName)
```

#### Reducers
Reducer is a pure function that takes a state and an action and returns the next state. We can reducer because it reduces a collection of actions and an initial state (of the store) on which to perform these actions to get the resulting final state.

In Redux, all the application state is stored as a single object. It's a good idea to think of its shape before writing any code.

Here is a representation of redux store:
```jsx
import { SET_NAME } from './ActionTypes'

const INITIAL_STATE = {
  name: ''
}

const Greeting = (state = INITIAL_STATE, action) => {
  switch(action.type) {
    case SET_NAME:
      return {
        ...state,
        name: action.value
      }
    default:
      return state
  }
}

export const Greeting
```
Theoretically, I think you have understood what is redux, its benefits and main components. Let's create a sample app with Vanilla JS and Redux for better understanding.

We are going to create a counter app that shows the count and you can increase the count by clicking over the plus button and to decrease the count you can click over the minus button.

### Counter app using Vanilla Js and Redux

So, let's start with our application.

Create a folder for your counter app and then create `index.html` file inside it put this content inside it.

```html
<!DOCTYPE html>
<html>
<head>
<meta name="description" content="Redux with vanilla JS">
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/3.3.1/redux.js"></script>
</head>
<body>
  <div id="counter">
    <button id="inc">+</button>
    <span id="text"></span>
    <button id="dec">-</button>
  </div>
  <script src="./counter.js"></script>
</body>
</html>
```
In above example, to use redux inside the vanilla js we are importing it from CDN redux link using script tag. You can access redux from `Redux` variable inside your app.

Now, let's give life to our application by creating `counter.js` inside it and import `createStore` from `Redux` global object and create the counter store from your application.
```jsx
const { createStore } = Redux

const counter = (state = 0, action) => {
  switch(action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state + 1
    default:
      return state
  }
}

const store = createStore(counter)
```
We have created the `counter` store, initially, the state will be zero by default and we have defined two cases `INCREMENT` and `DECREMENT` which increase and decrease state value.

Let's create action objects for our counter reducer:
```jsx
const increment = () => {
  return {
    type: 'INCREMENT'
  }
}

const decrement = () => {
  return {
    type: 'DECREMENT'
  }
}
```

Actions are created, the next step is to make your UI elements dynamic and subscribe to our redux store.
```jsx
class Counter {
  constructor(options) {
    this.$element = options.el
    this.store = options.store

    // Subcribe to the Redux store
    this.store.subcribe(this.update.bind(this))

    this.$el.querySelector('#inc').addEventListener('click', this.increment.bind(this))
    this.$el.querySelector('#dec').addEventListener('click', this.decrement.bind(this))
  }

  this.increment() {
    this.store.dispatch(increment())
  }

  this.decrement() {
    this.store.dispatch(decrement())
  }

  this.update() {
    this.$el.querySelector('#text').innerHTML = store.getState()
  }

  render() {
    this.update()
  }
}
```
Here, we have added listeners for increment and decrement buttons and subcribed to redux store, updates value in the UI.

Now let's invoke our Class Counter on DOM load.

```js
document.addEventListener('DOMContentLoaded', () => {
  const counter = new Counter({
    el: document.getElementById('counter'),
    store
  })

  counter.render()
})
```
Now, open your `index.html` in browser to see it actually working.

Check out the application source code [here](https://github.com/mountainfirefly/redux-vanilla-js-counter-app)

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
