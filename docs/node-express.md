# NodeJS / Express ⌛

[Return to Table of Contents](../README.md)

## Content ✈️ 

- [**Useful middlewares**](#useful-middlewares)
- [**Boilerplate**](#boilerplate)

## **Boilerplate**

- ### NodeJS / Express - boilerplate

```javascript
'use strict';
require('dotenv').config();
const createError = require('http-errors');
const express = require('express');
const passport = require('passport');
const expressSession = require('express-session');
const path = require('path');
const cookieParser = require('cookie-parser');
const initRouter = require('./routes/initRouter');
const corsMiddleware = require('./middlewares/cors');
const UserService = require('./services/user');
const FileService = require('./services/file');
const AddUserId = require('./middlewares/getUserId');

// init AWS s3 bucket

// init DB
require('./config/db');

const app = express();

app.use(corsMiddleware);
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));
app.use(expressSession({
  secret: process.env.SECRET_KEY,
  resave: false,
  saveUninitialized: true
}));

//Init Passport Middleware

app.use(passport.initialize({}));
app.use(passport.session({}));

require('./config/passport')(passport, new UserService());

// init Router

initRouter(app);

// catch 404 and forward to error handler
app.use((req, res, next) => {
  next(createError(404));
});

// error handler
app.use((err, req, res) => {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```

## **Useful middlewares**

- ### CORS middleware.

> Use if to prevent CORS errors, while making requests.

```javascript
'use strict';

module.exports = (req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept, Authorization');
  next();
};
```
