# Thenable

A thenable is an object that implements the `then()` method, which is called with two callbacks: one for when the promise is fulfilled, and one for when it is rejected. Promises are by themselves thenables.

To interoperate with existing Promise operations, the JavaScript language allows using thenables in place of promises. For example, `Promise.resolve` will not only resolve promises, but also trace thenables.

```js
const aThenable = {
  then(onFulfilled, onRejected) {
    onFulfilled({
      // The thenable is fulfilled with another thenable
      then(onFulfilled, onRejected) {
        onFulfilled(42);
      },
    });
  },
};

Promise.resolve(aThenable); // A promise fulfilled with 42
```

## References

* [[MDN] `Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise#thenables)