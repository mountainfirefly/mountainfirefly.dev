---
title: "Learn about the Express framework"
date: "2019-12-01T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-about-the-express-framework/"
category: "ExpressJS"
tags:
  - "Programming"
  - "Javascript"
  - "Express"
  - "framework"
  - "Nodejs"
description: "In this post, we will go through express framework definition, how you can install it and learn about routing, how to create routes with the express framework. It completes with, what is express middleware and how you can create global middleware and middleware for specific endpoints."
---
![Express JS](/media/post16-image1.png)

The last article was about the HTTP server, how we can create routes in it without using a framework. You can check it out [here](https://mountainfirefly.dev/posts/learn-about-http-server/).

### What is ExpressJS?
ExpressJS or simply Express is a web application server framework, which is specifically designed for building and provide robust features for single-page, multi-page and hybrid applications.

It is an open-source framework developed and maintained by the NodeJS foundation and it has become a standard web framework for NodeJS applications.

### Installing and using Express
You can install Express by NPM(Node Package Manager). To install express, open your terminal and execute this command.

```zsh
npm install express
```

The above command request Node Package Manager to download the express module from it and install it on your system.

Now, let us use the newly installed Express module and create a Hello world application with it.

This is an example ***Hello World*** application. Create a file called `app.js` and put the below code inside that.
```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

#### Above code explanation
- In our first line of code, we are requiring the express module to use it.
- To use express module functions, we call it and get all the defined functions inside the `app` variable.
- Next, we have defined a `port` variable with value `3000`.
- Now we are using the `get` function to create a route. It takes route path as first param and callback function as a second param, which returns the value that will display on the screen while we open `http://localhost:3000` in the browser. In our case, we are returning `Hello World!` text.
- Finally, we are using `app.listen` function to make our server application to listen to client requests on port number 3000. You can specify any available port here.

Return the above code by executing this command in your terminal.
```zsh
node app.js
```

Then, open `http://localhost:3000` to see ***Hello World!*** text in your browser.

### What is Routes?
Routing determines the way how are an application's endpoints respond to client requests. You define routing using methods that are available in Express app object, for example, app object has `app.get` method for handling GET requests and `app.post` for handling POST requests and so on. It has methods for all types of requests.

For example, a client can make `GET`, `POST`, `PUT` and `DELETE` HTTP requests for the sample URL listed below:
```zsh
http://localhost:3000/books
http://localhost:3000/students
```

- If the client makes `GET` request for the first URL, then the response ideally is a list of books.
- If the client makes `GET` request for the second URL, then the response ideally is a list of students.
- So based on the URL accessed by the client, the different functions on the webserver will be invoked.

Let's create sample routes for better understanding with Express
```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.get('/books', (req, res) => {
  res.send('This page is for displaying list of books.')
})

app.get('/students', (req, res) => {
  res.send('This page is for displaying list of Students')
})

app.route('/student/:studentId')
.get((res, req) => {
  res.send('This page is for displaying perticular student details.')
})
.put((res, req) => {
  res.send('This page is for updating a student details.')
})
.delete((res, req) => {
  res.send('This page is for deleting a student details.')
})

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

This is the general syntax for defining a route
```zsh
app.METHOD(PATH, HANDLER)
```
- `app` is an instance of the express module.
- `METHOD` is an HTTP request method (`GET`, `POST`, `PUT` and `DELETE`).
- `PATH` is the route URL, which is used to identify client requests.
- `HANDLER` is a function, which gets executed when the route URL gets matched.

### Express Middleware
Middleware is literally meant you put anything in the middle of the layer of the software. Express middleware the functions which execute during the lifecycle of a request. Each middleware has access to the HTTP request and response to each path it is attached to.

Middleware functions can execute these tasks:
- Execute any code.
- Make a change to the HTTP request and response.
- End the request-response cycle.
- Call the next middleware in the stack.

##### Let's set up a global logger middleware
It means it will be called for every incoming request.

Here is an exmaple:
```js
const express = require('express')
const app = express()
const port = 3000

app.use((req, res, next) => {
  console.log('Req', req, 'LOGGED')
  next()
})

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

We use `app.use` to call this middleware on every call to the application. So when a user tries to access the `/` route, it will first print `console.log` statement in the terminal from the middleware function.
##### Let's setup middleware for specific endpoints
It means it will be called for specific incoming requests only.

Here is an exmaple:
```js
const express = require('express')
const app = express()
const port = 3000

const requireJSONContent = () => {
  return (req, res, next) => {
    if (req.headers['Content-Type'] !== 'application/json') {
      res.status(400).send('Server requires application/json')
    } else {
      next()
    }
  }
}

app.get('/', (req, res) => res.send('Hello World!'))

app.post('/students', requireJSONContent(), (req, res) => {
  res.send('Add JSON content.')
})

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

As you can see above, we didn't define our `requireJSONContent` middleware with `app.use` because it listens to every incoming request. We wanted to use `requireJSONContent` middleware for the specific endpoints, as above mentioned code it has been only used for `POST` `/students` endpoint.

So, this is it for this article, I hope it helps you understand the express framework. If you want to learn more about Express framework you can check out the official guide of [express framework](https://expressjs.com/).

I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).
