# express-promise-json-router

Based on [express-promise-router](https://raw.githubusercontent.com/alex-whitney/express-promise-router).

A simple wrapper for Express 4's Router that allows middleware to return promises. This package makes it simpler to
write route handlers for Express when dealing with promises by reducing duplicate code.

## Getting Started
Install the module with: `npm install express-promise-json-router --save`.

`express-promise-json-router` is a drop-in replacement for Express 4's Router.


## Documentation

Middleware and route handlers can simply return a promise. If the promise is rejected, ```express-promise-json-router``` will
call ```res.status(err.statusCode || err.status).json(err)``` with the reason. This functionality removes the need to explicitly define a rejection handler.

```javascript
// With Express 4's router
var router = require('express').Router();

router.use('/url', function (req, res, next) {
    Promise.reject().catch(next);
})

// With express-promise-json-router
var router = require('express-promise-json-router')();

router.use('/url', function (req, res) {
    return Promise.reject(); // will throw 500
})

router.use('/url', function (req, res) {
    return Promise.reject({statusCode: 403}); // will throw 403 error. Also supports status instead of statusCode
})
```

When the returned promise is resolved, its value will be used to call ```res.status(data.statusCode || data.status).json(data)```
```javascript
router.use('/url', function (req, res) {
    // equivalent to calling next()
    return Promise.resolve({statusCode: 201, otherInfo: 1}); // will result in a 201 response with the object passed in the response
});
```

This package still allows calling ```next``` directly.
```javascript
router = require('express-promise-json-router')();

// still works as expected
router.use('/url', function (req, res, next) {
    next();
});
```

## License
Licensed under the MIT license.
