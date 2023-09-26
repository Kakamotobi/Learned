# Real-time Communication

## Table of Contents
- [Stateless](#stateless)
  - [HTTP Polling](#http-polling)
  - [HTTP Long Polling](#http-long-polling)
  - [Server-Sent Events(SSE)](#server-sent-eventssse)
- [Stateful](#stateful)
  - [WebSocket](#websocket)
    - [WebSocket Handshake](#websocket-handshake)
    - [STOMP](#stomp)

## [Stateless](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Stateless-Stateful-Connections.md#stateless-connection)
### HTTP Polling
- HTTP is stateless meaning that it is a one time request/response cycle. The client has to initiate the communication by asking the server for something.
- Therefore, in order to achieve (pseudo) real-time communication, the client must continually ask the server for updates.
- Caveats
  - It is difficult to determine the appropriate rate at which the client should request for updates.
    - Too frequent means there will be lots of "no update" responses.
    - Too infrequent means there will be delayed updates (loses "real-time").
### HTTP Long Polling
- The client sends a request to the server. And the server keeps the connection open until there is data to send to the client.
- Upon receiving the data, the client immediately sends another long polling request to the server; repeating the process.
- Caveats/Considerations
  - How long should a server keep a single connection open?
### Server-Sent Events(SSE)
- SSE is a one-way connection from the server to the client.
- The client initializes an `EventSource` object with the URL of a script that generates events.

## [Stateful](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Stateless-Stateful-Connections.md#stateful-connection)
### WebSocket
- A communication protocol that allows asynchronous bidirectional communication between a client (web browser) and a server.
  - i.e. polling is not required.
- _Like HTTP, WebSocket is a TCP-based protocol in the Application Layer of the OSI model._
  - i.e. a WebSocket server is simply an application listening on a port of a TCP server that follows the WebSocket protocol.
- A WebSocket client and server _exchange variable-length [data frames](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers#format) instead of a stream_.
  - No headers are exchanged once the connection is established.
  - WebSocket can also use SSL (wss) in the same way that HTTP uses SSL (https).
- `WebSocket` API (client)
  - Once the WebSocket connection is established between the client and server, you can send data to the server using `send()` and receive data from the server using `onmessage`.
#### WebSocket Handshake
- The server listens for incoming socket connection attempts (HTTP request).
- The "handshake" refers to the bridge from HTTP to WebSockets.
  - Details of the connection are checked/agreed upon here.
##### Steps
1) The client initiates the WebSocket handshake by making an HTTP GET request to the server asking for a WebSocket connection.
    - The client negotiates extensions (optional) and subprotocols (mandatory) in the request headers.
      - Extensions: control the WebSocket frame and modify the payload.
        - Ex: compression.
      - Subprotocols: structure the WebSocket payload (no modification).
        - Simply establishes structure between the interested parties.
        - Ex: STOMP.
    - The request headers include `Sec-WebSocket-Key`, `Sec-WebSocket-Version`, `Sec-WebSocket-Protocol`.
2) The server receives the request and determines whether to open the WebSocket connection or reject the request.
    - In the response, the server indicates that the protocol will switch from HTTP to WebSocket.
    - The response headers include `Sec-WebSocket-Accept`, which is derived from the `Sec-WebSocket-Key` request header from the client.
3) The client receives a "success" response, indicating a successful handshake (WebSocket connection).
#### STOMP
- A text-oriented protocol that can be used as a subprotocol for WebSocket connections.
- _As a subprotocol to a protocol like WebSocket, STOMP is also in the Application Layer of the OSI model._
- STOMP is based on a _publish-subscribe_ mechanism where clients subscribe to a particular "topic" and will hence be notified of messages to that topic.
- P.S. STOMP is not WebSocket exclusive.
- [Other WebSocket subprotocols](https://www.iana.org/assignments/websocket/websocket.xml#subprotocol-name).

## Reference
[What is HTTP Long Polling? | PubNub](https://www.pubnub.com/blog/http-long-polling/)
[Using server-sent events - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)  
[RFC 6455 - The WebSocket Protocol](https://datatracker.ietf.org/doc/rfc6455/?include_text=1)  
[WebSocket - IBM Documentation](https://www.ibm.com/docs/en/was/9.0.5?topic=applications-websocket)  
[Writing WebSocket servers - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)  
[What is the difference between WebSocket and STOMP protocols? - Stack Overflow](https://stackoverflow.com/questions/40988030/what-is-the-difference-between-websocket-and-stomp-protocols)  
[Long Polling vs WebSockets - which to use in 2023](https://ably.com/blog/websockets-vs-long-polling)  
[STOMP - stomp.github.io](https://stomp.github.io/stomp-specification-1.2.html)  
