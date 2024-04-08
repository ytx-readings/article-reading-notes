# CORS

The **CORS** (Cross-Origin Resource Sharing) is a system, consisting of transmitting [HTTP headers](../http/HTTP%20headers.md), that determines wether browsers should block or permit frontend JavaScript code to access responses from a different origin.

## CORS headers

* `Access-Control-Allow-Origin`: Indicates whether the response can be shared.
* `Access-Control-Allow-Credentials`: Indicates whether the response can be exposed when the credentials flag is true.
* `Access-Control-Allow-Methods`: Specifies the method or methods allowed when accessing the resource in response to a [preflight request](./Preflight%20Request.md).
* `Access-Control-Expose-Headers`: Indicates which headers can be exposed as part of the response by listing their names.
* `Access-Control-Max-Age`: Indicates how long the results of a preflight request can be cached.
* `Access-Control-Request-Headers`: Used when issuing a preflight request to let the server know what HTTP headers will be used when the actual request is made.
* `Access-Control-Request-Method`: Used when issuing a preflight request to let the server know what [HTTP method](../http/methods/HTTP%20request%20methods.md) will be used when the actual request is made.
* `Origin`: Indicates where a fetch originates from.
* `Timing-Allow-Origin`: Specifies origins that are allowed to see values of attributes retrieved via features of the Resource Timing API, which would otherwise be reported as zero due to cross-origin restrictions.