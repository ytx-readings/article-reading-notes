# The `Promise` constructor

The `Promise()` constructor creates `Promise` objects. It is primarily used to wrap callback-based APIs that do not already support promises.

## Syntax

```js
new Promise(executor)
```

* The `executor` is a function to be executed by the processor. It is a function with two parameters: `resolveFunc` and `rejectFunc`. The `resolveFunc` and `rejectFunc` are functions that are used to resolve or reject the promise.
    * Both `resolveFunc` and `rejectFunc` can accept a single parameter of any type.

        ```js
        resolveFunc(value); // called on resolved
        rejectFunc(reason); // called on rejected
        ```
    * The `executor` function's completion state has limited effects on the promise's state:
        * The `return` statements only serve as control flows, but do not impact the promise's fulfillment value. If the executor exits and does not give chance for `resolveFunc` or `rejectFunc` to be called, the promise remains pending forever.
        * If an error is thrown in the executor, the promise is rejected, unless `resolveFunc` or `rejectFunc` has already been called.

## Return value

When called via `new`, the `Promise` constructor returns a promise object.

A promise's state can only change once, from pending (initial state) to either _resolved_ or _rejected_ (in both cases it is settled). Once it is settled, its state may not be changed again.

The promise is resolved when the `resolveFunc` function is called, or rejected when the `rejectFunc` function is called. Note that if you call `resolveFunc` or `rejectFunc` and pass another `Promise` object as an argument, it can be said to be "resolved" but _still not "settled"_.

If the executor function throws an error, it will cause the promise to be rejected with that error, and its return value will be neglected. _However, if the `resolveFunc` or the `rejectFunc` is called before throwing the error, the promise will be resolved or rejected, and the error will be ignored._

## Execution flow

The typical flow of constructing a `Promise` object with an executor function is as follows:

1. When the constructor generates the new `Promise` project, it also generates the corresponding pair of functions for `resolveFunc` and `rejectFunc` tethered to the `Promise` object.
2. The `executor` function is called _synchronously_ (as soon as the `Promise` object is created) with the `resolveFunc` and `rejectFunc` functions as arguments.
3. The `executor` function executes. The eventual completion of the asynchronous task is communicated with the promise instance via the side effect caused by `resolveFunc` or `rejectFunc`. The side effect is that the promise becomes "_settled_".
    * If `resolvedFunc` is called first, the value passed will be _resolved_. The promise may stay pending (in case another [_thenable_](./Thenable.md) is passed), become fulfilled (if a non-thenable value is passed), or become rejected (in case of an invalid resolution value).
    * If `rejectFunc` is called first, the reason passed will instantly become _rejected_.
    * Once one of the settling functions (`resolveFunc` or `rejectFunc`) is called, the promise stays settled. Only the _first call_ to `resolveFunc` or `rejectFunc` affects the promise's eventual state. Subsequent calls to the settling functions neither change the fulfillment value/rejection reason, nor toggle its eventual state from _resolved_ to _rejected_ or the opposite.
4. Once the promise settles, it (_asynchronously_) invokes any further handlers through the `then()`, `catch()`, or `finally()` methods. The eventual fulfillment value or rejection reason is passed to the invocation or fulfillment and rejection handlers as an input parameter (see [Chained Promises](./Chained%20Promises.md)).

### Example

The callback-based `readFile` API, as shown in the following example, can be transformed into a promise-based one.

```js
const readFilePromise = (path) =>
  new Promise((resolve, reject) => {
    readFile(path, (error, result) => {
      if (error) {
        reject(error);
      } else {
        resolve(result);
      }
    });
  });

readFilePromise("./data.txt")
  .then((result) => console.log(result))
  .catch((error) => console.error("Failed to read data"));
```

## The `resolve` function

The `resolve` function has the following behaviors:

* If it is called with the same value as the newly created promise (the promise it's "tethered to"), the promise is rejected with a `TypeError`.
* If it is called with a non-[thenable](./Thenable.md) value, then the promise is immediately fulfilled with that value.
* If it is called with a [thenable](./Thenable.md) value (including another `Promise` instance), then the thenable's `then` method is saved and called in the future (it is always called _asynchronously_). The `then` method will be called with two callbacks, which are two new functions with the exact same behaviors as the `resolveFunc` and `rejectFunc` passed to the executor function. If calling the `then` method throws, then the current promise is rejected with the thrown error.

In the last case, it means code like

```js
new Promise((resolve, reject) => {
  try {
    thenable.then(
      (value) => resolve(value),
      (reason) => reject(reason),
    );
  } catch (e) {
    reject(e);
  }
});
```

is roughly equivalent to

```js
new Promise((resolve, reject) => {
  try {
    thenable.then(
      (value) => resolve(value),
      (reason) => reject(reason),
    );
  } catch (e) {
    reject(e);
  }
});
```

Except that in the `resolve(thenable)` case:

1. `resolve` is called synchronously, so that calling `resolve` or `reject` again has no effect, even when the handlers attached through another `Promise.then()` are not called yet.
2. The `then` method is called asynchronously, so that the promise will never be instantly resolved if a thenable is passed.

## References

* [[ECMA] ECMA Language Specification](https://tc39.es/ecma262/multipage/control-abstraction-objects.html#sec-promise-constructor)
* [[MDN] `Promise()` constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)