---
title: "Build and run Node App with Docker"
date: "2019-09-01T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/build-and-run-node-app-with-docker/"
category: "Docker"
tags:
  - "Programming"
  - "Javscript"
  - "NodeJs"
  - "Deployment"
  - "Docker"
  - "Container"
description: "In this post, I'll give an introduction about docker, docker containers and docker commands. Also going to explain the difference between containerization and virtualization. At last, we will build and run a node application with docker."
---
![Docker](/media/post7-image1.png)

Firstly, let's have a quick understanding of docker and container then will move on to the building of a node app in the docker container.

###What is Docker?
Docker is a tool designed to make it easier to create, deploy and run an application using containers. Containers help the developer to package up the whole application (including its dependencies, libraries and other things it needs to run the application) as one package.

Docker is a bit like a virtual machine. But unlike a virtual machine, rather than creating the whole virtual operating system, Docker allows applications to use the same Linux kernel as the system they are running on. This gives the application a significant performance boost and reduces the size of the application.

Docker benefits both developer and system administrators, by making it the part of DevOps (Developer + operations) toolchain. So, developers can focus on writing code without worrying about the system that it will ultimately be running on.

###What is a Container?
Container contains applications in a way that keep them isolated from the host system they are running on. Containers allow a developer to package up the application with all its parts it needs, such as libraries and other dependencies, and ship it all as one package. And they are designed to make it easier for developer and system administrator to push their application code from dev environment to a staging or production environment in a fast and replicable way. In a way, containers behave like a virtual machine. To the outside world, they can look like a complete system.

###Here's is a list of docker commands
- `docker run` - Runs a command in a new container.
- `docker start` - Starts one or more stopped containers.
- `docker stop` - Stops one or more running containers.
- `docker build` - Builds an image from the Docker file.
- `docker pull` - Pulls an image or repository from a registry.
- `docker push` - Pushes and image or repository to a registry.
- `docker export` - Export a container's filesystem as a tar archive.
- `docker exec` - Runs a command in a run-time container.
- `docker search` - Searches the Docker Hub for images.
- `docker attach` - Attaches to a running container.
- `docker commit` - Create a new image from a container's changes.

Check out the complete list of commands in the [Docker documentation](https://docs.docker.com/engine/reference/commandline/docker/)

###Containerization vs Virtualization
***Containerization***: is a lightweight alternative to the full machine virtualization that involves encapsulating an application to the container with its operating system.

Containers use the same host OS, instead of installing OS for each guest VM.

***Virtualization***: A virtual machine is a complete copy of the operating system, this means replication of binaries, drivers and libraries between the different VMs running on the same server and wastage of significant server resources.

Containerization provides isolation to run your application while sharing the same OS resources and on the other hand, virtualization provides the same functionality but has its own OS so VM systems can run on a different operating system and VM can host multiple applications versus a container that will normally have a single application running on it.

For example, if we have a 2 node API running on different node version it may possible if we deploy them in VM they may break but if we deploy them in containers they are completely isolated from each other.

###Build Node app with Docker
Now you know what is docker and its benefits, we will build a node application with it.

Follow this guide to install Docker on your machine [Docker Installation](https://docs.docker.com/install/). Once you have installed docker on your machine you can move ahead.

Firstly, we are going to create a sample node application and then we will build a docker image for it, and lastly, we will instantiate a container from that image.

####Create the Nodejs app
First, create a directory where all the files are gonna store. In this directory create a `package.json` file that describes your app and its dependencies:

```js
{
  "name": "docker-node-app",
  "version": "1.0.0",
  "description": "It is a sample node application that is containerised and deployed using docker.",
  "main": "index.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "4.17.1"
  }
}
```
Install application dependencies by following command:

```
npm install
```

Then, create a `app.js` file that defines a web app using [Express.js](https://expressjs.com/) framework:

```js
const express = require('express')
const app = express()
const port = 8080

app.get('/', (req, res) => res.send('Node application containerised using docker.'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```
Now we created the sample Nodejs application, in the next steps, we will run this application inside the docker container. First, you need to build a docker image for your application.

####Create a Dockerfile
Create an empty file called `Dockerfile`:
```zsh
touch Dockerfile
```
Open `Dockerfile` inside your editor.

Each `Dockerfile` must begin with a `FROM` instruction. The first thing we need to do is define the application's base image, on which our application is going to build. In this case, we are using node to build our image.
```dockerfile
FROM node:10
```
Next, we will create a directory inside the docker container where the application's code is going to live, that is called working directory for the application. We create that with the following instruction:

```dockerfile
WORKDIR /usr/src/app
```

This image comes with Nodejs and NPM already installed so next thing you will install the application dependencies inside the docker image. For that, you need to copy app `package.json` and `package-lock.json` inside the image and with the help of `RUN` instruction we will do `npm install` as mentioned below:

```dockerfile
# A wildcard is used to ensure both package.json and package-lock.json are copied.
COPY package*.json ./

#Install dependencies
RUN npm install
```

To copy to application's source code inside the docker image, use the `COPY` instruction:
```dockerfile
COPY .  .
```
This app is bind to port `8080` so we'll use the `EXPOSE` instruction to have it mapped with the docker daemon.
```dockerfile
EXPOSE 8080
```

Finally, we define the command to run the application using `CMD` instruction which defines application run-time. Here we start our node application like this `node app.js`:
```dockerfile
# CMD command defines your app runtime.

CMD ["node", "app.js"]
```

Your `Dockerfile` should look like this:

```dockerfile
FROM node:10
# Create a work dir
WORKDIR /user/src/app

# Copy package.json and package-lock.json in docker container
COPY package*.json ./

# Install all the dependencies for the application
RUN npm install

COPY .  .

EXPOSE 8080

# CMD command defines your app runtime.
CMD ["node", "app.js"]

```

####Build the image
Go to the directory that has your `Dockerfile` and runs the following command to build the Docker image. The `-t` lets you tag your image with a name so it's easier to find while you run `docker images` command:

```zsh
docker build -t <your username>/docker-node-app .
```
Your image will be listed by `docker images` command:
```zsh
$ docker images

# Example
REPOSITORY                          TAG           ID            CREATED
node                                10            1934b0b038d1  5 days ago
<your username>/docker-node-app     latest        d64d3505b0d2  1 minute ago
```

####Run the image
Run your image with the following command:
```zsh
docker run -it -d -p 9000:8080 <your username>/docker-node-app
```
`-d` flag runs the application in detach mode, that means running the container in the background and `-p` flag redirects the public port `9000` to the private port `8080` inside the docker container.

if you wish to go inside the container, you can use the following command:
```zsh
# Enter in the container
$ docker exec -it <container id> /bin/bash
```

To test your app, open [http://localhost:9000](http://localhost:9000) inside your browser.

Check out the application source code [here](https://github.com/mountainfirefly/docker-node-app)

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
