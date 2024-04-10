# Preflight Request

A **preflight request** is a [CORS](./CORS.md) request that checks if the CORS protocol is understood and a server is aware using specific methods and headers.

It is an [`OPTIONS`](../http/methods/OPTIONS.md) request, using two or three HTTP headers:

* `Access-Control-Request-Method`
* `Origin`
* `Access-Control-Request-Headers` (optional)

A preflight request is automatically issued by the browser. In normal cases, front-end developers do not need to create such requests on their own. It is sent when a request is qualified as "to be preflighted" and omitted for [simple requests](./Simple%20Request.md). The browser sends a preflight request to the server to check if the actual request is safe to send, before sending the actual request.

In CORS scenarios, cross-origin requests are preflighted since they may have implications for user data.

## Examples

### Sending a `DELETE` Request

A client may ask the server if it could safely send a `DELETE` request, by sending a preflight request first:

```http
OPTIONS /resource/foo
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Origin, X-Requested-With
Origin: https://foo.bar.org
```

If the server allows it, then it responds to the preflight request with a `Access-Control-Allow-Methods` containing the `DELETE` method:

```http
HTTP/1.1 204 No Content
Connection: keep-alive
Access-Control-Allow-Origin: https://foo.bar.org
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Allow-Headers: Origin, X-Requested-With
Access-Control-Max-Age: 86400
```

The preflight request can be _optionally cached_ for requests created in the same URL using the `Access-Control-Max-Age` header. To cache preflight requests, the browser uses a specific cache separate from the normal HTTP cache. Preflight requests are never cached in the browser's general HTTP cache.

## References

* [MDN](https://developer.mozilla.org/)
    * [Glossary: Preflight Request](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request)
    * [Reference: HTTP Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests)