# HTTP
- HTTP communication consists of two parts: the HTTP Request sent by the client and the HTTP Repsonse sent by the server.

## Table of Contents
- [Evolution of HTTP](#evolution-of-http)
- [HTTP Request](#http-request)
- [HTTP Response](#http-response)
- [HTTP Timings](#http-timings)
- [Resource Formats](#resource-formats)
  - [MIME Type](#mime-type)

## Evolution of HTTP
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/09/HTTP-1.1_vs._HTTP-2_vs._HTTP-3_Protocol_Stack.svg" alt="HTTP Versions" width="40%"/>
</p>

- When the World Wide Web wa introduced, there was the need for a standard protocol in transferring information between clients and servers.
- **HTTP/0.9**
  - In 1990, the creator of the WWW, Tim Berners-Lee, developed the first version of HTTP.
  - This version only supported very basic functionality in sending/receiving information.
  - Example
    - Request:
      ```
      GET /somepage.html
      ```
    - Response:
      ```
      <html>I am an HTML page</html>
      ```
- **HTTPS**
  - In 1994, HTTPS was introduced.
  - Instead of HTTP over simply TCP/IP, SSL was added to provide an encryption.
    - TLS is an upgraded version of SSL.
- **HTTP/1.0**
  - In 1996, HTTP/1.0 was released.
  - It includes new features: _HTTP headers_, transmit multiple requests over a single connection, better error handling, improved caching, etc.
    - The `Content-Type` header allowed servers to respond with other documents other than HTML files.
  - Example
    - Request:
      ```
      Get /somepage.html HTTP/1.0
      User-Agent: // ...
      ```
    - Response:
      ```
      200 OK
      Date: // ...
      Server: // ...
      Content-Type: text/html
      <HTML>
        I am an HTML page
        <IMG SRC="/someimage.gif">
      </HTML>
      ```
    - Request for IMG:
      ```
      GET /someimage.jpg HTTP/1.0
      User-Agent: //...
      ```
    - Response for IMG:
      ```
      200 OK
      Date: // ...
      Server: // ...
      Content-Type: text/gif
      (image content)
      ```
- **HTTP/1.1**
  - In 1997, HTTP/1.1 was released.
  - It includes new features: reusable/persistent connections (`Keep-Alive`), pipelining (allows second request to be sent before receiving the response to the first one), chunked responses, more cache mechanisms, client and server agreeing on which content to exchange, etc.
    - The `Host` header allowed server collocation (different domains hosted on the same IP address).
  - Example
    - Request:
      ```
      GET /path/to/somepage HTTP/1.1
      Host: google.com
      User-Agent: // ...
      Accept: // MIME types that are allowed
      Accept-Language: // ...
      Accept-Encoding: // ...
      Referer: // ...
      ```
    - Response:
      ```
      200 OK
      Connection: Keep-Alive
      Content-Encoding: // ...
      Content-Type: text/html; charset=utf-8
      // ...
      Transfer-Encoding: chunked
      // ...
      (response content)
      ```
      - _Note: if there are other resources in the page, the browser sends subsequent requests for them._
- [**REST**](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/API.md#representational-state-transferrest)
  - In 2000, REST was introduced.
  - It is a new pattern for using HTTP that became common in the 2010s.
- **HTTP/2**
  - In 2015, HTTP/2 was standardized.
  - As web pages became more complex, heavy, visually intensive, interactive, more data needed to be transmitted over more HTTP requests. The need for improved performance was felt.
  - It improved performance and efficiency in transferring data over the Internet.
  - It include new features: binary protocol rather than text protocol, multiplexing (parallel requests over the same connection), headers compression, server push (server populates data in a client cache) etc.
- **HTTP/3**
  - In 2022, HTTP/3 was released.
  - It includes new features: using QUIC instead of TCP, multiplexing with multiple streams over UDP (one error only blocks the stream with the error).

## HTTP Request
- An HTTP Request consists of three elements: a request line, headers, and a body (if needed).
### Request Line
- The Request Line consists of three key elements: an HTTP method, request target, HTTP version.
- Ex: `GET /dog.png HTTP/1.0`.
#### HTTP Method
- `GET`, `POST`, `PATCH`, `DELETE`, etc.
#### Request Target
- Usually in a form of a URL, or an absolute path.
#### HTTP Version
- The HTTP Version represents the HTTP specification that the client tried to comply, which will also be used for the response accordingly.
### Request Headers
> **HTTP headers** let the client and the server pass additional information with an HTTP request or response. An HTTP header consists of its case-insensitive name followed by a colon (:), then by its value. | MDN
- HTTP headers are meant to provide additional information about the HTTP message to the recipient.
#### Headers Grouped by Context
- **General Headers**
  - Apply to the message as a whole, not to the content itself.
  - Ex: `Connection: keep-alive`.
- **Request Headers**
  - Contain more information about the resource to be fetched, or about the client requesting the resource.
  - Used to provide information about the request context, so that the server can tailor the response.
  - Ex: `Host: localhost:8000`, `User-Agent: Mozilla 5.0`, `Accept: text/html, image/jpeg`.
- **Representation Headers**
  - Contain information about the body of the resource.
  - Describes the particular representation/format of the resource sent in an HTTP message body, and any encoding applied to it.
  - Previously called Entity Header.
  - Ex: `Content-Type: application/json`, `Content-Type: multipart/form-data`, `Content-Length: 35`.
### Request Body
- The payload of the request.
- Requests such as `POST` that need to send data to the server require a body.
#### 2 Categories of Response Body
- **Single-Resource Body**
  - Consist of a single file, defined by two headers: `Content-Type` and `Content-Length`.
- **Multiple-Resource Body**
  - Consist of a multipart body, in which each body contains a different bit of information.
  - Typical with HTML forms.
  - `Content-Type: multipart/form-data`.

## HTTP Response
- An HTTP Request consists of three elements: a status line, headers, and a body (usually needed).
### Status Line
- The Status Line consists of three key elements: HTTP version, status code, status text.
- Ex: `HTTP/1.1 200 OK`.
### Response Headers
#### Headers Grouped by Context
- **General Headers**
- **Response Headers**
  - Provide information about the response, not the content of the message.
  - Provide additional information about the server which doesn’t fit in the status line.
  - Ex: `Access-Control-Allow-Origin: *`, `Server: Apache`.
- **Representation Headers**
### Response Body
- The payload of the response.
- Some responses do not have a corresponding payload to a request.
#### 3 Categories of Response Body
- **Single Resources Body**
  - That consists of a single file, defined by two headers: `Content-Type` and `Content-Length`.
  - That consists of a single file, encoded by chunks with `Transfer-Encoding` set to `chunked`.
    - a.k.a. **Chunked Transfer Encoding**
    - The response body is not wholy stored as a part of the response because it may be very large (affects program memory).
    - Therefore, instead, a stream of chunks of data (sequence of bytes) is sent.
- **Multiple-Resource Body**
  - Consist of a multipart body, in which each body contains a different bit of information.
  - These are rare to find.

## HTTP Timings
<p align="center">
  <img src="https://blog-assets.risingstack.com/2017/09/http-timing-nodejs.png" alt="HTTP Timings" width="80%" />
</p>

```js
const timings = {
  // use process.hrtime() as it's not a subject of clock drift
  startAt: process.hrtime(),
  dnsLookupAt: undefined,
  tcpConnectionAt: undefined,
  tlsHandshakeAt: undefined,
  firstByteAt: undefined,
  endAt: undefined
}

const req = http.request({ ... }, (res) => {
  res.once('readable', () => {
    timings.firstByteAt = process.hrtime();
  })
  res.on('data', (chunk) => { responseBody += chunk });
  res.on('end', () => {
    timings.endAt = process.hrtime();
  });
});

req.on('socket', (socket) => {
  socket.on('lookup', () => {
    timings.dnsLookupAt = process.hrtime();
  });
  socket.on('connect', () => {
    timings.tcpConnectionAt = process.hrtime();
  });
  socket.on('secureConnect', () => {
    timings.tlsHandshakeAt = process.hrtime();
  });
});
```

## Resource Formats
### MIME Type
> A **media type** (also known as a **Multipurpose Internet Mail Extensions** or **MIME type**) indicates the nature and format of a document, file, or assortment of bytes. | MDN

- Browsers use the MIME type (not the file extension) to decide how to process a URL.
  - This is why it is important that the server responds with the correct MIME type in the `Content-Type` header.
- Content Types
  ```
  text/plain
  text/html
  text/css
  image/jpeg
  image/png
  audio/mpeg
  audio/ogg
  audio/*
  video/mp4
  application/octet-stream
  multipart/mixed
  ```

## Reference
[Evolution of HTTP - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)  
[HTTP 3 Explained - YouTube](https://www.youtube.com/watch?v=ai8cf0hZ9cQ&ab_channel=High-PerformanceProgramming)  
[HTTP requests - IBM Documentation](https://www.ibm.com/docs/en/cics-ts/5.3?topic=protocol-http-requests)  
[HTTP responses - IBM Documentation](https://www.ibm.com/docs/en/cics-ts/5.3?topic=protocol-http-responses)  
[HTTP Messages - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)  
[HTTP headers - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)  
[Chunked transfer encoding - Wikipedia](https://en.wikipedia.org/wiki/Chunked_transfer_encoding)  
[Understanding & Measuring HTTP Timings with Node.js - RisingStack Engineering](https://blog.risingstack.com/measuring-http-timings-node-js/)  
[MIME types (IANA media types) - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)  
