# `Promise`

A **`Promise`** is an object representing the eventual completion or failure of an asynchronous operation. It allows you to associate handlers with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise to supply the value at some point in the future.

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