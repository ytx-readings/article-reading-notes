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