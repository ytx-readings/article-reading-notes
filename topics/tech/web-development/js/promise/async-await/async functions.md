# `async` functions

In JavaScript, `async` functions provide the functionality of asynchronous, promise-based function calling, allowing asynchronous operations to be written in a cleaner style similar to synchronous code, avoiding the needs of explicit promise chaining.

## Syntax

```js
async function name(param0) {
    statements
}

async function name(param0, param1) {
    statements
}

async function name(param0, param1, /* …, */ paramN) {
    statements
}
```

> **Note**: There cannot be a line terminator between `async` and `function`, otherwise a semicolon is automatically inserted, causing `async` to become an identifier and the rest to become a `function` declaration.

## Description

An `async` function is a function declared with the `async` keyword, creating an `AsyncFunction` object. Each time it is called, it creates a new `Promise` that will be resolved with the function's return value, or rejected with an exception uncaught within the async function.

An `async` function may contain zero or more [`await`](./await.md) expressions. `await` expressions make the promise-returning functions behave as if they are synchronous code, by suspending the execution until the promise is fulfilled or rejected. The resolved value of the promise is treated as the return value of the `await` expression. The keywords `async` and `await` enables the use of ordinary `try`/`catch` blocks around asynchronous code, allowing for easier error handling.

> **Note**: The `await` keyword can only be used inside async functions within regular JavaScript code. If you use it outside of an async function's body, you will get a `SyntaxError`.
>
> `await` can be used on its own with [JavaScript modules](../../modules/README.md).

> **Note**: The purpose of `async`/`await` is to simplify the syntax necessary to consume promise-based APIs. The behavior of `async`/`await` is similar to combining [generators](../../iterators%20&%20generators.md) and [promises](../../promise/README.md).