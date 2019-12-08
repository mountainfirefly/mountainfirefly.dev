---
title: "Create React Node boilerplate"
date: "2019-12-08T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/create-react-node-boilerplate/"
category: "NodeJS"
tags:
  - "ReactJS"
  - "Javascript"
  - "NodeJS"
  - "Webpack"
  - "Babel"
  - "Programming"
  - "Boilerplate"
description: "In this post, In this article, we will be setting up a full-stack boilerplate using React, Webpack, Babel, and NodeJs. I will be taking you through each step of React-Node-Boilerplate creation."
---
![Boilerplate](/media/post17-image1.png)
In this article, we will be setting up a full-stack boilerplate using React, Webpack, Babel, and NodeJs. I will be taking you through each step of React-Node-Boilerplate creation.

Let’s not wait and get to work.

First I will just give a short introduction to the ingredients that will be used.

#### React
It is a library for building user interfaces and it is maintained and developed by the Facebook community. For more information about React, you can check out the official [documentation](https://reactjs.org/).

#### NodeJs
NodeJS is an open-source, cross-platform, Javascript runtime environment that executes Javascript outside the browser. For more information please check the official [documentation](https://nodejs.org/en/).

#### Webpack
Webpack is an open-source Javascript module bundler and primary used for Javascript but it can transform any frontend asset like HTML, CSS, and images if corresponding loaders are included.

#### Babel
It is an open-source Javascript compiler that is mainly used for convert ECMAJavascript 2015+ into a backward-compatible version of Javascript.

Now you have a basic idea of all the ingredients that will be used in the boilerplate creation, now let’s start with the coding.

Create a directory for our boilerplate and create a package.json file.

```zsh
mkdir react-node-boilerplate
cd react-node-boilerplate
yarn init
```

`yarn init` will create a `package.json` inside this directory. Now, let's add all the required packages for this boilerplate.

```zsh
yarn add react react-dom express nodemon wepback @babel/core @babel/preset-env @babel/plugin-transform-react-jsx @babel/preset-react babel-loader css-loader webpack webpack-cli webpack-dev-middleware webpack-hot-middleware mini-css-extract-plugin sass-loader css-loader node-sass file-loader express-static-gzip @babel/plugin-proposal-class-properties ejs
```

These packages I know will be needed for this boilerplate if something is missed will add that later.

Now, let's set up our start-server script file. Create a directory `bin` and `www` file under that and put the below content inside that.

To create a directory and file run following commands:
```zsh
mkdir bin
cd bin
touch www
```

`www` file content:
```js
var http = require('http')

var port = process.env.PORT || '3000'

var server = http.createServer()

server.listen(port, () => console.log('server listening on port', port))
```

Inside your `package.json` add/replace your `scripts` object with below content:

```js
"scripts": {
  "start": "NODE_ENV=development nodemon ./bin/www",
  "start:prod": "NODE_ENV=production nodemon ./bin/www"
},
```

Here, you can see we are using nodemon to start our start because we don't want to restart our server whenever we change a file. So, it monitors changes in the source code and restarts your server automatically, like a hot reloader.

Start your server by running the following command:
```zsh
yarn start
```

You can see in the command line that your basic HTTP server is running.

Create entry point for our express application `app.js`
```js
var express = require('express')
var path = require('path')
var expressStaticGzip = require("express-static-gzip");

var app = express()
var indexRouter = require('./routes/index') // This one is for our frontent application
var apiRouter = require('./routes/api') // This one is for our backend APIs

// view engine setup
app.set('views', path.join(__dirname, 'views'))
app.set('view engine', 'ejs')

app.use(express.json())
app.use(express.urlencoded({ extended: false }))
app.use(express.static(path.join(__dirname, 'public')));

app.use('/dist/bundle', expressStaticGzip(path.join(__dirname, 'dist/bundle'), {
  enableBrotli: true,
  orderPreference: ['br', 'gz'],
  setHeaders: function (res, path) {
    res.setHeader("Cache-Control", "public, max-age=31536000")
  }
}));

// Using webpack for our development
if (process.env.NODE_ENV === "development") {
  var webpack = require("webpack");
  var webpackConfig = require("./webpack.config");
  var compiler = webpack(webpackConfig);

  app.use(
    require("webpack-dev-middleware")(compiler, {
      noInfo: true,
      publicPath: webpackConfig.output.publicPath
    })
  );

  app.use(require("webpack-hot-middleware")(compiler))
} else {
  var webpack = require("webpack");
  var webpackConfig = require("./webpack.config.prod");
  var compiler = webpack(webpackConfig);

  app.use(
    require("webpack-dev-middleware")(compiler, {
      noInfo: true,
      publicPath: webpackConfig.output.publicPath
    })
  );
}

console.log('Coming here too indexroute', indexRouter, apiRouter)
// Route handler
app.use('/api/v1', apiRouter) // api route handler
app.use('/', indexRouter); // react handler

module.exports = app
```

##### Above code explanation
- Importing express module for creating APIs.
- `app` variable is the instance of the express module.
- import routes file that we will be creating later in this article.
- Setup view engine using `app.set` function.
- Next, we have defined our `/dist/bundle` route which is rendering the content of the `dist/bundle` file. We are using the `expressStaticGzip` module for compressing our bundle file.
- For the development of the react application and creating a production bundle file for it, we are using a webpack.
- At last, we have rendered our router handler that will be creating later in this article.

Let's create `webpack.config.js`, `webpack.config.prod.js` and `.babelrc` files for webpack configuration under your project directory.

`webpack.config.js` for development mode.
```js
var webpack = require('webpack')
var path = require('path')
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

module.exports = {
  mode: 'development',
  devtool: 'inline-source-map',
  entry: [
    './client/index.js',
  ],
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: { loader: 'babel-loader' },
      },
      {
        test: /\.(scss|css)$/,
        use: [
          { loader: MiniCssExtractPlugin.loader },
          {
            loader: 'css-loader',
          },
          { loader: 'sass-loader' }
        ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'file-loader',
            options: {}
          }
        ]
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  },
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist/bundle/',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.DefinePlugin({
      'process.env': {
        'NODE_ENV': JSON.stringify('development')
      }
    }),
    new MiniCssExtractPlugin({
      filename: "bundle.css",
    })
  ]
}
```
`.babelrc` config file:
```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  "plugins": [
    "@babel/plugin-proposal-class-properties"
  ]
}
```

`webpack.config.prod.js` for production mode:
```js
var webpack = require('webpack')
var path = require('path')
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

module.exports = {
  mode: 'production',
  devtool: 'inline-source-map',
  entry: [
    './client/index.js',
  ],
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: { loader: 'babel-loader' },
      },
      {
        test: /\.(scss|css)$/,
        use: [
          { loader: MiniCssExtractPlugin.loader },
          {
            loader: 'css-loader',
          },
          { loader: 'sass-loader' }
        ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'file-loader',
            options: {}
          }
        ]
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  },
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist/bundle/',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env': {
        'NODE_ENV': JSON.stringify('production')
      }
    }),
    new MiniCssExtractPlugin({
      filename: "bundle.css",
    })
  ]
}
```
We have created two webpack configuration files one is for development env and another one is the production env.

In `webpack.production.js` file you can add plugins to reduce more size of the bundle file or other configuration according to your needs.

Now, create a `client` directory for react application and put a sample component inside it.

Follow all the below-mentioned steps to create the sample react app.
```zsh
mkdir client
cd client
touch index.js
mkdir components
mkdir scss
cd scss
touch index.scss
```
Put below code inside your `index.js` file.
```js
import React from 'react'
import ReactDOM from 'react-dom'

import App from './components/App'
import './scss/index.scss'

ReactDOM.render(
  <div>
    <App /> 
  </div>
  ,
  document.getElementById('root')
);
```
Put below code inside `index.scss` file.
```scss
body {
  margin: 10x;
}

h1 {
  text-align: center;
  color: gray;
  margin-top: 50px;
}
```

Now create `App.js` file inside the `components` directory that we are importing under our `index.js` file.
`App.js`
```js
import React from 'react'

const App = () => {
  return (
    <div>
      <h1>React-Node-Boilerplate</h1>
    </div>
  )
}

export default App
```
The sample code for our frontend is done. Now, will move on to create routes for rendering backend APIs and frontend bundle file.

Create a `routes` directory and under that `API` directory for creating and managing APIs.

```zsh
mkdir routes
cd routes
touch index.js
mkdir api
cd api
touch index.js
```

Put below code inside `routes/index.js` file, this router file is for rendering the frontend (react-application).
```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('*', function(req, res, next) {
  res.render('index', { title: 'React-Node-Boilerplate' })
});

module.exports = router
```

For rendering backend APIs, open `routes/api/index.js` file and put below content inside it.

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function (req, res, next) {
  res.json({ success: true, message: 'Welcome to Node APIs' });
});

module.exports = router;
```

We have created a home there, for now, please add routes according to your requirement inside this file.

Routing part is done too, so will move on creating `views` that are for express templates. Create a `views` directory and `index.ejs` file inside it.

Execute below commands inside your terminal
```zsh
mkdir views
cd views
touch index.ejs
```
Put below code inside `index.ejs` file
```js
<!DOCTYPE html>
<html>

<head>
  <title><%= title %></title>
  <link rel="shortcut icon" href="https://cdn2.iconfinder.com/data/icons/nodejs-1/512/nodejs-512.png">
  <link rel='stylesheet' href="/static/bundle.css" />
  <meta content="width=device-width, initial-scale=1" name="viewport" />
  <!-- Global site tag (gtag.js) - Google Analytics -->
</head>

<body>
  <div id="root"></div>
  <script src="/static/bundle.js"></script>
</body>

</html>
```

At last, replace your `bin/www` code with below code:
```js
#!/usr/bin/env node

var app = require('../app');
var http = require('http')

var port = process.env.PORT || '3000'
app.set('port', port);

var server = http.createServer(app)

server.listen(port, () => console.log('server listening on port', port))
```

Here, we are importing the whole app and passing it to the `createServer()` function and it is returning an instance of the server under server variable and that is listening on to `3000` port.

We have set up everything that is needed for react-node-boilerplate. Now run `yarn start` to see it working.
```zsh
yarn start
```

Open `http://localhost:3000/` in your browser to see your react application working in development mode.

Open `http://localhost:3000/api/v1/` to see backend APIs of the application, right now we only have `/` route which is rendering a JSON object `{"success":true,"message":"Welcome to Node APIs"}` on the screen.

To start a production server run this command in your terminal:
```zsh
yarn start:prod
```
In case if you facing any issues please check out the working application code [here](https://github.com/mountainfirefly/react-node-boilerplate) or you can ask me your questions on [twitter](https://twitter.com/mountainfirefly).

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
