# Using Promises

A `Promise` is an object representing the eventual completion or failure of an asynchronous operation. It allows you to associate handlers (callbacks) with an asynchronous action's eventual success value or failure reason. This lets asynchronous methods return values like synchronous methods: instead of the final value, the asynchronous method returns a promise to supply the value at some point in the future.

This tutorial will cover the basics of using promises in JavaScript. It is divided into the following parts:

1. [From callbacks to promises: basic usage](#from-callbacks-to-promises-basic-usage)
2. [Chaining promises](#chaining-promises)
3. [Error handling](#error-handling)
4. [Composition](#composition)
5. [Creating promises from old callback APIs](#creating-promises-from-old-callback-apis)
6. [Timing](#timing)

## From callbacks to promises: basic usage

Before the introduction of promises, asynchronous operations were handled using callbacks. For each asynchronous operation, you usually need to provide a _success callback_ to do the operation after the asynchronous operation is completed, and an _error callback_ to handle the error if the operation fails.

Here is some code that demonstrates callback-style asynchronous operations:

```js
function successCallback(value) {
  console.log('The operation succeeded:', value);
}

function failureCallback(error) {
  console.error('The operation failed:', error);
}

doSomethingAsync(args, successCallback, failureCallback);
```

If the asynchronous operation above is written to return a promise, you would attach the callbacks to the returned promise instead:

```js
doSomethingAsync(args).then(successCallback, failureCallback);
```

This convention has several advantages over the callback style. The following sections will explore each of them.

## Chaining promises

## Error handling

## Composition

## Creating promises from old callback APIs

## Timing

## See also

* [`async` functions](./async-await/async%20functions.md)
* [`await`](./async-await/await.md)