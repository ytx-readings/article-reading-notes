# `Promise[@@species]`

The `Promise[@@species]` accessor property returns the constructor used to construct return values from promise methods.

> **Warning**: The existence of @@species allows execution of arbitrary code and may create security vulnerabilities. It also makes certain optimizations much harder. Engine implementers are [investigating whether to remove this feature](https://github.com/tc39/proposal-rm-builtin-subclassing). Avoid relying on it if possible.

## Syntax

```js
Promise[Symbol.species]
```

### Return value

The value of the constructor (`this`) on which `get @@species` was called. The return value is used to construct return values from promise chaining methods that create new promises.

## Description

The `@@species` accessor property returns the default constructor for `Promise` objects. Subclass constructors may override it to change the constructor assignment. The default implementation is basically:

```js
// Hypothetical underlying implementation for illustration
class Promise {
    static get [Symbol.species]() {
        return this;
    }
}
```

Because of this polymorphic implementation, `@@species` of derived subclasses would also return the constructor itself by default.

```js
class SubPromise extends Promise {}
SubPromise[Symbol.species] === SubPromise; // true
```

## References

* [[MDN] `Promise[@@species]`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/@@species)
* [ECMAScript Language Specification](https://tc39.es/ecma262/multipage/control-abstraction-objects.html#sec-get-promise-@@species)