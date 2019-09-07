---
title: "Part 2: Build and run a Node-Mongo App with Docker Compose"
date: "2019-09-08T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/build-and-run-node-app-with-docker-compose/"
category: "Docker"
tags:
  - "Programming"
  - "Javscript"
  - "NodeJs"
  - "Deployment"
  - "Docker"
  - "Container"
  - "Compose"
description: "In this post, will be get to know about docker compose and what is the difference between the docker and docker compose. To getting hands with it we'll be building a node-mongo application."
---
![Docker-Compose](/media/post8-image1.png)

In the last post, I have explained about docker and built a node application with it. If you have no idea about docker then you can check that out here [About Docker](https://mountainfirefly.dev/posts/build-and-run-node-app-with-docker/).

If you can know what is Docker and how does it works then let's roll with Docker Compose.

### Introduction to Docker Compose
Docker-compose is a tool that helps with the management of several different containers at once. If your docker application includes more than one container (for example, a web server, a backend server and database running in the separate containers), building, running, and connecting the containers from separate Dockerfiles is inconvenient and time-consuming. Docker-compose solves this problem by allowing you to use a YAML file to define multiple containers. Then, with a single command, you create and start all the containers from your configuration.

#####Using a Compose is a three-step process:
- Define your app's environment with a `Dockerfile` so it can be reproduced anywhere.
- Define your services that make up your app in `docker-compose.yml` so they can be run together in the isolated environments.
- Run `docker-compose up` and Compose runs and starts your entire app.

#####Compose has commands for managing the whole lifecycle of your application:
- Start, stop and rebuild services
- View the status of running services
- Stream the log output of running application
- Run a one-off command on a service

###Difference between Docker and Docker Compose
The `docker` is used to manage individual containers on the Docker engine. Docker is a tool which is used by developer and operation teams to create an automated deployment of the application in a lightweight environment so that application work efficiently in a different environment.

The `docker-compose` is used to manage multi-container applications. Containers run in isolation but can interact with each other. In Docker compose, a user can start all the services with the single command.

### Build a node-mongo app with Docker Compose
Now you have a basic idea about docker-compose, will be building a sample application with NodeJS and MongoDB.

We will create separate isolated containers for NodeJS and MongoDB, and connect both of them with Docker Compose.

If you don't have Docker installed on your machine, follow this guide to install Docker on your machine [Docker Installation](https://docs.docker.com/install/).

You can verify if `docker-compose` is installed on your machine with this command:
```zsh
# For Docker
docker --version

# For docker-compose
docker-compose --version
```
###Create a node-mongo sample app
First, create a directory where all the files are going to live. In this directory, create a `package.json` that is going to describe your app and its dependencies.

```js
{
  "name": "docker-node-mongo-app",
  "version": "1.0.0",
  "description": "It is a sample node-mongo application that is containerizated and deployed using.",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "express": "4.17.1",
    "mongoose": "5.6.13"
  }
}
```

Now you have listed all the dependencies inside the `package.json` file, install them by running this command inside your terminal:

```zsh
# If you are using yarn, you can run `yarn install`.
npm install
```

Then, will create an `app.js` file that defines a node application using [Express.js](https://expressjs.com/) framework. Also, we will `mongoose` package to connect to MongoDB inside this file.

```js
const express = require('express')
const mongoose = require('mongoose')
const app = express()
const port = 8080

let mongoServiceRuning = false

// fix depreciation warning.
mongoose.set('useFindAndModify', false);
mongoose.set('useNewUrlParser', true);

// connect to mongodb
mongoose.connect('mongodb://mongo:27017/docker-node-mongo-app', function (err) {
  console.log('mongodb connected ?', err ? false : true);
  mongoServiceRuning = err ? false : true 
});


app.get('/', (req, res) => {
  res.send(`Node application containerised using docker that is ${mongoServiceRuning ? 'successfully connected to mongoDB' : 'unable to connect to mongoDB.'}`)
})

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
This application will be listening on port `8080`. You can see inside this file, there is flag called `mongoServiceRuning` app use this flag to track if this application has successfully connected to MongoDB based on the value `true` and `false`.

Also, you must be wondering about this `mongodb://mongo:27017/docker-node-mongo-app` why this app is using `mongo` rather than `localhost` because here MongoDB will be running as separate service in the separate container and inside docker-compose YAML file will be exposing that service as `mongo`.

Now we have created a node-mongo application, in the next steps, we will going to run this application inside the Docker containers.

####Create a Dockerfile
In this step, you write a Dockerfile that will set up the app's environment. This image contains all the dependencies that are required to run the application inside the docker container.

In this directory, create a file called `Dockerfile` and paste the following:

```dockerfile
FROM node:10

# Create a work dir
WORKDIR /user/src/app

# Copy package.json and package-lock.json in docker container
COPY package*.json ./

# Install all the dependencies for the application
RUN npm install

COPY . .

# Your app binds to port 8080 so you will have use EXPOSE instruction to have it mapped with docker daemon: 
EXPOSE 8080

# CMD command defines your app runtime.
CMD ["node", "app.js"]
```

This tells Docker to:
 - Use Nodejs image version 10.
 - Next, it defined WORKDIR, this is where the app is going to live inside the container.
 - Copy `package.json` inside the docker container.
 - Then, run `npm install` inside the docker container to install app's dependencies.
 - Copy all the files from the current directory to docker WORKDIR.
 - Your app is running on port 8080 inside the docker container, with this command this port gets mapped to docker daemon.
 - Finally, with CMD command defines your app runtime.

Create a `.dockerignore` file to ignores files that you don't want to copy inside the docker container. For example:

```
node_modules
npm-debug.log
```
####Define services in a Docker Compose file
Create a file called `docker-compose.yml` in your project directory and paste the following:
```yml
version: '3'
services:
  web:
    container_name: docker-node-mongo-app
    restart: always
    build: .
    ports:
      - '8000:8080'
    links:
      - 'mongo'
  mongo:
    container_name: mongo
    image: mongo
    ports:
      - '27017:27017'
```
This compose file defines two services `web` and `mongo`.

#####`web` service
The `web` services use the image that's going to build from `Dockerfile` in the current directory. It then binds the container (`8080`) and host machine (`8080`) to the exposed port, `8000` and `8080`. Also, it links this service to the `mongo` service as well.

#####`mongo` service
The `mongo` service uses a public MongoDB image pulled from the Docker Hub registry.

####Build and run this app with Docker Compose
From your project directory, start up your application by running `docker-compose up`.

```zsh
# Use `-d` option to run it in the detached mode.
docker-compose up -d
```
Compose pull the MongoDB and NodeJS image, builds an image for your code, and starts the defined services. In this case, the code statically copied to the image in build time.

To test your app, open [http://localhost:8000](http://localhost:9000) inside your browser. And you hopefully see this message in the web page:
```
Node application containerised using docker that is successfully connected to mongoDB
```

Use the following command to see if docker containers are running:
```zsh
$ docker container ls
CONTAINER ID           IMAGE                      CREATED            STATUS              PORTS
cee722aab6d7           docker-node-mongo-app_web  2 minutes ago      Up About a minute   0.0.0.0:9000->8080/tcp
7f929fe48f00           mongo                      2 minutes ago      Up About a minute   0.0.0.0:27017->27017/tcp
```

You can bring everything down, remove the container entirely, with the `docker-compose down` command as following:
```zsh
docker-compose down
```

***NOTE***: `docker-compose up` never rebuilds an image. Rebuild your image by the following command:
```zsh
docker-compose build
```
At this point, you have seen the basics of how the Docker Compose works.

Check out the application source code [here](https://github.com/mountainfirefly/docker-node-mongo-app).

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)