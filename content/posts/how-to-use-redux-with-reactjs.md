---
title: "How to use Redux with ReactJS"
date: "2019-10-13T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/how-to-use-redux-with-reactjs/"
category: "ReactJS"
tags:
  - "Programming"
  - "Javascript"
  - "ReactJS"
  - "React-Redux"
  - "Redux"
description: "In this post, we'll learn about React-Redux which provides a binding to connect React application to Redux. At last, we will create a sample application with React-Redux for a better understanding and for practical usage."
---
![React-ReduxJS](/media/post12-image1.png)

In my last post, I talked about Redux, its benefits and explained its main components/concepts. If you don't have an understanding of how does Redux works I highly recommend you to check that out [here](https://mountainfirefly.dev/posts/learn-state-management-with-redux/).

In this post, we'll be exploring React-Redux and how you can use it to connect your React application with Redux.

Let's start.

### What is React-Redux?
React bindings are not included in Redux by default. You need to install them explicitly. React-Redux is a package that provides official bindings for Redux. With the help of this package, you can connect your React application with Redux.

I have seen people getting confused with the React-Redux package they think once they have installed the `react-redux` package they don't need to explicitly install the Redux in their application. But that is not true, `react-redux` is a package that sits in between React Redux and helps them communicate with each other. You need to install Redux separately in your React application.

There is two main component/function provided by `react-redux` which I think most important:
- Provider
- `connect()`

#### Provider
`react-redux` provides `<Provider />`, which makes the Redux store available to the rest of your app.
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'

import store from './store'
import App from './App'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

#### `connect()`
`react-redux` provides a `connect` function for your connect your component to the store. Here is how you connect your component with store:
```jsx
import React from 'react'
import { connect } from 'react-redux'

import DisplayNameView from './DisplayName.component'
import { changeName } from './actions'

const mapStateToProps = (state, ownProps) => {
  const { name } = state.app

  return {
    name
  }
}

const DisplayName = connect(mapStateToProps, {
  changeName
})(DisplayNameView)

export default DisplayName
```
`connect` function doesn't modify the passed component instead it returns the new component, which is wrapped by the connected component.

This function accepts four different types of parameters, all optional. They are called:
1. `mapStateToProps`?: Function
2. `mapDispatchToProps`?: Function | Object
3. `mergeProps`: Function
4. `options`?: Object

Mainly first two are beings most used in the React-Redux application. So will be discussing the first two functions only:

##### `mapStateToProps`:
if the `mapStateToProps` function is specified, the new wrapper component will subscribe from the store updates. So whenever the store is updated `mapStateToProps` will be called. If you don't want to subscribe to the store updates pass `null` or `undefined` in place of `mapStateToProps`.

```jsx
const mapStateToProps = (state, ownProps) => {
  const { name } = state.app
  return {
    name
  }
}
```

##### `mapDispatchToProps`:
You can use `mapDispatchToProps` as a function or as an object. If you use it as a function it takes two params `dispatch` and `ownProps`. The `dispatch` you can use it to dispatch actions and `ownProps` is nothing but props that are passed from the parent component.

Here is example of how to use `mapDispatchToProps` function:
```jsx
const mapDispatchToProps = (dispatch) => {
  return {
    increment: (value) => dispatch({ type: 'CHANGE_NAME', value })
  }
}
```

If you using it as an object, here is an example for better understanding :
```jsx
import { connect } from 'react-redux'

import { changeName, changeEmail } from './actions'
import PersonView from './Person.component'

const Person = connect(null, {
  changeEmail,
  changeName
})

export default Person
```
By now, you most probably understood what is React-Redux and what are its functions. Now, we are going to create a sample posts app that will display sample posts. We will create very minimal UI for the list of posts, just to see how does redux works in a live application and how does it manage our application state.

### Create Sample Posts App
We will be using `create-react-app` to get started with the react project.
Run the following commands to create sample-posts-app:
```zsh
npx create-react-app sample-posts-app
cd sample-posts-app
npm start
```

Once the basic react project is set up, let's install Redux and React-redux. To do that run following commands in your terminal:
```zsh
npm install react-redux
npm install redux
```
Create a `reducers` directory for all your reducers and `PostReducer.js` under `reducers` dir. Also, create `actions` dir and `PostActions.js` under it.

```zsh
cd src
mkdir reducers
mkdir actions
cd reducers/
touch PostReducer.js
cd ..
cd actions
touch PostActions.js
```

Open `PostActions.js` file in your code editor and create an action to store your posts in the store like this.
```jsx
export const setPosts = (value) => {
  return {
    type: 'SET_POSTS',
    value
  }
}

export const toggleLoading = (value) => {
  return {
    type: 'TOGGLE_LOADING',
    value
  }
}
```
Open `PostReducer.js` file and put this code inside `PostReducer.js`.

```jsx
const INITIAL_STATE = {
  posts: [],
  loading: false
}

const PostReducer = (state = INITIAL_STATE, action) => {
  switch(action.type) {
    case 'SET_POSTS':
      return {
        ...state,
        posts: action.value
      }
    case 'TOGGLE_LOADING':
      return {
        ...state,
        loading: action.value
      }
    default:
      return state
  }
}

export default PostReducer
```
The next step is to create a `store.js` file which will create a redux store for our application with the help of reducers. Create `store.js` under `src` dir.

```zsh
cd src
touch store.js
```

Open `store.js` and put the following code inside this file.

```jsx
import { createStore } from 'redux'
import RedditReducer from './reducers/redditReducer'

// We are not using combineReducer here because we don't have multiple reducers.
const store = createStore(RedditReducer)

export default store
```

Now let us make our redux store available to the rest of our app by putting the following code inside your `index.js` that is under `src` dir.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';

import store from './store'
import App from './App';
import './index.css';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

In this step, will be creating our components and containers for the posts application. Create `containers` and `components` dirs under `src` dir also create component and container file for posts as well in the respective directories.

```zsh
cd src
mkdir containers
mkdir components
cd containers
touch Posts.js
cd ..
cd components
touch Posts.js
```

Now will create minimal UI to display posts data in the `components/Posts.js` components

```jsx
import React from 'react'

const Posts = ({ posts, loading }) => {
  if (loading) {
    return <p>Loading...</p>
  }

  if (!posts.length) {
    return <p>No Posts Found.</p>
  }

  return (
    <div>
      <ul>
        {posts.map((post, index) => {
          return (
            <li style={{ textAlign: 'left' }} key={index}>
              <h3>{post.title}</h3>
              <p>{post.body}</p>
            </li>
          )
        })}
      </ul>
    </div>
  )
}

export default Posts
```
The above component accepting the two props `posts` which will contain posts data and `loading` it will `true` when our application will be fetching the posts and based on this the app will show the loading text on the screen. 

Next will create `containers/Posts.js` where we will fetch posts data and dispatch an action to store it inside the redux store. Also, it will pass posts to the post component that we have created above.

```jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'

import PostsView from '../components/Posts'
import { setPosts, toggleLoading } from '../actions/PostActions'

class FetchPosts extends Component {
  componentDidMount() {
    const { toggleLoading, setPosts } = this.props
    
    toggleLoading(true)

    fetch('https://jsonplaceholder.typicode.com/posts')
    .then((res) => {
      if (res.status === 200) {
        return res.json()
      }
    })
    .then((result) => {
      setPosts(result)
      toggleLoading(false)
    })
    .catch(() => {
      toggleLoading(false)
    })
  }

  render() {
    return <PostsView {...this.props} />
  }
}

const mapStateToProps = (state) => {
  const { posts, loading } = state

  return {
    posts,
    loading
  }
}

const Posts = connect(mapStateToProps, {
  setPosts,
  toggleLoading
})(FetchPosts)

export default Posts
```

Inside `componentDidMount` we are making an API request to fetch all the posts and once they will fetched, it will dispatch an action `setPosts` which is going to store posts data in the redux store.

We have used the `connect` function here to access the redux state and `dispatch` function. The `mapStateToProps` function returning `posts` and `loading` so that it will be accessible inside the child components.

Now will call our Posts container inside the `App.js`, you can do it by replacing the `App.js` code with the following code.

```jsx
import React from 'react';

import Posts from './containers/Posts'
import './App.css';

const App = () => {
  return (
    <div className="App">
      <div className="App-header">
        <h2>Posts</h2>
      </div>
      <Posts />
    </div>
  )
}

export default App;
```

Finally, run `npm start` and that will open the browser window and there you can see this app running.

```zsh
npm start
```
I hope you have understood what is react-redux, it's core functions/components and you can check out the whole application code [here](https://github.com/mountainfirefly/sample-posts-app) in case you or I have missed something.

Thanks for reading. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
