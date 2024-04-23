# `queueMicrotask()` global function

The `queueMicrotask()` method provided in the global scope (usually available via the `Window` or `Worker` interface), is used to schedule a microtask to be executed at the safe time prior to control returning to the runtime's event loop.

A **microtask** is a short function that is run after the current task has completed its work and when there is no other code waiting to be run before control of the execution context is returned to the runtime's event loop.

This lets your code run without interfering with any other, potentially higher priority, code that is pending, but before the runtime regains control over the execution context, potentially depending the work you need to complete. Learn more about how to use microtasks and why you may choose to do so in the [Microtask](./microtask.md) guide.

The importance of microtasks comes in its ability to perform tasks asynchronously but in a specific order. Microtasks are especially useful for libraries and frameworks that need to perform final cleanup or other just-before-rendering tasks.

## Syntax

```js
queueMicrotask(() => {/* … */});
```

The `queueMicrotask` function takes a single argument, a function to be executed as a microtask. The callback function will be executed when the JavaScript engine determines that it is safe to call your code. Enqueued microtasks are executed after all pending tasks have completed but before yielding control to the runtime's event loop.

## Examples

Taken from the [`queueMicrotask` spec](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#microtask-queuing):

```js
MyElement.prototype.loadData = function (url) {
    if (this._cache[url]) {
        queueMicrotask(() => {
            this._setData(this._cache[url]);
            this.dispatchEvent(new Event("load"));
        });
    } else {
        fetch(url)
            .then((res) => res.arrayBuffer())
            .then((data) => {
                this._cache[url] = data;
                this._setData(data);
                this.dispatchEvent(new Event("load"));
            });
    }
};
```

## References

* [[MDN] `queueMicrotask` global function](https://developer.mozilla.org/en-US/docs/Web/API/queueMicrotask)
* [[WHATWG] HTML Standard: microtask queuing](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#microtask-queuing)