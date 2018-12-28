# React & Express Development Environment and Deployment

## Development Environment

1. make server project
    ~~~
    mkdir server
    cd server
    npm init
    ~~~

1. install express
    ~~~
    npm install --save express
    ~~~

1. make index.js
    ~~~javascript
    const express = require('express');

    const app = express();

    app.get('/server', (req, res) => {
        res.send('<h1>server !!!</h1>');
    });

    app.get('/api/a', (req, res) => {
        res.send('<h1>server api a !!!');
    });

    app.get('/api/b', (req, res) => {
        res.send('<h1>server api b !!!');
    });

    const PORT = process.env.PORT || 5000;

    app.listen(PORT); 
    ~~~

1. start express server.
    ~~~
    node index.js
    ~~~

1. make sure that the server works by accessing localhost:5000/server with web browser.

1. make client project under the server project folder.
    ~~~
    create-react-app client
    ~~~

1. change working directory to client and delete all files in client/src folder.
    ~~~
    cd client
    rm src/*
    ~~~

1. install http-proxy-middleware, react, react-dom, react-router and react-router.dom
    ~~~
    npm install http-proxy-middleware react react-dom \
                react-router react-router-dom --save
    ~~~

1. make src/index.js or src/index.jsx
    ~~~javascript
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';

    ReactDOM.render(<App />, document.getElementById('root'));
    ~~~

1. make src/App.js or src/App.jsx
    ~~~javascript
    import React from 'react';
    import { BrowserRouter, Route, Switch } from 'react-router-dom';

    const Root = () => <h2>Root</h2>;
    const RootContent = () => <h2>Root Content</h2>;
    const Client1 = () => <h2>Client1</h2>;
    const Client2 = () => <h2>Client2</h2>;
    const NotFound = () => <h2>NotFound</h2>;

    const App = () => {
      return (
        <div>
          <BrowserRouter>
            <div>
              <Root />
              <Switch>
                <Route exact path="/" component={RootContent} />
                <Route exact path="/client1" component={Client1} />
                <Route exact path="/client2" component={Client2} />
                <Route exact path="*" component={NotFound} />
              </Switch>
            </div>
          </BrowserRouter>
        </div>
      );
    };

    export default App;
    ~~~

1. make src/setupProxy.js
    ~~~javascript
    const proxy = require('http-proxy-middleware');

    module.exports = function(app) {
      app.use(proxy('/server', { target: 'http://localhost:5000' }));
      app.use(proxy('/api/*', { target: 'http://localhost:5000' }));
    };
    ~~~

1. change the working directory to server project folder.
    ~~~
    cd ..
    ~~~
    and make sure tha you're in the server project folder using pwd command

1. install concurrently and nodemon
    ~~~
    npm install concurrently nodemon --save
    ~~~

1. edit package.json in the server project folder like the following.
    ~~~json
   "scripts": {
     "start": "node index.js",
     "server": "nodemon index.js",
     "client": "npm run start --prefix client",
     "dev": "concurrently \"npm run server\" \"npm run client\" "
    }
    ~~~

1. run servers
    ~~~
    npm run dev
    ~~~
    then, all of the following urls will work properly.
    - http://localhost:3000/
    - http://localhost:3000/client1
    - http://localhost:3000/client2
    - http://localhost:3000/server
    - http://localhost:3000/api/a
    - http://localhost:3000/api/b

# Deployment to Production

1. stop the servers that started in the previous section.

1. in the server folder, build the react client app.
    ~~~
    npm run build --prefix client
    ~~~
    this will make client/build folder and the required static resources in it.

1. edit index.js or index.jsx file like this.
    ~~~javascript
    const express = require('express');
    const path = require('path');

    const app = express();

    app.get('/server', (req, res) => {
      res.send('<h1>server !!!</h1>');
    });

    app.get('/api/a', (req, res) => {
      res.send('<h1>server api a !!!');
    });

    app.get('/api/b', (req, res) => {
      res.send('<h1>server api b !!!');
    });

    if (process.env.NODE_ENV === 'production') {
      app.use(express.static('client/build'));

      app.get('*', (req, res) => {
        res.sendFile(path.resolve(__dirname, 'client', 'build', 'index.html'));
      });
    }

    const PORT = process.env.PORT || 5000;

    app.listen(PORT);
    ~~~
1. set the environment variable NODE_ENV to production and start express server.
    - in bash shell
        ~~~
        export NODE_ENV=production
        npm run server
        ~~~
    - in fish shell
        ~~~
        set -gx NODE_ENV production
        npm run server
        ~~~
    then, all of the following urls will work properly.
    - http://localhost:5000/
    - http://localhost:5000/client1
    - http://localhost:5000/client2
    - http://localhost:5000/server
    - http://localhost:5000/api/a
    - http://localhost:5000/api/b

## Optional : Heroku Deployment

1. edit the scripts section of the package.json in the server project folder.
    ~~~json
    "scripts": {
        "start": "node index.js",
        "server": "nodemon index.js",
        "client": "npm run start --prefix client",
        "dev": "concurrently \"npm run server\" \"npm run client\" ",
        "heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix client && npm run build --prefix client"
    },
    ~~~

1. do git push
    ~~~
    git push heroku master
    ~~~
