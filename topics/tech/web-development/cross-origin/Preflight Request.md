# Preflight Request

A **preflight request** is a [CORS](./CORS.md) request that checks if the CORS protocol is understood and a server is aware using specific methods and headers.

It is an [`OPTIONS`](../http/methods/OPTIONS.md) request, using two or three HTTP headers:

* `Access-Control-Request-Method`
* `Origin`
* `Access-Control-Request-Headers` (optional)

A preflight request is automatically issued by the browser. In normal cases, front-end developers do not need to create such requests on their own. It is sent when a request is qualified as "to be preflighted" and omitted for [simple requests](./Simple%20Request.md). The browser sends a preflight request to the server to check if the actual request is safe to send, before sending the actual request.

In CORS scenarios, cross-origin requests are preflighted since they may have implications for user data.

## References

* [MDN](https://developer.mozilla.org/)
    * [Glossary: Preflight Request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)
    * [Reference: HTTP Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests)