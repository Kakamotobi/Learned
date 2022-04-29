# Network Protocols

## Table of Contents
- [What are Network Protocols?](#what-are-network-protocols)
- [Types of Network Protocols](#types-of-network-protocols)
- [HTTP vs HTTPS](#http-vs-https)
- [TCP vs UDP vs IP vs TCP/IP](#tcp-vs-udp-vs-ip-vs-tcpip)
- [Protocol Stacks](#protocol-stacks)

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

## HTTP vs HTTPS
### Hypertext Transfer Protocol (HTTP)
- An application layer protocol that allows for the communication between different systems (most commonly used to transfer data from a web server to a browser to allow users to view web pages).
- Information that flows from server to browser is not encrypted, which means it can easily be stolen.
### Hypertext Transfer Protocol Secure (HTTPS)
- Uses a **Secure Sockets Layer (SSL) certificate** or **Transport Layer Security (TLS) certificate**, which is an upgraded version of SSL, to create a secure encrypted connection between the server and the browser, thereby protecting potentially sensitive information from being stolen as it is transferred between the server and the browser.
- Crucial especially for websites requiring sensitive data (ex: credit card info, password).
- Can also boost SEO efforts.
- It is required for Accelerated Mobile Pages (AMP).
  - AMP: a way to load content onto mobile devices at a much faster rate.
### Mixed Content
> An HTTPS page that includes content fetched using cleartext HTTP is called a **mixed content page**. Pages like this are only partially encrypted, leaving the unencrypted content accessible to sniffers and man-in-the-middle attackers. That leaves the pages unsafe.
- Fix: serve all the content as HTTPS instead of HTTP (http:// --> https://).

## TCP vs UDP vs IP vs TCP/IP
### Transmission Control Protocol (TCP)
- TCP is a transport layer protocol.
- It provides a reliable virtual-circuit connection between applications before data transmission begins.
- Data is received in the order that it was sent in.
- Data is treated as a stream of bytes.
### User Datagram Protocol (UDP)
- UDP is an alternative transport layer protocol.
- It provides an unreliable datagram connection between applications before data transmission begins.
- Data is transmitted link by link and there is no end-to-end connection.
- There is no guarantee of successful data transmission.
- Data can be lost, duplicated and can arrive out of order.
### Internet Protocol
- IP is a network layer protocol.
- It provides a datagram service between applications, which supports both TCP and UDP.
### TCP/IP
- TCP/IP simply refers to a large family of protocols. TCP and IP are the two most important protocols among them; hence, named TCP/IP.
<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/Computer%20Network/refImg/tcpip.png" alt="TCP/IP Illustration" width="80%" />
</p>


## Protocol Stacks
### Example 1 - regular, non-encrypted HTTP
| Layer               | Protocol   |
| ------------------- | ---------- |
| Application         | HTTP       |
| Transport           | TCP or UDP |
| Internet or Network | IP         |
| Link or Data Link   | Ethernet   |
### Example 2 - HTTPS
| Layer               | Protocol   |
| ------------------- | ---------- |
| Application         | HTTPS      |
|                     | TSL(SSL)   |
| Transport           | TCP or UDP |
| Internet or Network | IP         |
| Link or Data Link   | Ethernet   |

## Reference
[Types of Network Protocols and Their Uses - GeeksforGeeks](https://www.geeksforgeeks.org/types-of-network-protocols-and-their-uses/)  
[security - Difference between HTTPS and SSL - Stack Overflow](https://stackoverflow.com/questions/6093430/difference-between-https-and-ssl)  
[HTTP vs HTTPS](https://seopressor.com/blog/http-vs-https/)  
[TCP/IP TCP, UDP, and IP protocols - IBM Documentation](https://www.ibm.com/docs/en/zos/2.2.0?topic=internets-tcpip-tcp-udp-ip-protocols)  
[Mixed content - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)  
