# Network Protocols

## Table of Contents
- [What are Network Protocols?](#what-are-network-protocols)
  - [Types of Network Protocols](#types-of-network-protocols)
- [Open Systems Interconnection(OSI) Model](#open-systems-interconnectionosi-model)
- [TCP vs UDP vs QUIC](#tcp-vs-udp-vs-quic)
  - [Transmission Control Protocol(TCP)](#transmission-control-protocoltcp)
    - [TCP 3-Way Handshake](#tcp-3-way-handshake)
    - [TCP Termination](#tcp-termination)
    - [TCP Header](#tcp-header)
  - [User Datagram Protocol(UDP)](#user-datagram-protocoludp)
    - [UDP Header](#udp-header)
  - [QUIC](#quic)
  - [Transport Layer Protocols in relation to the Evolution of HTTP](#transport-layer-protocols-in-relation-to-the-evolution-of-http)
- [Sockets](#sockets)
  - [Types of Sockets](#types-of-sockets)
    - [Stream Sockets (for TCP)](#stream-sockets-for-tcp)
      - [(Stream) Socket API](#stream-socket-api)
    - [Datagram Sockets (for UDP)](#datagram-sockets-for-udp)
    - [Raw Sockets](#raw-sockets)
- [Ports](#ports)
- [Transport Layer Security(TLS)](#transport-layer-securitytls)
  - [TLS Handshake](#tls-handshake)
- [HTTP vs HTTPS](#http-vs-https)
- [Protocol Suites](#protocol-suites)
  - [TCP/IP Suite](#tcpip-suite)
    - [Internet Protocol(IP)](#internet-protocolip)
  - [Regular, non-encrypted HTTP](#regular-non-encrypted-http)
  - [Encrypted HTTP(HTTPS)](#encrypted-httphttps)
- [Secure Shell(SSH)](#secure-shellssh)
- [Some CLI Network Commands](#some-cli-network-commands)

## What are Network Protocols?
> A network protocol is an accepted set of rules that govern data communication between different devices in the network. It determines what is being communicated, how it is being communicated, and when it is being communicated. It permits connected devices to communicate with each other, irrespective of internal and structural differences. | GeeksforGeeks

- A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.
- All activity on the Internet that involves two or more communicating remote entities is governed by a protocol.
### Types of Network Protocols
- There are largely three major categories.
#### Communication
- Network communication protocols formally describe the rules and format by which data is transferred over the network.
- They handle syntax, semantics, error detection, synchronization and authentication.
- Ex: HTTP, TCP/IP.
#### Management
- Network management protocols help define procedures and policies used to monitor, manage and maintain your computer network, and help communicate these needs across the network to ensure stable communication and optimal performance across the board.
- Ex: ICMP, SNMP, FTP.
#### Security
- Network security protocols secure the data in transit over the network.
- They also determine how the network secures data from unauthorized extraction attemptions.
- They primarily depend on encryption to secure data.
- Ex: HTTPS, SSL, TLS.

## Open Systems Interconnection(OSI) Model
- The OSI model _attempts_ to represent the abstract layers of communication between computing systems.
- Higher layers have gone through the lower layers.
  - i.e. lower layers are not aware of what happens in higher layers.

<p align="center">
  <img src="https://www.tech-faq.com/wp-content/uploads/2009/01/osimodel.png" alt="OSI Model" width="50%" />
  <img src="https://media.fs.com/images/community/upload/kindEditor/202107/29/original-seven-layers-of-osi-model-1627523878-JYjV8oybcC.png" alt="OSI Model" width="50%" />
</p>

- **Application**
  - The Application Layer involves high-level protocols for human-computer interaction.
    - i.e. the actual message being sent.
  - Browsers rely on this layer to initiate communication.
  - Ex: HTTP/HTTPS, SSH, SMTP.
  - Data Unit: data
- **Presentation**
  - The Presentation Layer is responsible for:
    - Translating incoming data to a format that the receiver can understand.
    - Encrypting/Decrypting the data.
    - Compressing data, which was received from the Application Layer, before passing it on to the Session Layer.
  - Data Unit: data
- **Session**
  - The Session Layer is responsible for establishing and maintaining _sessions_ and _ports_ for continuous transmissions between the two network nodes.
  - Data Unit: data
- **Transport**
  - The Transport Layer is responsible for actually transmitting the data segments between endpoints on the network.
    - These may be connection-oriented or connectionless.
  - Ex: TCP, UDP.
  - Data Unit: segment or datagram.
- **Network**
  - The Network Layer is responsible for deciding the actual path on the network that the data will pass through.
  - Ex: IP.
  - Data Unit: packet
- **Data Link**
  - The Data Link Layer is responsible for the node-to-node transmission of data.
  - Data Unit: frame
- **Physical**
  - The Physical Layer is responsible for the transmission of raw bit streams over the physical connection between devices (cables/wires).
  - Data Unit: bit

## TCP vs UDP vs QUIC
- TCP, UDP, and QUIC are different transport layer protocols.
- _These protocols are the first protocol that a client and server establish before actually transmitting data._
### Transmission Control Protocol(TCP)
- **TCP is a connection-oriented protocol that first establishes a reliable virtual-circuit connection between two applications before starting data transmission.**
  - i.e. end-to-end connection.
- In TCP, data is sent as a stream of bytes, where they are received in the order that they were sent in.
- TCP is useful for web browsing, email, file transfer, etc.
#### TCP 3-Way Handshake
- TCP: before anything, the client and server first "discuss" the parameters of the connection.
- This is done through the use of three messages: SYN, SYN + ACK, ACK.
  - SYN: Synchronized sequence numbers.
  - ACK: Acknowledgement

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/tcp-handshake.png" alt="TCP 3-Way Handshake" width="80%" />
</p>

1. The client sends a SYN(n) packet to the server.
2. The server receives the client's SYN(n) packet and then sends SYN(k) and ACK(n+1) packets to the client.
   - _The client receives the ACK(n+1) packet and confirms that the server was indeed the recipient of its SYN(n) packet._
3. The client sends an ACK(k+1) packet to the server.
   - _The server receives the ACK(k+1) packet and confirms that the client was indeed the recipient of its SYN(k) packet._
   - The TCP socket connection is established.
#### TCP Termination
- a.k.a. **TCP 4-Way Handshake**
- When terminating a TCP connection, a 4-way handshake is used.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/tcp-termination.png" alt="TCP Connection Termination" width="80%" />
</p>

1. The client sends a FIN(f) packet to the server.
2. The server receives the client's FIN(f) packet and then sends an ACK(f+1) packet to the client.
   - _The client receives the ACK(f+1) packet and confirms that the server was indeed the recipient of its FIN(f) packet._
3. The server then continues to send a FIN(j) packet to the client.
   - _The server waits for the application to be ready to close and then sends the FIN packet._
4. The client receives the server's FIN(j) packet and then sends an ACK(j+1) packet to the server.
   - _The server receives the ACK(j+1) packet and confirms that the client was indeed the recipient of its FIN(j) packet._
   - The TCP connection is successfully terminated.
#### TCP Header
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/tcp-header.png" alt="TCP Header" width="80%" />
</p>

- **Source Port**
  - The port number of the sender (Ex: client).
- **Destination Port**
  - The port number of the receiver (Ex: server).
- **Sequence Number**
  - Indicates how much data is sent during the TCP session.
  - The initial sequence number (SYN) is a random 32-bit value.
- **Acknowledgement Number**
  - The ACK packet is a 32-bit field value that is the SYN packet incremented by 1.
  - The receiver sends this back to the sender in order to get the next TCP segment.
- **Header Length**
  - a.k.a. **Data Offset(DO)**
  - The _data offset_ field, which indicates the length of the TCP header, which in turn reveals where the actual data begins.
- **Reserved**
  - Unused bits that are always set to 0.
- **Flags**
  - URG
    - Indicates whether the Urgent Pointer field is filled or not.
    - i.e. indicating whether the data should be prioritized over other data or not.
  - ACK
    - Indicates whether the Acknowledgement Number field is filled or not.
  - PSH
    - Indicates that the buffered data should be "pushed" to the OS asap.
    - i.e. if the PSH flag is `1`, it means there is no more connected segments.
  - RST
    - Indicates that the connection should be reset.
    - i.e. the connection should be terminated right away, usually due to some unrecoverable errors.
  - SYN
    - Indicates to synchronize the sequence numbers.
    - Only the first packet sent from each end should have this flag on.
  - FIN
    - Used to end the connection.
    - Since TCP is full duplex, both parties have to use this flag to end the connection.
- **Window**
  - Specifies the number of bytes that can be transmitted at once.
  - The receiver uses this to inform the sender that it
- **Checksum**
  - Used to check for any errors in the TCP header and payload.
- **Urgent Pointer**
  - Indicates the offset from the Sequence Number for the last urgent data byte.
- **Options**
  - An optional field to specify any additional information.
### User Datagram Protocol(UDP)
- a.k.a. "fire and forget" protocol.
- **UDP is a connectionless protocol, based on unreliable datagrams, that does not establish any connection between two applications before starting data transmission.**
  - _Datagram_ is a transfer unit (consisting header and payload sections) for connectionless communication services within a network that uses packet-switching.
- Data is transmitted link by link (no end-to-end connection).
  - i.e. it does not establish a session.
  - i.e. the transmitting computer simply sends the data but does not make sure the data is received by the receiving computer.
- There is no guarantee that the data is successfully transmitted (data can be lost, duplicated, arrive out of order). Therefore, UDP is faster than TCP.
- UDP is useful for broadcasting, video streaming, multiplayer games, etc.
#### UDP Header
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/udp-header.png" alt="UDP Header" width="80%" />
</p>

### QUIC
- QUIC is a combination of TCP and UDP features, and hence result to lower latency and quicker loads.
- QUIC improves performance of by establishing several multiplexed connections between the two endpoints using UDP.
- QUIC packets are encapsulated on top of UDP datagrams; meaning that QUIC streams are delivered independently.
  - Therefore, one lost packet does not affect the others.
#### QUIC Connection
- _QUIC combines the **TCP 3-Way Handshake** and the **TLS1.3 Handshake**._
  - i.e. the transport and crypto protocols are combined into one, which allows for the connection to be established in just one round trip.

<p align="center">
  <img src="https://blog.cloudflare.com/content/images/2018/07/http-request-over-quic@2x.png" alt="QUIC Handshake" width="60%" />
</p>

1) The client sends a connection request (QUIC Initial, Client Hello) to the server.
2) The server sends a connection response (QUIC Initial, Server Hello) to the client.
3) The client sends a confirmation (Client Finished) to the server.

### Transport Layer Protocols in relation to the Evolution of HTTP
- In HTTP 1.1, each single request and response cycle requires a separate TCP connection.
  - Therefore, 3 client requests (Ex: script.js, styles.css, image.png) require 3 TCP connections with the server.
- As **HTTP/2** was standardized in 2015, it became possible to complete multiple request/response cycles in a single TCP connection.
  - Therefore, 3 client requests require only 1 TCP connection with the server.
  - However, as the TCP approach still requires some overhead, it becomes inefficient for HTTP requests requiring frequent connection.
- In an attempt to mitigate overhead in the TCP approach, **HTTP/3** was standardized in 2022.
  - HTTP/3 is uses a multiplexed transport layer called **QUIC**, which is built on **UDP**.

## Sockets
- Before HTTP communication is allowed, a TCP connection between the client and server must first be established. When establishing this TCP connection, **sockets** are used.
- **A socket is an interface (software endpoint) that the OS provides for users to read/write from one endpoint to another endpoint over a network.**
  - _A Socket is identified by an **IP Address** and a **Port Number**, which together form a unique network address/endpoint._
  - Each socket has a unique name called a **socket descriptor**.

<p align="center">
  <img src="https://docs.oracle.com/javase/tutorial/figures/networking/5connect.gif" alt="Socket Connection Request" width="50%" />
</p>
<p align="center">
  <img src="https://docs.oracle.com/javase/tutorial/figures/networking/6connect.gif" alt="Socket Connection Response" width="50%" />
</p>

### Types of Sockets
#### Stream Sockets (for TCP)
> A stream socket provides a bidirectional, reliable, sequenced, and unduplicated flow of data with no record boundaries. After the connection has been established, data can be read from and written to these sockets as a byte stream. The socket type is `SOCK_STREAM` | Oracle
##### (Stream) Socket API
- Most programming languages provide a socket interface.
  - Ex: `net` library in Node.js.
- All HTTP libraries (Ex: Fetch API, Axios) are based on sockets. Therefore, we can get resources using the sockets API.
  - Ex: `$ curl www.google.com`, `http.get()` are all based on sockets.
- All we need are: the target server's IP address and Port number, and our IP address and Port number (taken care of by the OS).
- Flow Example
  1. Create a socket (client).
  2. Connect this socket to an IP and Port.
  3. Send a request over that socket-to-socket connection.
     - This data can be anything (Ex: simple text (HTTP message, etc.), media file).
     - If sending an HTTP message, follow the standard and structure (request line, headers, meta information, body).
  4. Receive the response.
#### Datagram Sockets (for UDP)
> A datagram socket supports a bidirectional flow of messages. A process on a datagram socket can receive messages in a different order from the sending sequence. A process on a datagram socket can receive duplicate messages. Record boundaries in the data are preserved. The socket type is `SOCK_DGRAM`. | Oracle
#### Raw Sockets
> Raw sockets provide access to ICMP. These sockets are normally datagram oriented, although their exact characteristics are dependent on the interface provided by the protocol. Raw sockets are not for most applications. Raw sockets are provided to support the development of new communication protocols, or for access to more esoteric facilities of an existing protocol. Only superuser processes can use raw sockets. The socket type is `SOCK_RAW`. | Oracle

## Ports
- **A Port Number identifies a specific process/application on the computer that is communicating over the network.**
- Ports make it easy to distinguish between the different kinds of traffic (Ex: email, webpage) that come through the same Internet connection.
- cf. IP Address
  - An IP Address represents a specific device. Whereas, a Port Number represents a specific process/service/application within the device.
- There are 65,535 possible number of ports.
### Some Port Number Standards
- **20**, **21:** FTP
- **22:** SSH
- **53:** DNS
- **80:** HTTP
- **443:** HTTPS

## Transport Layer Security(TLS)
> The TLS protocol aims primarily to provide cryptography, including privacy (confidentiality), integrity, and authenticity through the use of certificates, between two or more communicating computer applications. | Wikipedia
- TLS is an upgraded version of Secure Sockets Layer(SSL).
### TLS Handshake
- The TLS handshake refers to the exchange of messages between the client and server to identify and verify each other, then establish the encryption algorithms and session keys to be used.
- The TLS handshake serves to start off the communication session between the client and server.
- The TLS handshake is fundamental to HTTPS.
  - Therefore, it also occurs during API calls.
#### The Process
- TLS 1.3, which was finalized in 2018, reduces the handshake to one round trip as to two round trips in TLS 1.2, thereby improving latency.
##### TLS 1.2
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/refImg/tls1.2.png" alt="TLS 1.2" width="80%" />
</p>

1) The client initiates the handshake by sending a "client hello" message along with its TLS version, cipher algorithms and data compression methods that it supports, and a string of random bytes (client random) to the server.
2) In response, the server sends a "server hello" message along with the TLS version, the selected cipher algorithm and compression method, its public certificate (containing the server's public key), and a string of random bytes (server random) to the client.
3) The client verifies the server's certificate against its trusted Certificate Authorities(CAs). If the certificate can be trusted, the client generates and sends one more string of random bytes called *premaster secret* that has been encrypted with the server's public key. This string can only be decrypted with the private key on the server. The client sends a "finished" message.
4) The server decrypts the premaster secret using its private key. At this point, both the client and the server have the same master secrets that are generated using the client random, server random, and the premaster secret. *Subsequent communications between the client and server are encrypted with this master secret.* The server sends a "finished" messaged.
##### TLS 1.3
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/refImg/tls1.3.png" alt="TLS 1.3" width="80%" />
</p>

1. The client initiates the handshake by sending a "client hello" message along with its TLS version, cipher alogrithms, data compression methods that it supports, a string of random bytes (client random), and a client key share.
2. The server generates the premaster secret using the client key share and server key share (that it just generated). It then generates the master secret using the client random, server random (that it just generated), and premaster secret. The server sends a "server hello" message along with the TLS version, the selected cipher algorithm and compression method, the server random, and server key share.
3. The client generates the premaster secret using the client key share and server key share. It then generates the master secret using the client random, server random, and premaster secret. *Subsequent communications between the client and server are encrypted with this master secret.*

## HTTP vs HTTPS
### Hypertext Transfer Protocol (HTTP)
- An application layer protocol that allows for the communication between different systems (most commonly used to transfer data from a web server to a browser to allow users to view web pages).
- Information that flows from server to browser is not encrypted, which means it can easily be stolen.
### Hypertext Transfer Protocol Secure (HTTPS)
- Uses an **SSL certificate** or **TLS certificate** to create a secure encrypted connection between the server and the browser, thereby protecting potentially sensitive information from being stolen as it is transferred between the server and the browser.
- Crucial especially for websites requiring sensitive data (ex: credit card info, password).
- Can also boost SEO efforts.
- It is required for Accelerated Mobile Pages (AMP).
  - AMP: a way to load content onto mobile devices at a much faster rate.
### Mixed Content
> An HTTPS page that includes content fetched using cleartext HTTP is called a **mixed content page**. Pages like this are only partially encrypted, leaving the unencrypted content accessible to sniffers and man-in-the-middle attackers. That leaves the pages unsafe.
- Fix: serve all the content as HTTPS instead of HTTP (http:// --> https://).

## Protocol Suites
### TCP/IP Suite
- "TCP/IP" simply refers to a large family/set of protocols.
  - It is called so simply because TCP and IP are the two most important protocols in this set of protocols.
- In the TCP/IP model, the Application, Presentation, Session Layers are integrated as a single layer as "Application Layer".

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/tcpip.png" alt="TCP/IP Illustration" width="80%" />
</p>

#### Internet Protocol(IP)
- IP is a network layer protocol.
- It provides a datagram service between applications, which supports both TCP and UDP.
- IP is used to identify different computers on the network by signing each computer a unique IP address.
### Regular, non-encrypted HTTP
| Layer               | Protocol |
| ------------------- | -------- |
| Application         | HTTP     |
| Transport           | TCP/UDP  |
| Internet or Network | IP       |
| Link or Data Link   | Ethernet |
### Encrypted HTTP(HTTPS)
<table>
  <thead>
    <tr>
      <th>Layer</th>
      <th>Protocol</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan=2>Application</td>
      <td>HTTPS</td>
    </tr>
    <tr>
      <td>TLS/SSL</td>
    </tr>
    <tr>
      <td>Transport</td>
      <td>TCP/UDP</td>
    </tr>
    <tr>
      <td>Internet or Network</td>
      <td>IP</td>
    </tr>
    <tr>
      <td>Link or Data Link</td>
      <td>Ethernet</td>
    </tr>
  </tbody>
</table>

## Secure Shell(SSH)
> The SSH protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network. | SSH Academy

- SSH is used for remote login and command-line execution.
- SSH works by an SSH client connecting to an SSH server (called SSHD).
- SSH is an application layer that works over TCP, since it needs a secure connection.

<p align="center">
  <img src="https://www.ssh.com/hubfs/Imported_Blog_Media/SSH_simplified_protocol_diagram-2.png" alt="SSH Key-based Authentication" width="80%" />
</p>

# Some CLI Network Commands
- `$ ping`
- `$ ifconfig`
- `$ whois <IP Address or URL>`
- `$ curl <IP Address or URL>`
- `$ nslookup <Domain Name>`
- `$ traceroute <IP Address or URL>`
- `$ telnet <host>`
  - Telnet is not a secure protocol. Use SSH instead.
- `$ netstat -a`
  - Shows the state of all currently existing sockets.
- `$ netstat -s`
  - Shows the statistics of each protocol (Ex: `tcp`, `udp`, `ip`, etc.).

## Reference
[Telephone network](https://en.wikipedia.org/wiki/Telephone_network)  
[Introduction to Segment Routing - Cisco](https://learningnetwork.cisco.com/s/blogs/a0D3i000002SKP6EAO/introduction-to-segment-routing)  

[Types of Network Protocols and Their Uses - GeeksforGeeks](https://www.geeksforgeeks.org/types-of-network-protocols-and-their-uses/)  

[OSI model - Wikipedia](https://en.wikipedia.org/wiki/OSI_model)  
[What is the OSI Model? | Cloudfarez](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)  
[The OSI Model – What It Is; Why It Matters; Why It Doesn’t Matter. - Tech-FAQ](https://www.tech-faq.com/osi-model.html)  
[TCP/IP vs. OSI: What’s the Difference Between them? | FS Community](https://community.fs.com/blog/tcpip-vs-osi-whats-the-difference-between-the-two-models.html)  

[Transmission Control Protocol - Wikipedia](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)  
[TCP 3-Way Handshake Process - GeeksforGeeks](https://www.geeksforgeeks.org/tcp-3-way-handshake-process/)  
[Why TCP Connect Termination Need 4-Way-Handshake? - GeeksforGeeks](https://www.geeksforgeeks.org/why-tcp-connect-termination-need-4-way-handshake/)  
[User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)  
[Datagram - Wikipedia](https://en.wikipedia.org/wiki/Datagram)  
[TCP/IP TCP, UDP, and IP protocols - IBM Documentation](https://www.ibm.com/docs/en/zos/2.2.0?topic=internets-tcpip-tcp-udp-ip-protocols)  
[Modernizing the internet with HTTP/3 and QUIC | Fastly](https://www.fastly.com/blog/modernizing-the-internet-with-http3-and-quic)  
[HTTP 3 Explained - YouTube](https://www.youtube.com/watch?v=ai8cf0hZ9cQ&ab_channel=High-PerformanceProgramming)  

[Basic introduction to HTTP requests with TCP Sockets in NodeJs | Aditya Thebe](https://www.adityathebe.com/raw-http-request-with-sockets-nodejs/)  
[What is a socket? - IBM Documentation](https://www.ibm.com/docs/en/zos/2.3.0?topic=services-what-is-socket)  
[What Is a Socket? (The Java™ Tutorials > Custom Networking > All About Sockets)](https://docs.oracle.com/javase/tutorial/networking/sockets/definition.html)  
[What is a computer port? | Ports in networking | Cloudflare](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)  
[Socket Types (Programming Interfaces Guide) | Oracle](https://docs.oracle.com/cd/E19683-01/816-5042/sockets-4/index.html)  

[What is a computer port? | Ports in networking | Cloudflare](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)  

[Mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)  
[Transport Layer Security - Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security)  
[An overview of the SSL or TLS handshake - IBM Documentation](https://www.ibm.com/docs/en/ibm-mq/7.5?topic=ssl-overview-tls-handshake)  
[What happens in a TLS handshake? | SSL Handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/#:~:text=A%20TLS%20handshake%20is%20the,and%20agree%20on%20session%20keys.)  
[TLS 1.2 and TLS 1.3 Handshake Walkthrough | by Carson | Medium](https://cabulous.medium.com/tls-1-2-andtls-1-3-handshake-walkthrough-4cfd0a798164)  
[security - Difference between HTTPS and SSL - Stack Overflow](https://stackoverflow.com/questions/6093430/difference-between-https-and-ssl)  
[HTTP vs HTTPS](https://seopressor.com/blog/http-vs-https/)  

[What is SSH (Secure Shell)? | SSH Academy](https://www.ssh.com/academy/ssh)  
