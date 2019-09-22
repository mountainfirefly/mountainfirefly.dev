---
title: "Learn about the basic features of React Hooks"
date: "2019-09-22T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-about-basic-features-of-react-hooks/"
category: "ReactJS"
tags:
  - "Programming"
  - "Javascript"
  - "ReactJS"
  - "Lifecycle"
  - "Hooks"
description: "In this post, we will learn about React Hooks and implementing the component lifecycle methods with it. Also, how we can define the state and change with it."
---
![React Hooks](/media/post10-image1.png)

### What is React Hooks?
Hooks are the new addition in React 16.8. It let you use state and other React features without writing a class. Now we can use reserved features of a class-based component ( such internal state, lifecycle, etc) in the functional component. Writing functional component is always preferred by the community because they bring us advantages like: code that is easier to read, maintain, test and often following the best practices.

For example, it is easier to create presentational, container and business logic functional components than to create class-based components.

Let's understand, what is the difference that is made by React Hooks with examples:

###### Class-Based component

```jsx
import React, { Component } from 'react'

class Greeting extends Component {
  constructor(props) {
    super(props)

    this.state = {
      name: 'Pankaj Thakur'
    }
  }

  render() {
    return (
      <div>
        <p>Hello, {this.state.name}</p>
      </div>
    )
  }
}

export default Greeting
```
Now, implementing the above example with React hooks.
###### Functional component with React hooks
```jsx
import React, { useState } from 'react'

const Greeting = () => {
  const [name, setName] = useState('Pankaj Thakur')

  return (
    <div>
      <p>Hello, {name}</p>
    </div>
  )
}

export default Greeting
```
In case of functional component it just plain javascript function which returns React elements and it requires you write less code and it's easier to read whereas a class-based component requires you to extend from `React.Component` and create a render function which returns the React elements.

### How to use state and props in React hooks based components?
To use the state inside the functional component, React hooks gives us a hook called `useState`. We can use it inside the functional component to use state variables. React will preserve the state between re-renders. When we call `useState` it returns pair: the current state value and a function to update the state value. We can use this function to update the value from anywhere. It does the same thing as `this.setState` in class-based components.

Let's understand `useState` by an example:

```jsx
import React, { useState } from 'react'

const GreetingSomeone = () => {
  const [name, setName] = useState('')

  const handleNameChange = (e) = {
    setName(e.target.value)
  }

  return (
    <div>
      <input value={name} placeholder='Type your name...' onChange={handleNameChange} />
      <p>Hello, {name}</p>
    </div>
  )
}
```
The only argument `useState` takes is initial value. Here, in the above example we have set it to empty string.

Initially, it will display nothing and then as you put your name inside the input box, it will show your name. So on input `onChange` it is calling a function `handleNameChange`, inside it, it is calling `setName` function which is taking updated input value for `name` state variable.

##### `props`
To use `props` inside the functional component is easy. It comes in the function component parameter.

Here is an example for better understanding:
```jsx
import React from 'react'

const Person = (props) => {
  return (
    <div>
      <p>{props.name}</p>
    </div>
  )
}

export default Person
```
In above this example, just make sure wherever you are calling above component pass name prop into it otherwise it will display nothing.

### Implement component Lifecycle methods with React hooks
In this, we will implement the main lifecycle methods of a component with React hooks.
- `componentDidMount`
- `componentDidUpdate`
- `componentWillUnmount`

For performing the side effects inside the functional component, we are going to use `useEffect` hook. It takes two arguments, a function, and an optional array. The function defines which side effect to run and optional array defines when to re-run the effect.

##### `componentDidMount`
It is a lifecycle method and it runs only once when the component mounts. In this, you can do stuff like fetching data and also you can change component state.

Let's access this lifecycle method with React hooks:
```jsx
import React, { useEffect, useState } from 'react'

const Greeting = () => {
  const [name, setName] = useState('')

  useEffect(() => {
    setName('Pankaj Thakur')
  }, [])

  return (
    <div>
      <p>Hello, {name}</p>
    </div>
  )
}
```
In the above example, we are using `useEffect` hook for access effects in the component. In `useEffect`, the first parameter is callback function and the second parameter is empty array this because we don't want it to call our callback function whenever the state changes. The `componentDidMount` method only trigger once as the component mounts so it's going to do the same thing.

##### `componentDidUpdate`
`componentDidUpdate` is lifecycle method that runs every time when a component gets new props, or a state change happens.

Let's implement this method with React hooks:
```jsx
import React, { useEffect, useState } from 'react'

const Greeting = () => {
  const [name, setName] = useState('')

  useEffect(() => {
    console.log('Component has updated!')
  })

  return (
    <div>
      <p>Hello, {name}</p>
    </div>
  )
}

export default Greeting
```
In the above example, it's going to print `Component has updated!` whenever the props or state gets changed.

Also, you can subscribe to a particular state or props change like this:

```jsx
import React, { useEffect, useState } from 'react'

const Greeting = (props) => {
  useEffect(() => {
    console.log('Name prop has changed!')
  }, [props.name])

  return (
    <div>
      <p>Hello, {props.name}</p>
    </div>
  )
}

export default Greeting
```
Here, it's going to `console.log` only for the `name` prop change. Whenever the `name` prop gets changed it will print `Name prop has changed!` in the console. In the callback function, you can give instructions according to your need.

##### `componentWillUnmount`
`componentWillUnmount` is invoked immediately before a component is umounted or destroyed. We do necessary clean up in the method such as cancel requests, detach listeners and clean up of any subscriptions that have been created while a component is mounted.

Let's implement is with React hooks:
```jsx
import React, { useEffect } from 'react'

const ResizePage = () => {
  const handleSize = () => {
    // Calls as you change the resize you page
  }

  useEffect(() => {
    // Subcribing to resize listener
    window.addEventListener("resize", handleResize);
    return () => {
      // Unsubcribing to resize listener
      window.removeEventListener("resize", this.handleResize);
    }
  })

  return (
    <div>
      <p>Resize your page.</p>
    </div>
  )
}

export default ResizePage
```
This is how we unsubscribe to the listener, we need to return a function that contains the unsubscribe instructions from the callback function in the `useEffect`.

So, these are some of the most common features of React hooks. You can check out [here](https://reactjs.org/docs/hooks-intro.html) for full documentation on React hooks.

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
