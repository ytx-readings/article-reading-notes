# HTTP request methods

The common HTTP request methods include the following:

* [`GET`](./GET.md)
* [`HEAD`](./HEAD.md)
* [`POST`](./POST.md)
* [`PUT`](./PUT.md)
* [`DELETE`](./DELETE.md)
* [`CONNECT`](./CONNECT.md)
* [`OPTIONS`](./OPTIONS.md)
* [`TRACE`](./TRACE.md)
* [`PATCH`](./PATCH.md)

| Method | `GET` | `HEAD` | `POST` | `PUT` | `DELETE` | `CONNECT` | `OPTIONS` | `TRACE` | `PATCH` |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Request has body | No | No | Yes | Yes | May | No | No | No | Yes |
| Successful response has body | Yes | No | Yes | May | May | No | May | Yes | May |
| Safe | Yes | Yes | No | No | No | No | Yes | Yes | No |
| Idempotent | Yes | Yes | No | Yes | Yes | No | Yes | Yes | No |
| Cacheable | Yes | Yes | Only if freshness information is included | No | No | No | No | No | Only if freshness information is included |
| Allowed in HTML forms | Yes | No | Yes | No | No | No | No | No | No |

## References

* [MDN: HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)