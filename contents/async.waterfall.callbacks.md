# Convention to use distinctive callback names in async.waterfall

This document applies to the commonly used `async` ([link](https://github.com/caolan/async)) nodejs module and specifically its `waterfall` ([link](https://github.com/caolan/async#waterfall)) method call.

- - -

### Problem:

There are quite a few node projects that use the well known `async.waterfall` method to execute a series of callbacks in a linear fashion. In some cases the `async.waterfall` call is nested in a function body that has its own callback. An example would be like this:

```javascript
function doSomething(foo, bar, cb) {
  async.waterfall([
    function(cb) {
      // ...
    },
    function(returnVal, cb) {
      // ...
    },
    function(returnVal, cb) {
      // ...
    },
    function(returnVal, cb) {
      // ...
    },
    function(returnVal, cb) {
      // ...
    },
  ], function(err, returnValue) {
    // ...
  });
}
```

Notice that both the main `doSomething` function and the functions in `async.waterfall` have the same callback names, referenced as `cb`. Even though node's javascript compiler will correctly call the right callback based on its reference lookup, this piece of code is error prone to humans.

### Proposed convention:

In order to have distinctive callback names, introduce the following naming convention:

* Parent function callbacks are called `callback` (not `cb`, those extra 6 characters won't hurt your fingers but will help people)
* Callbacks that are used with `async.waterfall` are called `next` in order to express linear (sync) execution.

Essentially the example above becomes the following:

```javascript
function doSomething(foo, bar, callback) {
  async.waterfall([
    function(next) {
      // ...
    },
    function(returnVal, next) {
      // ...
    },
    function(returnVal, next) {
      // ...
    },
    function(returnVal, next) {
      // ...
    },
    function(returnVal, next) {
      // ...
    },
  ], function(err, returnValue) {
    // ...
  });
}
```

### Intent:

The goal of the above is to indicate linear execution and to introduce a convention to distinguish between nested callbacks.

### Known drawbacks:

None as of writing. Know of something? Open an [issue](https://github.com/liquid/javascript.conventions/issues).
