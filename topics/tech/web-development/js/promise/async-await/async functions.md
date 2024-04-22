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

### Return values

**`async` functions always return a promise.** If the return value inside the function body is not explicitly a promise, it will be implicitly wrapped in a promise.

For example, the following code:

```js
async function foo() {
    return 1;
}
```

is similar to:

```js
function foo() {
    return Promise.resolve(1);
}
```

> **Note**: Even though the return value of an `async` function behaves as if it is wrapped in a [`Promise.resolve`](../methods/Promise.resolve.md), they are not equivalent.
>
> An async function will return a different _reference_, whereas `Promise.resolve` returns the same reference if the given value is a promise.
>
> It can be a problem when you want to check the equality of a promise and a return value of an async function:
>
> ```js
> const p = new Promise((res, rej) => {
>     res(1);
> });
>
> async function asyncReturn() {
>     return p;
> }
>
> function basicReturn() {
>     return Promise.resolve(p);
> }
>
> console.log(p === basicReturn()); // true
> console.log(p === asyncReturn()); // false
> ```

### Execution flow

The body of an `async` function can be thought of as being split by zero or more `await` expressions. Top-level code, up to and including the first `await` expression (if there is one), is run synchronously. In this way, an `async` function without an `await` expression will run synchronously. If, however, there is an `await` expression inside the function body, the `async` function will always complete asynchronously.

For example:

```js
async function foo() {
    await 1;
}
```

is also equivalent to:

```js
function foo() {
    return Promise.resolve(1).then(() => undefined);
}
```

Code after each `await` expression can be thought of as existing in a `.then` callback. In this way a promise chain is progressively constructed with each reentrant step through the function. The return value forms the final link in the chain.

In the following example, we successively `await` two promises. Progress moves through function `foo` in three stages:

1. The first line of the body of function `foo` is executed synchronously, with the `await` expression configured with the pending promise. Progress through `foo` is then _suspended_, and control is yielded back to the function that called `foo` (in this case, the top-level block).
2. Some time later, when the first promise (`result1`) has either been fulfilled or rejected, control moves back into `foo`. The result of the first promise fulfillment (if not rejected) is returned from the `await` expression. Here `1` is assigned to `result1`. The progress continues, and the second `await` expression is evaluated. Again, progress through `foo` is _suspended_ and the control is yielded.
3. Some time later, when the second promise (`result2`) has either been fulfilled or rejected, control re-enters `foo`. The result of the second promise resolution is returned from the second `await` expression. Here `2` is assigned to `result2`. Control moves to the return expression (if any). The default return value of `undefined` is returned as the resolution value of the current promise.

```js
async function foo() {
    const result1 = await new Promise((resolve) =>
        setTimeout(() => resolve("1")),
    );
    const result2 = await new Promise((resolve) =>
        setTimeout(() => resolve("2")),
    );
}

foo();
```

We can see from this example that the promise chain is not built up in one go. Instead, the promise chain is constructed in stages as the control is successively yielded from and returned to the `async` function. As a result, we must be mindful of error handling behavior when dealing with concurrent asynchronous operations.

For example, the following code throws an _unhandled promise rejection error_, even if a `.catch` handler has been configured further along the promise chain. This is because `p2` will not be "wired into" the promise chain until control returns `p1`.

```js
async function foo() {
    const p1 = new Promise((resolve) => setTimeout(() => resolve("1"), 1000));
    const p2 = new Promise((_, reject) => setTimeout(() => reject("2"), 500));
    const results = [await p1, await p2]; // Do not do this! Use Promise.all or Promise.allSettled instead.
}
foo().catch(() => {}); // Attempt to swallow all errors…
```

`async function` declarations behave similar to `function` declarations — they are [hoisted](../../hoisting.md) to the top of their scope and can be called anywhere in their scope, and they can be redeclared only in certain contexts.

## Examples

### Async functions and execution order

```js
function resolveAfter2Seconds() {
    console.log("starting slow promise");
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("slow");
            console.log("slow promise is done");
        }, 2000);
    });
}

function resolveAfter1Second() {
    console.log("starting fast promise");
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("fast");
            console.log("fast promise is done");
        }, 1000);
    });
}

async function sequentialStart() {
  console.log("== sequentialStart starts ==");

  // 1. Start a timer, log after it's done
  const slow = resolveAfter2Seconds();
  console.log(await slow);

  // 2. Start the next timer after waiting for the previous one
  const fast = resolveAfter1Second();
  console.log(await fast);

  console.log("== sequentialStart done ==");
}

async function sequentialWait() {
    console.log("== sequentialWait starts ==");

    // 1. Start two timers without waiting for each other
    const slow = resolveAfter2Seconds();
    const fast = resolveAfter1Second();

    // 2. Wait for the slow timer to complete, and then log the result
    console.log(await slow);
    // 3. Wait for the fast timer to complete, and then log the result
    console.log(await fast);

    console.log("== sequentialWait done ==");
}

async function concurrent1() {
    console.log("== concurrent1 starts ==");

    // 1. Start two timers concurrently and wait for both to complete
    const results = await Promise.all([
        resolveAfter2Seconds(),
        resolveAfter1Second(),
    ]);
    // 2. Log the results together
    console.log(results[0]);
    console.log(results[1]);

    console.log("== concurrent1 done ==");
}

async function concurrent2() {
    console.log("== concurrent2 starts ==");

    // 1. Start two timers concurrently, log immediately after each one is done
    await Promise.all([
        (async () => console.log(await resolveAfter2Seconds()))(),
        (async () => console.log(await resolveAfter1Second()))(),
    ]);
    console.log("== concurrent2 done ==");
}

sequentialStart(); // after 2 seconds, logs "slow", then after 1 more second, "fast"

// wait above to finish
setTimeout(sequentialWait, 4000); // after 2 seconds, logs "slow" and then "fast"

// wait again
setTimeout(concurrent1, 7000); // same as sequentialWait

// wait again
setTimeout(concurrent2, 10000); // after 1 second, logs "fast", then after 1 more second, "slow"
```

#### `await` and concurrency

In `sequentialStart`, execution suspends 2 seconds for the first `await`, and then another second for the second `await`. The second timer is not created until the first has already fired. So the code finishes after 3 seconds.

In `sequentialWait`, both timers are created and then `await`ed. The timers run concurrently, which means the code finishes in 2 rather than 3 seconds, i.e. the slowest timer. However, the `await` calls still run in series, which means the second `await` will wait for the first one to finish. In this case, the result of the fastest timer is processed after the slowest.

If you wish to safely perform other jobs after two or more jobs run concurrently and are complete, you must await a call to [`Promise.all()`](../methods/Promise.all.md) or [`Promise.allSettled()`](../methods/Promise.allSettled.md) before that job.

> **Warning**: The functions `sequentialWait` and `concurrent1` are not functionally equivalent.
>
> * In `sequentialWait`, if promise `fast` rejects before promise `slow` is fulfilled, then an unhandled promise rejection error will be raised, regardless of whether the caller has configured a `catch` clause.
> * In `concurrent1`, `Promise.all` wires up the promise chain in one go, meaning that the operation will fail-fast regardless of the order of rejection of the promises, and the error will always occur within the configured promise chain, enabling it to be called in the normal way.

### Rewriting a `Promise` chain with an `async` function

An API that returns a `Promise` will result in a promise chain, and it splits the function into many parts. Consider the following code:

```js
function getProcessedData(url) {
    return downloadData(url) // returns a promise
        .catch((e) => downloadFallbackData(url)) // returns a promise
        .then((v) => processDataInWorker(v)); // returns a promise
}
```

It can be written with a single `async` function as follows:

```js
async function getProcessedData(url) {
    let v;
    try {
        v = await DownloadData(url);
    } catch (e) {
        v = wait downloadFallbackData(url);
    }
    return processDataInWorker(v);
}
```

Alternatively you can chain the promise with `catch()`:

```js
async function getProcessedData(url) {
    const v = await downloadData(url).catch((e) => downloadFallbackData(url));
    return processDataInWorker(v);
}
```

In the two rewritten versions, notice there is no `await` statement after the `return` keyword, although that would be valid too: The return value of an `async` function is implicitly wrapped in `Promise.resolve` – if it's not a promise itself (as in the examples).

## References

* [ECMAScript Language Specification: `async` function definitions](https://tc39.es/ecma262/multipage/ecmascript-language-functions-and-classes.html#sec-async-function-definitions)
* [[MDN] `async` function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)