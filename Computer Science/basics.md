# Basics

## Table of Contents
- [Networking](#networking)
- [HTTP vs HTTPS](#http-vs-https)
- [Typing](#typing)

## Networking
> An interconnection of multiple devices, also known as hosts, that are connected using multiple paths for the purpose of sending/receiving data or media. Computer networks can also include multiple devices/mediums which help in the communication between two different devices; these are known as **Network devices** and include things such as routers, switches, hubs, and bridges. | GeeksforGeeks
- Open System: a system that is connected to the network and is ready for communication.
- Closed System: a system that is not connected to the network and hence, cannot be communicated with.
### Types of Networks
- Local Area Network (LAN)
  - Interconnect computers within a limited area (ex: home, school, office).
- Metropolitan Area Network (MAN)
  - Interconnect computers within a geographic region the size of a metropolitan area (ex: city).
- Wide Area Network (WAN)
  - Interconnect computers across the world (ex: the Internet).
  - A network of networks.
### Networking Devices
- **Switch**
  - A network switch is a networking hardware that connects devices on a computer network by using packet switching to receive and forward data to the destination device.
  - Sends message to selected destination ports.
  - **Packet Switching**
    - A method of data transmission in which a message is broken into several parts, called packets, that are sent independently, in triplicate, over whatever route is optimum for each packet, and reassembled at the destination.
    - Each packet contains a piece part, called the payload, and an identifying header that includes destination and reassembly information.
- **Hub**
  - A network hub is a networking hardware that connects devices in a LAN.
  - Sends message to all connected ports.
  - c.f. Switch.
- **Routers**
  - A networking device that is used to interconnect LANs to form a WAN.
  - IP routers use IP addresses to determine where to forward packets.
  - Routing allows multiple networks to communicate independently and yet remain separate.
- **Bridges**
  - A networking device that creates a single, aggregate network from multiple communication networks or network segments.
  - Bridging connects two separate networks as if they were a single network.
  - c.f. Routers.
### Related Concepts
- **TCP/IP Protocol Suite**
  - The set of rules or algorithms which define the ways of how two entities can communicate across the network.
- **Domain Name System(DNS) Server**
  - Special servers that match up the web address(URL) that we type in our browsers to their actual IP addresses.
- **Port**
  - A logical channel through which data can be sent/received to an application.
  - Any host may have multiple applications running, and each of these applications is identified using the port number on which they are running.
- **Host**
  - Each device in the network is associated with a unique device name known as Hostname.
- **Socket**
  - The unique combination of IP address and Port number.
  - Commonly used for client and server interaction where the client connects to the server, exchange information, and then disconnect.
  - Flow
    - The server first establishes (binds) an address (socket/endpoint) that clients can use to find the server.
    - The socket on the server process waits for requests from a client.
      - The client-to-server data exchange takes place when a client connects to the server through a socket.
    - The server performs the client's request and sends the reply back to the client.
### Reference
[Basics of Computer Networking | GeeksforGeeks](https://www.geeksforgeeks.org/basics-computer-networking/)  
[What is a WAN? Wide-Area Network - Cisco](https://www.cisco.com/c/en/us/products/switches/what-is-a-wan-wide-area-network.html#~types)  
[How sockets work - IBM](https://www.ibm.com/docs/en/i/7.3?topic=programming-how-sockets-work)  

---

## HTTP vs HTTPS
### Hypertext Transfer Protocol (HTTP)
- Allows for the communication between different systems (most commonly used to transfer data from a web server to a browser to allow users to view web pages).
- Information that flows from server to browser is not encrypted, which means it can easily be stolen.
### Hypertext Transfer Protocol Secure (HTTPS)
- Uses a **Secure Sockets Layer (SSL) certificate** or **Transport Layer Security (TLS) certificate**, which is an upgraded version of SSL, to create a secure encrypted connection between the server and the browser, thereby protecting potentially sensitive information from being stolen as it is transferred between the server and the browser.
- Crucial especially for websites requiring sensitive data (ex: credit card info, password).
- Can also boost SEO efforts.
- It is required for Accelerated Mobile Pages (AMP).
  - AMP: a way to load content onto mobile devices at a much faster rate.
### Mixed Content
> An HTTPS page that includes content fetched using cleartext HTTP is called a **mixed content page**. Pages like this are only partially encrypted, leaving the unencrypted content accessible to sniffers and man-in-the-middle attackers. That leaves the pages unsafe.
- Fix
  - Serve all the content as HTTPS instead of HTTP (http:// --> https://).
### Reference
[HTTP vs HTTPS](https://seopressor.com/blog/http-vs-https/)  
[Mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)  
[How to fix a website with blocked mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content/How_to_fix_website_with_mixed_content)

---

## Typing
### Static Typing vs. Dynamic Typing
#### Static Typing
- Variable types are checked at "compile-time" or before runtime.
- So, compilation error will be thrown before the code runs.
- Ex: if a function receives a string as an argument, when in fact it's expecting a number, a compilation error is thrown.
#### Dynamic Typing
- Variable types are checked on the go as the code is executed.
- Ex: a function accepts any type of argument, and only knows which type it is when the code is executed.
### Strong Typing vs. Weak Typing
### Strong Typing
- Converts a variable or value's type to suit the current situation automatically.
- Ex: "123" will always be treated as a string (never used as a number unless otherwise manually converted).
- Python Example
  ```py
  x = 1
  y = "2"
  z = x + y # TypeError
  ```
### Weak Typing
- The interpreter or compiler attempts to make the best of what it is given.
- Ex: in some situations an integer miht be treated as if it were a string to suit the context of the situation.
- JavaScript Example
  ```js
  const x = 1;
  const y = "2";
  const z = x + y; // "12" because JS treated both values as strings and concatenated them together.
  ```
### Reference
[Understanding Types: Static vs. Dynamic, & Strong vs. Weak](https://medium.com/@cpave3/understanding-types-static-vs-dynamic-strong-vs-weak-88a4e1f0ed5f)
