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

A common use case is to execute two or more asynchronous operations one by one, using the previous operation's result as an input to the next operation. In the old days, doing several asynchronous operations will result in _callback hell_, where you have to nest callbacks inside callbacks, making the code hard to read and maintain.

Example of callback hell:

```js
doSomething(function (result) {
    doSomethingElse(result, function (newResult) {
        doThirdThing(newResult, function (finalResult) {
            console.log(`Got the final result: ${finalResult}`);
        }, failureCallback);
    }, failureCallback);
}, failureCallback);
```

With promise, we can accomplish the same thing by creating a _promise chain_. This is great because the callbacks are attached to the returned promise, instead of being passed as arguments to the asynchronous function. The code can be written as follows:

```js
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);
```

Note that the `Promise` object returned by the `then()` method is a **new promise**, different from the original. Hence the `promise2` promise object represents completion of not only `promise`, but also of the `successCallback` or `failureCallback` functions as well.

The `successCallback` or `failureCallback` functions can also be asynchronous functions returning a promise. if that is the case, then any callbacks added to `promise2` will get queued behind the promise returned by either `successCallback` or `failureCallback`.

With this pattern you can create longer chains of asynchronous operations, where each promise represents the completion of one asynchronous step in the chain. In addition, the arguments to `then` are optional, and `catch(failureCallback)` is the short-hand for `then(null, failureCallback)`. If your error handling is the same for all steps, you can attach it to the end of the chain:

```js
doSomething()
    .then(function (result) {
        return doSomethingElse(result);
    })
    .then(function (newResult) {
        return doThirdThing(newResult);
    })
    .then(function (finalResult) {
        console.log(`Got the final result: ${finalResult}`);
    })
    .catch(failureCallback);
```

The example above may be written in _arrow functions_ as well:

```js
doSomething()
    .then((result) => doSomethingElse(result))
    .then((newResult) => doThirdThing(newResult))
    .then((finalResult) => {
        console.log(`Got the final result: ${finalResult}`);
    })
    .catch(failureCallback);
```

Here, `doSomethingElse` and `doThirdThing` can return any value – if they return promises, the promise is first waited until it settles, and the next callback receives the fulfillment value, not the promise itself.

### Floating promises

It is important to always return promises from `then` callbacks, even if the promise always resolves to `undefined`. If the previous handler started a promise but did not return it, there is no way to track its state anymore, and the promise is said to be "_floating_".

```js
// WRONG USAGE (floating promise)
doSomething()
    .then((url) => {
        // Missing `return` keyword in front of fetch(url).
        fetch(url);
    })
    .then((result) => {
        // result is undefined, because nothing is returned from the previous
        // handler. There's no way to know the return value of the fetch()
        // call anymore, or whether it succeeded at all.
    });
```

By returning the result of the fetch call (which is a promise), we can both track its completion and receive its value when it completes.

```js
doSomething()
    .then((url) => {
        // `return` keyword added
        return fetch(url);
    })
    .then((result) => {
        // result is a Response object
    });
```

Floating promises could be worse if you have race conditions. If the promise from the last handler is not returned, the next `then` handler will be called immediately, before the desired results are available. So the values you intend to obtain may be incomplete. For example:

```js
// WRONG USAGE (intermediate promise not returned)

const listOfIngredients = [];

doSomething()
    .then((url) => {
        // Missing `return` keyword in front of fetch(url).
        fetch(url)
            .then((res) => res.json())
            .then((data) => {
                listOfIngredients.push(data);
            });
    })
    .then(() => {
        console.log(listOfIngredients);
        // listOfIngredients will always be [], because the fetch request hasn't completed yet.
    });
```

Hence, there is a rule of thumb: whenever your operation encounters a promise, return the promise and defer its handling to the next handler.

```js
const listOfIngredients = [];

doSomething()
    .then((url) => {
        // `return` keyword now included in front of fetch call.
        return fetch(url)
            .then((res) => res.json())
            .then((data) => {
                listOfIngredients.push(data);
            });
    })
    .then(() => {
        console.log(listOfIngredients);
        // listOfIngredients will now contain data from fetch call.
    });
```

Even better, you can flatten the nested chain into a single chain, which makes the code simpler and more readable, and makes error handling easier:

```js
doSomething()
    .then((url) => fetch(url))
    .then((res) => res.json())
    .then((data) => {
        listOfIngredients.push(data);
    })
    .then(() => {
        console.log(listOfIngredients);
    });
```

(More details are explained in the [Nesting](#nesting) section)

### Using [`async`](./async-await/async%20functions.md)/[`await`](./async-await/await.md)

Using `async`/`await` makes your code more intuitive and looks like synchronous code:

```js
async function logIngredients() {
    const url = await doSomething();
    const res = await fetch(url);
    const data = await res.json();
    listOfIngredients.push(data);
    console.log(listOfIngredients);
}
```

This code looks exactly like synchronous code, except for the `await` keywords showing up before promises. One of the only tradeoffs is that it may be easy to forget the await keyword, which can only be fixed when there's a type mismatch (e.g. trying to use a promise as a value).

The `async`/`await` syntax is built on top of promises. Since it very much resembles synchronous code, there is minimal refactoring needed to change the code from promises to `async`/`await`. Click on the links to read more about [`async` functions](./async-await/async%20functions.md) and [`await`](./async-await/await.md).

## Error handling

In the original callback example as shown above, the `failureCallback` callback is used three times. When the logic becomes more complicated, code like this will be even harder to write and read. With promises, you only need use `failureCallback` only once at the end of the promise chain:

```js
doSomething()
    .then((result) => doSomethingElse(result))
    .then((newResult) => doThirdThing(newResult))
    .then((finalResult) => console.log(`Got the final result: ${finalResult}`))
    .catch(failureCallback);
```

If there is an exception, the browser will look down the chain for `.catch()` handlers or `onRejected`. This is similar to how the `try`/`catch` block runs for synchronous code.

```js
try {
    const result = syncDoSomething();
    const newResult = syncDoSomethingElse(result);
    const finalResult = syncDoThirdThing(newResult);
    console.log(`Got the final result: ${finalResult}`);
} catch (error) {
    failureCallback(error);
}
```

The asynchronous code will look very similar to the example above if you write it with `async`/`await`:

```js
async function foo() {
    try {
        const result = await doSomething();
        const newResult = await doSomethingElse(result);
        const finalResult = await doThirdThing(newResult);
        console.log(`Got the final result: ${finalResult}`);
    } catch (error) {
        failureCallback(error);
    }
}
```

Promises solve a fundamental flaw with the callback pyramid of doom, by catching all errors, even thrown exceptions and programming errors. This is essential for functional composition of asynchronous operations. All errors are now handled by the [`catch()`](./methods/Promise.prototype.catch.md) method at the end of the chain, and you should almost never need to use `try`/`catch` without using `async`/`await`.

### Nesting

In the example above involving `listOfIngredients`, the first one has a promise chain nested inside the return value of another `then()` handler, while the second example uses an entirely flat chain. Simple promise chains are best kept flat without nesting, as nesting can be a result of careless composition.

_Nesting is a control structure to limit the scopes of `catch` statements._ Specifically, a nested catch only catches failures in its scope and below, not errors higher up in the chain outside the nested scope. When used correctly, this gives greater precision in error recovery:

```js
doSomethingCritical()
    .then((result) =>
        doSomethingOptional(result)
            .then((optionalResult) => doSomethingExtraNice(optionalResult))
            .catch((e) => {}), // inner catch
    ) // Ignore if optional stuff fails; proceed.
    .then(() => moreCriticalStuff())
    .catch((e) => console.error(`Critical failure: ${e.message}`)); // outer catch
```

The inner error-silencing catch only catches failures from `doSomethingOptional()` and `doSomethingExtraNice()`, after which the code continues to run `moreCriticalStuff()`. Importantly, if `doSomethingCritical()` fails, then its error is only caught by the outer and final `catch`, without being swallowed by the inner `catch`.

The code may be written in `async`/`await` as well:

```js
async function main() {
    try {
        const result = await doSomethingCritical();
        try {
            const optionalResult = await doSomethingOptional(result);
            await doSomethingExtraNice(optionalResult);
        } catch (e) {
            // Ignore failures in optional steps and proceed.
        }
        await moreCriticalStuff();
    } catch (e) {
        console.error(`Critical failure: ${e.message}`);
    }
}
```

### Chaining after a `catch`

It is possible to chain _after_ a failure, i.e. a `catch`, which is useful for doing new actions even after an action failed in the chain. Here is an example:

```js
doSomething()
    .then(() => {
        throw new Error("Something failed");

        console.log("Do this");
    })
    .catch(() => {
        console.error("Do that");
    })
    .then(() => {
        console.log("Do this, no matter what happened before");
    });
```

The output is as follows:

```
Initial
Do that
Do this, no matter what happened before
```

In `async`/`await`, the same code would look like this:

```js
async function main() {
    try {
        await doSomething();
        throw new Error("Something failed");

        console.log("Do this");
    } catch (e) {
        console.error("Do that");
    }
    console.log("Do this, no matter what happened before");
}

```

## Composition

## Creating promises from old callback APIs

## Timing

## See also

* [`async` functions](./async-await/async%20functions.md)
* [`await`](./async-await/await.md)

## References

* [[MDN] Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)