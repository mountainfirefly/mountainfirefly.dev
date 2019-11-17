---
title: "Learn about HTTP server"
date: "2019-11-03T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-about-http-server/"
category: "NodeJS"
tags:
  - "Programming"
  - "Javscript"
  - "Deployment"
  - "Request"
  - "Internet"
  - "HTTP"
  - "Server"
  - "NodeJS"
description: "In this post, we will learn about the HTTP server, how we can handle client requests and create server routing so that users can request a predictable response. We will be implementing this with http NodeJS module without using it any framework."
---
![HTTP Server](/media/post14-image1.png)

To comprehend what is an HTTP server, let's first understand what is a server, so the server is a computer program or device that provides functionalities to other computer programs or devices that are called clients. The server can provide various functionalities, often called services, such as sharing data or resources among different clients.

### Introduction to HTTP Server
HTTP is everywhere. Every website we visit it runs over an HTTP server. You may think then what about the HTTPS server. Technically, HTTPS is the same as HTTP with more security.

HTTP Server takes care of client requests and sends appropriate responses to them. In NodeJS, we have a couple of built-in HTTP servers like Apache, Nginx, etc. In this article, we are going to create an HTTP server so that we comprehend it.

Building an HTTP server in JAVA and another language framework is a tough task, that why every platform provides different types of built-in HTTP servers. However, in NodeJS, we can develop an HTTP server very easily, it doesn't require many efforts. We don't need to write a lot of code to develop a setup a basic HTTP server.

NodeJS provides a build-in module called HTTP, which allows NodeJS to transfer data over the HyperText Transfer Protocol(HTTP).

To include this module in your JS code, use `require()`
```js
var http = require('http')
```

### How to set up a simple HTTP server
Here we will learn how to create a simple HTTP server step by step.

Let us start by creating a directory for our NodeJS project

```zsh
mkdir http-server
cd http-server
```
Create a `packge.json` with below content or use `npm init` command and answer some questions and it will create `package.json` automatically for you.

```zsh
npm init
```
or create `package.json` file put this content inside it.

```js
{
  "name": "http-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "mountainfirefly <pankajdcoder@gmail.com>",
  "license": "MIT"
}
```

With `http` module, we can create an HTTP server that listens to client requests and response them back.

Use `createServer()` method to create a HTTP server.

Create a `server.js` file and put below content inside that.
```js
var http = require('http')

var server = http.createServer((request, response) => {
  response.write('Welcome to your HTTP server!')
  response.end()
})

server.listen(3000)
```
`createServer()` function creates an HTTP server and it is listening on port `3000` as mentioned in the `server.js`, you can change this port number according to your need.

Inside `createServer()` it taking a callback function which is giving a two params `request` and `response` and there we are writing a message on the response which is `Welcome to your HTTP server!` and ending it. So basically what is doing whenever you open `http://localhost:3000/` inside your web client(chrome, firefox, etc.) you will see a message on your screen `Welcome to your HTTP server!`.

For example, in case you open `https://facebook.com` in your web client, facebook server returns webpage which facebook page and in our case we returning a message. We can return whatever we want but for now, let's keep it simple.

Run your server by running `node server.js` in your terminal
```zsh
node server.js
```

### How to do routing with HTTP Server
So far we have created an HTTP server that is returning the same for all the requests. Suppose if we are hitting `http://localhost:3000/test` it is returning the same message that is `Welcome to your HTTP server!`.

To make HTTP server handles different type of requests we need to implement routing on the server. Which check the request and returns an appropriate response to it.

Let's create routing for HTTP Server

```js
var http = require('http')

var server = http.createServer((request, response) => {
  switch(request.url) {
    case '/test':
      response.write('Testing')
      break;
    case '/hello':
      response.write('Hello, Pankaj Thakur')
      break;
    default:
      response.write('Welcome to your HTTP server!')
  }
  response.end()
})

server.listen(3000)
```

In the above code, we are using a `switch` statement for implementing routing. Inside the `switch` statement we are passing `request.url` which contains the client requested Url. Suppose if a client sends a request by `http://localhost:8080/` then `url` will be `/` and it will match default case in the above and return message `Welcome to your HTTP server!`.

If a user sends a request by `https://localhost:8080/hello` then on the web client it will show `Hello, Pankaj Thakur` message on the screen.

You can check it by yourself, you just need to update the above code in the `server.js` and then restart your server.

```zsh
node server.js
```

So this is all about the HTTP server, I hope you must have enjoyed this article. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

You can check out the full sample application code [here](https://github.com/mountainfirefly/sample-http-server).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
