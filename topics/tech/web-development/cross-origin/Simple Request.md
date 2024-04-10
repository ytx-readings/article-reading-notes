# Simple Request

A request is called a **simple request** if it does not trigger a [CORS](./CORS.md) [preflight](./Preflight%20Request.md).

A request is simple if it meets the following requirements:

* The request method is one of these allowed methods:
    * [`GET`](../http/methods/GET.md)
    * [`HEAD`](../http/methods/HEAD.md)
    * [`POST`](../http/methods/POST.md)
* Apart from headers set by the user agent, the only headers that allowed to be set are the headers defined in the [Fetch spec](https://fetch.spec.whatwg.org/) as **CORS-safelisted request headers**, which are:
    * `Accept`
    * `Accept-Language`
    * `Content-Language`
    * `Content-Type` (but only with a value of `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`)
    * `Range` (only with a [simple range header value](https://fetch.spec.whatwg.org/#simple-range-header-value), e.g. `bytes=256`, or `bytes=127-255`)
* If the request is made using an `XMLHttpRequest` object, no event listeners are registered on the object returned by the `XMLHttpRequest.upload` property used in the request; that is, given an `XMLHttpRequest` object `xhr`, no code has called `xhr.upload.addEventListener()` to add an event listener to monitor the upload.
* No `ReadableStream` object is used in the request.

## References

* [MDN](https://developer.mozilla.org/)
    * [Reference: HTTP Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests)