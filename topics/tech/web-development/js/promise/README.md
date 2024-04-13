# `Promise`

A **`Promise`** is an object representing the eventual completion or failure of an asynchronous operation. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise to supply the value at some point in the future.

A promise is in one of these states:

* _pending_: initial state, neither fulfilled nor rejected.
* _fulfilled_: meaning that the operation completed successfully.
* _rejected_: meaning that the operation failed.

The _eventual value_ of a promise can either be fulfilled with a value, or rejected with a reason (error). A promise that is either fulfilled or rejected is said to be _settled_.

When either change of state takes place, the associated handlers queued up by a promise's `then()` method are called. If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there is no race condition between an asynchronous operation completing and its handlers being attached.

## The built-in `Promise` class

### Constructor

* [The `Promise()` constructor](./Promise%20constructor.md)

### Properties

* [`Promise[@@species]`](./Promise[@@species].md)

### [Methods](./methods/)

* [`Promise.all()`](./methods/Promise.all.md)
* [`Promise.allSettled()`](./methods/Promise.allSettled.md)
* [`Promise.any()`](./methods/Promise.any.md)
* [`Promise.prototype.catch()`](./methods/Promise.prototype.catch.md)
* [`Promise.prototype.finally()`](./methods/Promise.prototype.finally.md)
* [`Promise.race()`](./methods/Promise.race.md)
* [`Promise.reject()`](./methods/Promise.reject.md)
* [`Promise.resolve()`](./methods/Promise.resolve.md)
* [`Promise.prototype.then()`](./methods/Promise.prototype.then.md)
* [`Promise.withResolvers()`](./methods/Promise.withResolvers.md)

## Tutorials

* [Using Promises](./Using%20Promises.md)

## References

* [[ECMAScript Language Specification] `Promise`](https://tc39.es/ecma262/multipage/control-abstraction-objects.html#sec-promise-objects)
* [[MDN] `Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)