# Microtask

A **microtask** is a short function executed after the function or program which created it exits _and_ only if the [JavaScript execution stack](./event%20loop.md) is empty, but before returning control to the event loop being used by the user agent to drive the script's execution environment.

The event loop continuously iterates, processing events and callbacks. Microtasks are small units of work that are executed by the event loop immediately after the iteration completes. When many microtasks are scheduled, they form up a queue and may cause the event loop to block, which may result in degradation of application performance. The **microtask queue** is processed in order, executed one-by-one until the queue is empty.

## Tasks vs. Microtasks

**Tasks** and **Microtasks** are two important concepts in JavaScript. There are several aspects of differences between them:

### Tasks

A **task** is any JavaScript code scheduled by any standard mechanisms, such as initially starting to run a program, an event callback being run, or an interval or timeout being fired. They are all scheduled on the **task queue**.

Tasks are added to the task queue when:
    * A new JavaScript program or subprogram is executed (e.g. from console, or running code in a `<script>` environment).
    * An event fires, adding its callback to the task queue.
    * A timeout or interval created with `setTimeout` or `setInterval` is reached, causing the corresponding callback to be added to the task queue.

The event loop handles the tasks one after another, in the order in which they are enqueued.

Each iteration of the event loop will run the oldest runnable tasks. After that, _the microtasks are executed one after another until the microtask queue is empty_. Then the runtime moves on to the next iteration of the event loop.

### Microtasks

At first the difference between them look subtle, and they serve similar purposes: they are both JavaScript code that are placed into a queue to be run at appropriate times. But the event loop processes them differently. There are _two key differences_:

1. Each time a task exits, the event loop checks to see if the task is returning control to other JavaScript code. If not, it runs all of the microtasks in the microtask queue. The microtask queue is then processed multiple times per iteration of the event loop, including after handling events and other callbacks.
2. If a microtask adds more microtasks to the queue by calling [`queueMicrotask()`](./queueMicrotask.md), those newly added microtask execute _before the next task is run_, because the event loop keeps calling the microtasks until the microtask queue is empty, even if more microtasks are keeping added.

> **Warning**: Since microtasks can by themselves can add more microtasks, and the event loop continues to process the microtask queue until it is empty, it is possible to create an endless queue of microtask queue that the event loop will process indefinitely. So you should be cautious about recursively adding microtasks.

## References

* [MDN](https://developer.mozilla.org/)
    * [Microtask guide](https://developer.mozilla.org/)
    * [In depth: Microtasks and the JavaScript runtime environment](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth)