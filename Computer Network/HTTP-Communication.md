# HTTP Communication
- An HTTP communication consists of two parts: the HTTP Request sent by the client and the HTTP Repsonse sent by the server.

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

## Reference
[HTTP requests - IBM Documentation](https://www.ibm.com/docs/en/cics-ts/5.3?topic=protocol-http-requests)  
[HTTP responses - IBM Documentation](https://www.ibm.com/docs/en/cics-ts/5.3?topic=protocol-http-responses)  
[HTTP Messages - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)  
[HTTP headers - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)  
