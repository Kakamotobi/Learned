# When you Request a Page in your Browser

## 1) User Input
- As you press a letter into the URL bar, the browser takes it and looks for suggestions from your search history, bookmark, etc.

## 2) URL Parsing
- When you press enter or click on a suggestion, the browser parses the URL to get the protocol, domain, port, path, query, etc.

## 3) Find the Domain Name's corresponding IP Address
- The browser checks the browser cache, then the OS cache, then the Router cache, then the Local DNS Server (ISP) cache.
- If at this point, the IP address has not been found yet, the **[DNS query](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/DNS.md#dns-query)** process starts.

## 4) Setup Protocols - Open Socket, TCP Handshake, TLS Handshake
- Now that the browser obtained the IP address of the target server, it requests a **TCP socket stream**, which initiates the **[TCP handshake](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Network-Protocols.md#tcp-3-way-handshake)**.
  - In a socket stream, there are two sockets, one at each end of the communication (client and server).
    - A **[socket](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Computer-Network.md#related-concepts)** has a specific address, which consists of an IP address and a port number.
    - The server listens to the socket, waiting for a client to make a connection request.
    - The client creates a socket as well.
- Now that the sockets have been opened and TCP connection is established, the **[TLS handshake](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/Network-Protocols.md#tls-handshake)** starts off the communication session.
- When the TLS connection is established, data transfer begins.

## 5) HTTP Protocol
- The client and server now begin **[HTTP communication](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/HTTP-Communication.md)**.
- The client makes a request and the server responds with the resources (HTML, CSS, JS).

## 6) Browser Engine
- Manages actions between the UI and the rendering engine.

## 7) Rendering Engine
- Ex: Chrome uses Blink.
- **[Critical Rendering Path](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Browsers.md#critical-rendering-path)**

## 8) [JS Engine/Interpreter](https://github.com/Kakamotobi/Learned/blob/main/JS/JavaScript.md#javascript-engine)
- Ex: Chrome uses V8.
- **[JIT Compiler](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#just-in-timejit-compiler)**

## Reference
[How Browsers Work: Behind the scenes of modern web browsers - HTML5 Rocks](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)  
[vasanthk/how-web-works: What happens behind the scenes when we type www.google.com in a browser?](https://github.com/vasanthk/how-web-works)  
[Socket in Computer Network: GeeksforGeeks](https://www.geeksforgeeks.org/socket-in-computer-network/)  

