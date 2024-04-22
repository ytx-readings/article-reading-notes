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

If the promise is rejected, then the `await` expression throws the rejected value. The function containing the `await` expression will appear in the [stack trace](#improving-stack-trace) of the error. Otherwise, if the rejected promise is not awaited or is immediately returned, the caller function will not appear in the stack trace.

The `expression` is resolved in the same way as [`Promise.resolve()`](../methods/Promise.resolve.md): it's always converted to a native `Promise` and then awaited. If the `expression` is a:

* Native `Promise` (`expression` belongs to a `Promise` or its subclass, and `expression.constructor === Promise`): The promise is directly used and awaited natively, without calling `then()`.
* [Thenable object](../Thenable.md) (including non-native promises, polyfill, proxy, child class, etc.): A new promise is constructed with the native [`Promise()`](../Promise%20constructor.md) constructor by calling the object's `then()` method and passing in a handler that calls the resolve callback.
* Non-thenable value: An already-fulfilled promise is constructed and used.

Even when the used promise is already fulfilled, the `async` function's execution still pauses until the next tick. In the meantime, the caller of the `async` function resumes execution. See example below.

Because `await` is only v valid inside `async` functions and modules, which themselves are asynchronous and return promises, the `await` expression never blocks the main thread and only defers execution of code that actually depends on the result, i.e. anything after the `await` expression.

## Examples

### Awaiting a promise to be fulfilled

If a `Promise` is passed to an `await` expression, it waits for the `Promise` to be fulfilled and returns the fulfilled value.

```js
function resolveAfter2Seconds(x) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(x);
        }, 2000);
  });
}

async function f1() {
    const x = await resolveAfter2Seconds(10);
    console.log(x); // 10
}

f1();

```

### Thenable objects

[Thenable objects](../Thenable.md) are resolved just the same as actual `Promise` objects.

```js
async function f() {
    const thenable = {
        then(resolve, _reject) {
            resolve("resolved!");
        },
    };
    console.log(await thenable); // "resolved!"
}

f();
```

They can also be rejected:

```js
async function f() {
    const thenable = {
        then(resolve, reject) {
            reject(new Error("rejected!"));
        },
    };
    await thenable; // Throws Error: rejected!
}

f();
```

### Conversion to promise

If the value is not a `Promise`, `await` converts the value to a resolved `Promise`, and waits for it. The awaited value's identity does not change as long as it does not have a `then` property that is callable.

```js
async function f3() {
    const y = await 20;
    console.log(y); // 20

    const obj = {};
    console.log((await obj) === obj); // true
}

f3();
```

### Handling rejected promises

If the `Promise` is rejected, the rejected value is thrown.

```js
async function f4() {
    try {
        await Promise.reject(30);
    } catch (e) {
        console.log(e); // 30
    }
}

f4();
```

You can handle rejected promises without a `try` block by chaining a [`catch()`](../methods/Promise.prototype.catch) handler.

```js
const response = await promisedFunction().catch((err) => {
    console.error(err);
    return "default response";
});
// response will be "default response" if the promise is rejected
```

This is built on the assumption that `promisedFunction()` _never synchronously throws an error_, but always returns a rejected promise. This is the case for most properly-designed promise-based functions, which usually look like this:

```js
function promisedFunction() {
    // immediately return a promise to  minimize the chance of an error being thrown
    return new Promise((resolve, reject) => {
        // do something asynchronous
    });
}
```

However, if `promisedFunction()` does thrown an error _synchronously_, _the error won't be caught by the `catch()` handler_, so the `try…catch` statement is necessary.

### Top level `await`

You can use the `await` keyword on its own (outside of an `async` function) in a [module](../../modules/). This means that modules with child modules that use `await` will wait for the child modules to execute before themselves run, all while not blocking other child modules from loading.

Here is an example of a simple module using the **Fetch API** and specifying `await` with the `export` statement. Any modules that include this will wait for the fetch to resolve before running any code.

```js
// fetch request
const colors = fetch("../data/colors.json").then((response) => response.json());

export default await colors;
```

### Control flow effects of `await`

When an `await` is encountered in code (either in an `async` function or in a module), the awaited expression is executed, while all code that depends on the expression's value is paused and pushed into the [microtask queue](../../event-loop/microtask.md). The main thread is then freed for the next task in the event loop. This happens even if the awaited value is an already-resolved promise or not a promise. For example, consider the following code:

```js
async function foo(name) {
    console.log(name, "start");
    console.log(name, "middle");
    console.log(name, "end");
}

foo("First");
foo("Second");

// First start
// First middle
// First end
// Second start
// Second middle
// Second end
```

In this case, the two `async` functions are synchronous in effect, because they don't contain any `await` expression. The three statements happen in the same tick. In promise terms, the function corresponds to:

```js
function foo(name) {
    return new Promise((resolve) => {
        console.log(name, "start");
        console.log(name, "middle");
        console.log(name, "end");
        resolve();
    });
}
```

However, as soon as there is one `await`, the function becomes asynchronous, and the execution of following statements is deferred to the next tick:

```js
async function foo(name) {
    console.log(name, "start");
    await console.log(name, "middle");
    console.log(name, "end");
}

foo("First");
foo("Second");

// First start
// First middle
// Second start
// Second middle
// First end
// Second end
```

This corresponds to:

```js
function foo(name) {
    return new Promise((resolve) => {
        console.log(name, "start");
        console.log(name, "middle");
        resolve();
    }).then(() => {
        console.log(name, "end");
    });
}
```

While the extra `then()` handler is not necessary, and the handler can be merged with the executor passed to the constructor, the `then()` handler's existence means the code will take one extra tick to complete. The same happens for `await`. Therefore, make sure to use `await` only when necessary (to unwrap promises into their values).

Other microtasks can execute before the `async` function resumes. This example uses [`queueMicrotask()`](../../event-loop/queueMicrotask.md) to demonstrate how the microtask queue is processed when each `await` expression is encountered.

```js
let i = 0;

queueMicrotask(function test() {
    i++;
    console.log("microtask", i);
    if (i < 3) {
        queueMicrotask(test);
    }
});

(async () => {
    console.log("async function start");
    for (let i = 1; i < 3; i++) {
        await null;
        console.log("async function resume", i);
    }
    await null;
    console.log("async function end");
})();

queueMicrotask(() => {
    console.log("queueMicrotask() after calling async function");
});

console.log("script sync part end");

// Logs:
// async function start
// script sync part end
// microtask 1
// async function resume 1
// queueMicrotask() after calling async function
// microtask 2
// async function resume 2
// microtask 3
// async function end
```

In this example, the `test()` function is always called before the async function resumes, so the microtasks they each schedule are always executed in an intertwined fashion. On the other hand, because both `await` and `aueueMicrotask` schedule microtasks, the order of execution is always based on the order of scheduling. This is why the "`queueMicrotask()` after calling async function" log happens after the `async` function resumes for the first time.

### Improving stack trace

Sometimes, the `await` is omitted when a promise is directly returned from an `async` function:

```js
async function noAwait() {
    // Some actions...

    return /* await */ lastAsyncTask();
}
```

However, consider the case where `lastAsyncTask()` asynchronously throws an error:

```js
async function lastAsyncTask() {
    await null;
    throw new Error("failed");
}

async function noAwait() {
    return lastAsyncTask();
}

noAwait();

// Error: failed
//    at lastAsyncTask
```

Only `lastAsyncTask` appears in the stack trace, because the promise is rejected after it has already been returned from `noAwait` – in some sense, the promise is unrelated to `noAwait`. To improve the stack trace, you can use `await` to unwrap the promise, so that the exception gets thrown into the current function. The exception will then be immediately wrapped into a new rejected promise, but during error creation, the caller will appear in the stack trace:

```js
async function lastAsyncTask() {
    await null;
    throw new Error("failed");
}

async function withAwait() {
    return await lastAsyncTask();
}

withAwait();

// Error: failed
//    at lastAsyncTask
//    at async withAwait
```

However, there's a little performance penalty coming with `return await` because the `Promise` has to be unwrapped and wrapped again.

## References

* [[ECMASCript] `async` function definitions](https://tc39.es/ecma262/multipage/ecmascript-language-functions-and-classes.html#sec-async-function-definitions)
* [[MDN] `await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)