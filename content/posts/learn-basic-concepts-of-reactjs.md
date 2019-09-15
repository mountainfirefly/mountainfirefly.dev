---
title: "Learn the basic concepts of Reactjs"
date: "2019-09-15T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-basic-concepts-of-reactjs/"
category: "ReactJS"
tags:
  - "Programming"
  - "Javscript"
  - "ReactJS"
  - "State"
  - "Higher-Order Components"
  - "Lifecycle"
description: "In this post, we will get to know about ReactJS, JSX, component lifecycle methods, state, props, higher-order components and react context. Once you understand these concepts thoroughly you can build any application with ReactJS."
---
![Objects](/media/post9-image1.png)

### What is React?
React is a Javascript library that is used to build user interfaces. It is an open-source, component-based library which is only responsible for the view layer from MVC(Model View Controller).

Reactjs follows the declarative paradigm that means we just describe to it what we are trying to achieve, without instructing how to do it. It lets you compose complex UIs with small and isolated pieces that are called components.

A react application is build using multiple components, each responsible for rendering a small and usable piece of code that is written in HTML usually. Components can be nested within other components that let us allow to build a complex application out of simple building blocks.

### What is JSX?
JSX is an XML/HTML -like syntax used by React that extends ECMAScript so that XML/HTML -like text can co-exist with Javascript/Babel. It is not valid Javascript that can be read by broswer. We uses this with ReactJS to describe what is UI should look like,

This is the basic example of JSX below:

```js
const name = 'Pankaj Thakur';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
The above code is not directly read by the browser for that React uses preprocessors(i.e., transpilers like babel) to transform the JSX syntax to Standard Javascript Object that a Javascript engine will pass.

ReactDOM is a package that provides DOM specific methods that can be used at the top level of the web app to enable the efficient way of managing the DOM elements in the web page.

### Component Lifecycle Methods
The component lifecycle details the life of the component. Every component goes through this lifecycle like us we born, do something with our lives and finally, we die. Following are the main stages of the component lifecycle:
- Mounting
- Updating
- Unmounting

##### `render()`
It is the most common lifecycle method. Whatever you want to display on the web page you return that from this method. You will see this method in every class-component because this is required.

Below is an example of `render()` method:
```jsx
class Hello extends React.Component {
  render() {
    return (
      <div>
        <p>Hello, {this.props.name}</p>
      </div>
    )
  }
}
```
In the above example, `render()` method is returning JSX which displays the component UI and you can return `null` from this component if you don't want to show something. This method runs every time when the state or props gets changed.

The important thing is, you can't modify the component state from `render()` method as it runs on every state change and it can go in an infinite loop so avoid this.

##### `componentDidMount()`
Since the class-components are classes, in classes the first method runs are `constructor`, this where you would initialize your component state.

Now the next method comes is `render()` which returns your JSX. Here React `mounts` into the DOM and `componentDidMount` method runs. This is where you fire asynchronous requests for your data or you directly manipulate the DOM if you need.

```jsx
class FetchPosts extends Component {
  componentDidMount() {
    fetch('https://api.reddit.com/r/javascript')
    .then((res) => {
      if (res.status === 200) {
        return res.json()
      }

      return null
    })
    .then((data) => {
      console.log(data)
    })
  }

  render() {
    return (
      <div>Posts</div>
    )
  }
}
```
In the above example, it is making a get request for fetching reddit post that is releted to javascript. You can define all your asyncronous request like above.

##### `componentDidUpdate()`
This method runs as soon as any update happens in the component. It is useful when you want to do something on the state or prop change. You can call `setState()` in this method but don't forget it to embrace inside the condition that checks for the state or props change otherwise it can go in an infinite loop.

Take a look at the example below that shows the usage of this function:
```jsx
class FetchStackPosts extends Component {
  componentDidUpdate(preProps) {
    if (this.props.stack !== preProps.stack) {
      this.props.fetchPosts(this.props.stack)
    }
  }

  render() {
    return (
      <div>Posts</div>
    )
  }
}
```
Notice in above example it is comparing the current `stack` value with previous stack value. This is to check if there has been any change in the stack value, if there is change then only call the `fetchPosts()` method.

##### `componentWillUnmount()`
This method run just before the component gets destroyed. If there is any clean up we need to do before the component destroyed we do inside this method and we can't modify the state of the component inside this method.

Below example shows the usage of this method:
```jsx
class ResizePage extends Component {
  componentDidMount() {
    // Subscribing to resize event
    window.addEventListener("resize", this.handleResize);
  }

  componentWilUnmount() {
    // Unsubscribing to resize event
    window.removeEventListner("resize", this.handleResize);
  }
  render() {
    return <div>Hello, World</div>
  }
}
```
In the above example shows the common usage of the `componentWillUnmount()` method. After this the component gets removed from the DOM.

##### What is State and Props?
A React component can access dynamic information in two ways: props and state. Unlike props, the state can't be passed from outside of the component. To make the component to have a state property, define it inside the `constructor` method as below:

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
      <div>Hello, Mr. {this.state.name}</div>
    )
  }
}
```
`this.state` should be equal to an object and we access the value of state by this expression `this.state.name` inside the render or anywhere else inside the component.

If you want to update the value of the state variable we do it as follows:
```jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class Greeting extends Component {
  constructor(props) {
    super(props)

    this.state = {
      name: 'Pankaj Thakur'
    }
  }

  handleChange(e) {
    this.setState({
      name: e.target.value
    })
  }

  render() {
    return (
      <div>
        <p>Hello, Mr. {this.state.name}</p>
        <input value={this.state.name} onChange={this.handleChange} />
      </div>
    )
  }
}

ReactDOM.render(<Greeting />, document.getElementById('app'))
```
To update the state value, we use `this.setState` function which takes the updated value of variable inside the object.

***Props*** are values just like state but they are always passed from outside the component. We can't change the props value inside the child component.

Here is an example of how a component takes props:
```jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class Greeting extends Component {
  render() {
    return (
      <div>
        <p>Hello, Mr. {this.props.name}</p>
      </div>
    )
  }
}

ReactDOM.render(<Greeting name={'Pankaj Thakur'} />, document.getElementById('app'))
```
Above you see, `<Greeting name={'Pankaj Thakur'} />` this is how a component takes props and it can access inside the render method or elsewhere inside the child component like this `this.props.name`.

### Higher-Order Component

A higher-order component (HOC) is a function which takes a component and returns a new component. It is an advanced technique for reusing the component logic. They are the pattern that emerges from React's compositional nature.

Here is an basic example of HOC:
```jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

const GreetingWrapper = (DisplayNameComponent) => {
  return class extends Component {
    render() {
      return (
        <div>
          <p>Welcome to the Higher Order Component</p>
          <DisplayNameComponent />
        </div>
      )
    }
  }
}

class DisplayName extends Component {
  render() {
    return (
      <p>Mr. Pankaj Thakur</p>
    )
  }
}

const Greeting = GreetingWrapper(DisplayName)

ReactDOM.render(<Greeting />, document.getElementById('root'))
```
First, we have made one function that is `GreetingWrapper`. This function accepts one argument as a component that is `DisplayName` and returns a new `Greeting` component. This is the basic example of higher order component.

### Context
Context provides a way to pass data through components without explicitly pass a prop through every level of the tree. It allows you to have a central store where your data lives(just like Redux) and redux store can be inserted into any component. You can cut that middleman with the context if your application isn't big in size.

##### Create Context
First, we need to initialize Context which can be later used to create Provider and Consumer.

***MyContext.js***
```jsx
import React from 'react';

const MyContext = React.createContext()

export default MyContext;
```

##### Create Provider
Now, we are going to import our context that we will use to create a Provider. We will initialize a state with some values, which we can share using the prop value in Provider component.

***MyProvider.js***
```jsx
import React, { Component } from 'react';
import MyContext from './MyContext';

class MyProvider extends Component {
  render() {
    return (
      <MyContext.Provider
        value={{
          name: 'Pankaj Thakur'
        }}
      >
        {this.props.children}
      </MyContext.Provider>
    )
  }
}

export default MyProvider;
```
##### Create Consumer
We'll need to import the context again and wrap our component with it which inject the context argument in the component. You can use the context the same way you use the ***prop***.

***MyConsumer.js***
```jsx
import React, { Component, Fragment } from 'react'
import MyContext from './MyContext';

class MyConsumer extends Component {
  render() {
    return (
      <MyContext.Consumer>
        {
          (context) => (
            <Fragment>
              <div>{context.name}</div>
            </Fragment>
          )
        }
      </MyContext.Consumer>
    )
  }
}

export default MyConsumer;
```

To make the provider accessible to others, we need to wrap our app within.

***App.js***
```jsx
import React, { Component } from 'react';

import MyProvider from './MyProvider';
import MyConsumer from './MyConsumer';

class App extends Component {
  render() {
    return (
      <MyProvider>
        <MyConsumer />
      </MyProvider>
    )
  }
}

export default App
```
These are basic yet most important concepts in ReactJs. In this post, I have used class-component intentionally everywhere to be consistent. You can use the function component if the class component is not needed.

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)