# Controlling Media Types with ProducesAttribute

This repo is a contrived example for controlling media types in combination with `ProducesAttribute`.

My controller action produces a response with media type `application/json`. However, if something goes wrong I want the controller to return `Problem details` with media type `application/problem+json`.

If I do not specify any `ProducesAttribute` on the controller action method the response is sent as `application/problem+json`:

<pre>
$ curl -i http://localhost:5000/problem1
HTTP/1.1 500 Internal Server Error
Date: Mon, 02 Mar 2020 20:19:27 GMT
Content-Type: <strong>application/problem+json</strong>; charset=utf-8
Server: Kestrel
Transfer-Encoding: chunked

{"type":"https://tools.ietf.org/html/rfc7231#section-6.6.1","title":"An error occured while processing your request.","status":500,"detail":"Bad things happened.","traceId":"|2bb86458-402fe8f064326370."}
</pre>

However, decorating the controller action method with `ProducesAttribute` causes the response content type to be sent as `application/json`:

<pre>
$ curl -i http://localhost:5000/problem2
HTTP/1.1 500 Internal Server Error
Date: Mon, 02 Mar 2020 20:19:30 GMT
Content-Type: <strong>application/json</strong>; charset=utf-8
Server: Kestrel
Transfer-Encoding: chunked

{"type":"https://tools.ietf.org/html/rfc7231#section-6.6.1","title":"An error occured while processing your request.","status":500,"detail":"Worse things happened.","traceId":"|2bb86459-402fe8f064326370."}
</pre>