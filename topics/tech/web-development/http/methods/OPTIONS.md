# HTTP `OPTIONS` method

The HTTP **`OPTIONS` method** requests permitted communication options for a given URL or server. A client can specify a URL with this method, or an asterisk (`*`) to refer to the entire server.

## Syntax

```http
OPTIONS /index.html HTTP/1.1
OPTIONS * HTTP/1.1
```

## Examples

### Identifying allowed request methods

To find out which request methods a server supports, one can use the `curl` command-line program to issue an `OPTIONS` request:

```bash
curl -X OPTIONS https://example.org -i
```

The server will respond with a list of allowed methods in the `Allow` header:

```http
HTTP/1.1 204 No Content
Allow: OPTIONS, GET, HEAD, POST
Cache-Control: max-age=604800
Date: Thu, 13 Oct 2016 11:45:00 GMT
Server: EOS (lax004/2813)
```

### Preflight request in CORS

In [CORS](../../CORS/CORS.md), a [preflight request](../../CORS/Preflight%20Request.md) is sent with the `OPTIONS` method so that the server can respond if it is acceptable to send the request.

The following example requests permission for these parameters:

* The `Access-Control-Request-Method` header tells the server that when the actual request is sent, it will have a [`POST`](./POST.md) request method.
* The `Access-Control-Request-Headers` header tells the server that when the actual request is sent, it will have the `X-PINGOTHER` and `Content-Type` headers.

```http
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.example
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Origin: https://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

The server now can respond if it will accept a request under these circumstances. In this example, the server response says that:

* `Access-Control-Allow-Origin`: The https://foo.example origin is permitted to request the bar.example/resources/post-here/ URL via the following:
* `Access-Control-Allow-Methods`: The [`POST`](./POST.md), [`GET`](./GET.md), and `OPTIONS` methods are allowed.
* `Access-Control-Allow-Headers`: The `X-PINGOTHER` and `Content-Type` are permitted request headers.
* `Access-Control-Max-Age`: The above permissions may be cached for 86400 seconds (1 day).

```http
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: https://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
```

## Status codes

Permitted status codes are `200 OK` and `204 No Content`. But some browsers incorrectly believe that `204 No Content` applies to the resource and do not send the subsequent request to fetch it.

## References

* [HTTP Semantics: `#OPTIONS`](https://www.rfc-editor.org/rfc/rfc9110#OPTIONS)
* [[MDN] HTTP `OPTIONS` method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)