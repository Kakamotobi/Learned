# HTTP Communication
- An HTTP communication consists of two parts: the HTTP Request sent by the client and the HTTP Repsonse sent by the server.

## HTTP Request
- An HTTP Request consists of three elements: a request line, headers, and a body (if needed).
### Request Line
- The Request Line consists of three key elements: an HTTP method, request target, http version.
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
#### 2 Categories of Body
- **Single-Resource Bodies**
  - Consists of a single file, defined by two headers: `Content-Type` and `Content-Length`.
- **Multiple-Resource Bodies**
  - Consists of a multipart body, in which each body contains a different bit of information.
  - Typical with HTML forms.
  - `Content-Type: multipart/form-data`.

## Reference
[HTTP requests - IBM Documentation](https://www.ibm.com/docs/en/cics-ts/5.3?topic=protocol-http-requests)  
[HTTP Messages - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)  
[HTTP headers - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)  
