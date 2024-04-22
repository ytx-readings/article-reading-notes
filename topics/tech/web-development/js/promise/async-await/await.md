# `await`

The `await` operator is used to wait for a `Promise`. It can only be used inside an `async` function, or at the top level of a [module](../../modules/).

## Syntax

```js
await expression;
```

### Return value

The fulfillment value of the promise or thenable object, or, if the expression is not thenable, the expression's own value.

### Exceptions

Throws the rejection reason if the promise or thenable object is rejected.

## Description

`await` is usually used to unwrap promises by passing a [`Promise`](../README.md) as the `expression`. Using `await` pauses the execution of its surrounding `async` function until the promise is settled (fulfilled or rejected). Then the execution resumes, the value of the `await` expression becomes that of the fulfilled promise.

If the promise is rejected, then the `await` expression throws the rejected value. The function containing the `await` expression will appear in the stack trace of the error. Otherwise, if the rejected promise is not awaited or is immediately returned, the caller function will not appear in the stack trace.

The `expression` is resolved in the same way as [`Promise.resolve()`](../methods/Promise.resolve.md): it's always converted to a native `Promise` and then awaited. If the `expression` is a:

* Native `Promise` (`expression` belongs to a `Promise` or its subclass, and `expression.constructor === Promise`): The promise is directly used and awaited natively, without calling `then()`.
* [Thenable object](../Thenable.md) (including non-native promises, polyfill, proxy, child class, etc.): A new promise is constructed with the native [`Promise()`](../Promise%20constructor.md) constructor by calling the object's `then()` method and passing in a handler that calls the resolve callback.
* Non-thenable value: An already-fulfilled promise is constructed and used.

Even when the used promise is already fulfilled, the `async` function's execution still pauses until the next tick. In the meantime, the caller of the `async` function resumes execution. See example below.

Because `await` is only v valid inside `async` functions and modules, which themselves are asynchronous and return promises, the `await` expression never blocks the main thread and only defers execution of code that actually depends on the result, i.e. anything after the `await` expression.