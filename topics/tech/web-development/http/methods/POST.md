# HTTP `POST` method

The **HTTP `POST` method** sends data to server. The type of the body (the content being sent to the server) of the request is indicated by the `Content-Type` header.

The difference between [`PUT`](./PUT.md) and `POST` is that `PUT` is idempotent: calling it once or several times successively has the same effect (that is no side effect), where successive identical `POST` may have additional effects, like passing an order several times.

A `POST` request is typically sent via an HTML form and results in a change on the server. In this case, the content type is selected by putting the adequate string in the `enctype` attribute of the `<form>` element or the `formenctype` attribute of the `<input>` or `<button>` elements:

* `application/x-www-form-urlencoded`: The keys and values are encoded in key-value tuples separated by `&`, with a `=` between the key and the value. Non-alphanumeric characters are encoded. This is why this type is suitable for sending binary data (one should use `multipart/form-data` instead).
* `multipart/form-data`: Each value is sent as a block of data ("body part"), with a user agent-defined delimiter ("boundary") separating each part. The keys are given in the `Content-Disposition` header of each part.
* `text/plain`

When the `POST` request is sent via a method other than an HTML form, wuch as a `fetch` call, the body can take any type. As described in the HTTP 1.1 specification, `POST` is designed to allow a uniform method to cover the following functions:

* Annotation of existing resources
* Posting a message to a bulletin board, newsgroup, mailing list, or similar group of articles
* Adding a new user through a signup modal
* Providing a block of data, such as the result of submitting a form, to a data-handling process
* Extending a database through an append operation

## Syntax

```http
POST /test
```

## Example

A simple form using the default `application/x-www-form-urlencoded` content type:

```http
POST /test HTTP/1.1
Host: foo.example
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

field1=value1&field2=value2
```

A form using the `multipart/form-data` content type:

```http
POST /test HTTP/1.1
Host: foo.example
Content-Type: multipart/form-data;boundary="boundary"

--boundary
Content-Disposition: form-data; name="field1"

value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"

value2
--boundary--
```

## References

* [HTTP Semantics: `#POST`](https://www.rfc-editor.org/rfc/rfc9110#POST)
* [[MDN] HTTP `POST` method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)