# Network Protocols

## Table of Contents
- [What are Network Protocols?](#what-are-network-protocols)
- [Types of Network Protocols](#types-of-network-protocols)
- [TCP vs UDP vs IP vs TCP/IP](#tcp-vs-udp-vs-ip-vs-tcpip)
- [Transport Layer Security(TLS)](#transport-layer-securitytls)
- [HTTP vs HTTPS](#http-vs-https)
- [Protocol Suites](#protocol-suites)
- [Secure Shell(SSH)](#secure-shellssh)

## What are Network Protocols?
> A network protocol is an accepted set of rules that govern data communication between different devices in the network. It determines what is being communicated, how it is being communicated, and when it is being communicated. It permits connected devices to communicate with each other, irrespective of internal and structural differences. | GeeksforGeeks

## Types of Network Protocols
- There are largely three major categories.
### Communication
- Network communication protocols formally describe the rules and format by which data is transferred over the network.
- They handle syntax, semantics, error detection, synchronization and authentication.
- Ex: HTTP, TCP/IP.
### Management
- Network management protocols help define procedures and policies used to monitor, manage and maintain your computer network, and help communicate these needs across the network to ensure stable communication and optimal performance across the board.
- Ex: ICMP, SNMP, FTP.
### Security
- Network security protocols secure the data in transit over the network.
- They also determine how the network secures data from unauthorized extraction attemptions.
- They primarily depend on encryption to secure data.
- Ex: HTTPS, SSL, TLS.

## TCP vs UDP vs IP vs TCP/IP
### Transmission Control Protocol (TCP)
- TCP is a transport layer protocol.
- It provides a reliable virtual-circuit connection between applications before data transmission begins.
  - i.e. a session is established before data transmission.
- Data is received in the order that it was sent in.
- Data is treated as a stream of bytes.
#### TCP 3-Way Handshake
- The communication between the client and the server concerning the parameters of the connection before TLS handshake and transmitting data such as HTTP requests.
- The three messages are: SYN, SYN + ACK, ACK.
  - SYN: Synchronize sequence numbers.
  - ACK: Acknowledgement
##### The Process
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/refImg/tcp.png" alt="TCP Illustration" width="80%" />
</p>

1) The client sends a SYN packet to the server.
2) The server receives the SYN packet from the client. Then, creates a connection and responds by sending a SYN + ACK to the client.
3) The client receives the SYN + ACK. Then, sends an ACK to the server. Now, the TCP socket connection between the client and server is established.
### User Datagram Protocol (UDP)
- UDP is an alternative transport layer protocol.
- It provides an unreliable datagram connection between applications before data transmission begins.
- Data is transmitted link by link and there is no end-to-end connection.
  - i.e. does not establish a session.
  - When a computer sends data, it does not care whether the data is received at the other end (hence called a "fire and forget" protocol).
- There is no guarantee of successful data transmission.
  - With less overhead of guaranteeing successful data transmission, UDP is faster than TCP.
- Data can be lost, duplicated and can arrive out of order.
### Internet Protocol (IP)
- IP is a network layer protocol.
- It provides a datagram service between applications, which supports both TCP and UDP.
- IP is used to identify different computers on the network by signing each computer a unique IP address.
### TCP/IP
- TCP/IP simply refers to a large family of protocols. TCP and IP are the two most important protocols among them; hence, named TCP/IP.
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/refImg/tcpip.png" alt="TCP/IP Illustration" width="80%" />
</p>

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
### Example 1 - regular, non-encrypted HTTP
| Layer               | Protocol |
| ------------------- | -------- |
| Application         | HTTP     |
| Transport           | TCP/UDP  |
| Internet or Network | IP       |
| Link or Data Link   | Ethernet |
### Example 2 - HTTPS
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

<p align="center">
  <img src="https://www.ssh.com/hubfs/Imported_Blog_Media/SSH_simplified_protocol_diagram-2.png" alt="SSH Key-based Authentication" width="80%" />
</p>




## Reference
[Types of Network Protocols and Their Uses - GeeksforGeeks](https://www.geeksforgeeks.org/types-of-network-protocols-and-their-uses/)  
[TCP/IP TCP, UDP, and IP protocols - IBM Documentation](https://www.ibm.com/docs/en/zos/2.2.0?topic=internets-tcpip-tcp-udp-ip-protocols)  
[TCP 3-Way Handshake Process - GeeksforGeeks](https://www.geeksforgeeks.org/tcp-3-way-handshake-process/)  
[Mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)  
[Transport Layer Security - Wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security)  
[An overview of the SSL or TLS handshake - IBM Documentation](https://www.ibm.com/docs/en/ibm-mq/7.5?topic=ssl-overview-tls-handshake)  
[What happens in a TLS handshake? | SSL Handshake](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/#:~:text=A%20TLS%20handshake%20is%20the,and%20agree%20on%20session%20keys.)  
[TLS 1.2 and TLS 1.3 Handshake Walkthrough | by Carson | Medium](https://cabulous.medium.com/tls-1-2-andtls-1-3-handshake-walkthrough-4cfd0a798164)  
[security - Difference between HTTPS and SSL - Stack Overflow](https://stackoverflow.com/questions/6093430/difference-between-https-and-ssl)  
[HTTP vs HTTPS](https://seopressor.com/blog/http-vs-https/)  
[What is SSH (Secure Shell)? | SSH Academy](https://www.ssh.com/academy/ssh)  
