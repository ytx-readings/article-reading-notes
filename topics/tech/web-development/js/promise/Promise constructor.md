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

## Return value

When called via `new`, the `Promise` constructor returns a promise object.

A promise's state can only change once, from pending (initial state) to either _resolved_ or _rejected_ (in both cases it is settled). Once it is settled, its state may not be changed again.

The promise is resolved when the `resolveFunc` function is called, or rejected when the `rejectFunc` function is called. Note that if you call `resolveFunc` or `rejectFunc` and pass another `Promise` object as an argument, it can be said to be "resolved" but _still not "settled"_.

If the executor function throws an error, it will cause the promise to be rejected, and its return value will be neglected.

## References

* [[ECMA] ECMA Language Specification](https://tc39.es/ecma262/multipage/control-abstraction-objects.html#sec-promise-constructor)
* [[MDN] `Promise()` constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)