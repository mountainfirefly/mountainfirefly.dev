---
title: "State Management"
date: "2019-09-29T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/state-management/"
category: "Javascript"
tags:
  - "Programming"
  - "Javascript"
  - "State"
  - "Management"
description: "In this post, we will discuss what is state and state management and why it is important and what the tools/packages out there to manage state in Javascript applications."
---
![State Management](/media/post11-image1.png)

Before you start reading it, I just want to say, this what I think about state management personally and if want to correct anything in this article or you have any suggestions, I am happy to receive those. You can post your feedback on my Twitter post.

### What is State?
The state is all the information that is retrained by your application, often concerning previous events or interaction. The application is conceived of as acting by executing methods/functions that change the current state to another state.

For example, a user has landed on your application and suppose your application shows the list of restaurants nearby you. So the list of restaurants is the part of the state, every application which wants a user to do interact with it, it has to use the state. It can't work without it.

Most people think that state is only used by the react/redux applications because there they see the term `state` but that is not true, the state can be part of any programming language applications as long as it let the user interact with it.

### What is State Management?
Definition of State Management refers to the management of the state of one or more user interfaces controls like text fields, buttons, radio buttons, checkboxes, models, etc. in the graphic user interface.

State management is a form data-structure that you can read and write to. It makes your invisible state visible for you to work with.

In front-end web development, the DOM has inherent ability to manage your state of form without you having to do anything (i.e it remembers the user inputs). There are different ways of getting/setting state and you may use tools that allow you to manage your application state differently (such as Angular's $scope, React's component state, Redux's state management tool).

For example, you have an HTML form with several input boxes in it and submit button which is disabled if the form doesn't have valid data. So here, as you can see with the involvement of state management we can disable the submit button while our data is invalid. Also, suppose you type anything in the input box it will be tracked down with the help of state management and DOM is doing that tracking and state management for you here.

### Why state management is important?
The main reason I think personally if you want the user to interact with your application then you have to use state management, it can be DOM, React's component, Redux, etc doing the state management for you.

- Working with API's
- For taking user input
- Caching data in local-storage
- Perform actions on your application etc...

All it means, it gives life to your application without it, it is just an HTML webpage which displays some information and all the user can do is read what is there on the page. The user can scroll that page because the browser uses the state management when user scroll on the touchpad of their system the browser fires an event.
So State management is everywhere and you can't deny it.

### State Management Tools
These are most popular tools for State management these days for Javascript/React applications.

- [Redux](https://redux.js.org/)
- [MobX](https://mobx.js.org/)
- [MobX State Tree](https://github.com/mobxjs/mobx-state-tree)
- [freactal](https://github.com/FormidableLabs/freactal)
- [Unistore](https://github.com/developit/unistore)
- [Vuex](https://vuex.vuejs.org/guide/)

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
