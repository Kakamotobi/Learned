# Computer Network

## Table of Contents
- [What is Computer Networking?](#what-is-computer-networking)
- [The Internet vs The Web](#the-internet-vs-the-web)
- [The Internet](#the-internet)
  - [Overview](#overview)
  - [Network Edge](#network-edge)
    - [Access Network](#access-network)
  - [Network Core](#network-core)
    - [Circuit Switching](#circuit-switching)
    - [Packet Switching](#packet-switching)
- [Types of Networks](#types-of-networks)
- [Terminologies](#terminologies)
- [Related Concepts](#related-concepts)

## What is Computer Networking?
> An interconnection of multiple devices, also known as hosts, that are connected using multiple paths for the purpose of sending/receiving data or media. Computer networks can also include multiple devices/mediums which help in the communication between two different devices; these are known as **Network devices** and include things such as routers, switches, hubs, and bridges. | GeeksforGeeks

> Computer networking refers to interconnected computing devices that can exchange data and share resources with each other. These networked devices use a system of rules, called communications protocols, to transmit information over physical or wireless technologies. | AWS

- Computer networks are essentially made up of nodes and links. 
  - A network node may be some sort of data communication equipment (ex: modem, hub, switch, or data terminal equipment such as two or more computers and printers).
  - A network link is the transmission media between two nodes (ex: physical hardware such as cable wires, optical fibers, or wireless).

## The Internet vs The Web
### What is the Internet?
- **The technical infrastructure that connects computers and networks of computers; that use TCP/IP to communicate.**
  - A network of actual hardware/wires (in the sea, server, ISP, client).
- Data is sent in small chunks of packets for quick transmission.
- Routers exist to to make sure each data packets are sent to the correct address.
- Analogy: the roads, motorways, etc.
### What is the World Wide Web (Web)?
- **The Web is an application that runs on the Internet.**
- The pages that we see when using a device and we are online.
- Clients and Servers are computers that are connected to the Web.
- Analogy: the houses, shops, cafés on the roads are like the web servers hosting websites.

## The Internet
### Overview
<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/figure1.1-some-pieces-of-the-internet.png" alt="Figure 1.1 - Some Pieces of the Internet" />
  <p><em>Computer Network: Top-down Approach - Figure 1.1</em></p>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/figure1.15-interconnection-of-isps.png" alt="Figure 1.15 - Interconnection of ISPs" width="80%" />
  <p><em>Computer Network: Top-down Approach - Figure 1.15</em></p>
</div>

### Network Edge
- The first step between End-Systems and the Network Core.
#### Access Network
<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/figure1.4-access-networks.png" alt="Figure 1.4 - Access Networks" />
  <p><em>Computer Network: Top-down Approach - Figure 1.4</em></p>
</div>

- The network that physically connects an End-System to the first Router (a.k.a. Edge Router) on a path from the End-System to any other distant End-System.
##### Components
- **End-System**
  - a.k.a. hosts.
  - Ex: desktop computers, mobile devices, servers.
- **Modem**
  - Device that connects your home to your ISP through a physical connection, enabling access to the Internet.
  - Usually provided by an Access ISP (Ex: telco or cable company, university, company).
- **Router**
  - Device that creates a LAN. Connects to your Modem and then to your devices.
  - i.e. connects your End-System to your home network (LAN) or WiFi network.
- Ways to connect the Access Network to the Edge Router:
  - DSL (telephone line), Cable (TV line), FTTH, 5G fixed wireless.

### Network Core
- From a very high level view point, every network is comprised of a transmitting end, receiving end, and some sort of link between the two.
- There are two approaches to moving data through a network of links and switches: **Circuit Switching** and **Packet Switching**.
#### Circuit Switching
<div align="center">
  <img src="https://cdn.comparitech.com/wp-content/uploads/2019/03/Circuit-Switching-1024x427.jpg" alt="Circuit Switching" width="80%" />
</div>

- In **Circuit Switching**, two networks nodes have to first establish a **_dedicated communications channel (circuit)_** going through the network in order to communicate.
  - The channel remains dedicated until the communication session (telephone call) ends.
    - i.e. the resources along the whole path (buffers, links, transmission rate) are reserved for the duration of the communication session between the End-Systems.
  - Number of Circuits = Number of Simultaneous Connections Possible
- Traditional telephone networks, which started off as a _landline network_ where nodes (communication endpoints) were directly connected via wire, were based on Circuit Switching.
#### Packet Switching
<div align="center">
  <img src="https://learningcontent.cisco.com/images/Fig3_SegmentRouting.png" alt="Packet Switching" width="80%" />
</div>

- In **Packet Switching**, two network nodes communicate by sending/receiving packets on an existing connection.
  - A **Packet** is a small chunk of the message being sent.
  - These Packets travel through communication Links and Packet Switches to reach the destination (without a dedicated circuit).
- The modern Internet is based on Packet Switching.
##### Packet Switch
- Intermediary nodes that forward packets toward their ultimate destinations.
- Job is to switch an incoming packet onto an outgoing link.
- Types
  - **Router**
    - Typically used in the Network Core.
  - **Link-layer Switch**
    - Typically used in Access Networks.
##### Store-and-Forward Transmission
- **The Packet Switch buffers (i.e. “stores”) the Packet’s bits until it receives all of the Packet’s bits. Then, it can start transmitting the first bit of the Packet onto the outbound Link. And simultaneously starts to receive the next Packet’s bits.**
- End-to-End Delay = N * (L/R)
  - N = number of links in the path.
  - L = size of a packet in bits.
  - R = transmission rate (bits/sec).
- Most Packet Switches use store-and-forward transmission at the inputs to the Links.
##### Queuing Delays and Packet Loss
- A Packet Switch has multiple Links attached to it.
- The Packet Switch has an **Output Buffer** (a.k.a. Output Queue) for each attached Link that stores packets that the router is about to send into that Link.
- If the particular Link is occupied with transmitting another Packet, the Packet must wait in the Output Buffer.
- If an arriving Packet encounters a full Output Buffer, there will be **Packet Loss**.
  - Either the arriving Packet or a Packet already in the queue will be dropped.
- **Output Buffer Queuing Delay**
  - Depends on network congestion.
  - Ex: if arrival rate (in Mbps) of packets to the Router exceeds the Link’s Mbps.
##### Forwarding Table
- **A table that maps destination IP addresses (included in a Packet’s header) to outbound Links to determine which outbound Link to forward the Packet onto.**
- The Internet has a number of special Routing Protocols that are used to automatically set Forwarding Tables.

## Types of Networks
### Local Area Network (LAN)
- Interconnect computers within a limited area (ex: home, school, office).
### Metropolitan Area Network (MAN)
- Interconnect computers within a geographic region the size of a metropolitan area (ex: city).
### Wide Area Network (WAN)
- Interconnect computers across the world (ex: the Internet).
- A network of networks.

## Terminologies
- **Routers**
  - A networking device that is used to interconnect LANs to form a WAN.
  - IP routers use IP addresses to determine where to forward packets.
  - Routing allows multiple networks to communicate independently and yet remain separate.
- **Bridges**
  - A networking device that creates a single, aggregate network from multiple communication networks or network segments.
  - Bridging connects two separate networks as if they were a single network.
  - c.f. Routers.

## Related Concepts
- **TCP/IP Protocol Suite**
  - The set of rules or algorithms which define the ways of how two entities can communicate across the network.
  - i.e., defines how data should travel across the Internet.
- **Domain Name System(DNS) Server**
  - Special servers that match up the web address(URL) that we type in our browsers to their actual IP addresses.
  - The browser looks up the domain name in the DNS to find the website's real IP address in order to retrieve the website.
- **Port**
  - A logical channel through which data can be sent/received to an application.
  - Any host may have multiple applications running, and each of these applications is identified using the port number on which they are running.
- **Host**
  - a.k.a. End-System.
  - Each device in the network is associated with a unique device name known as Hostname.
- **Socket**
  - The unique combination of IP address and Port number.
  - Commonly used for client and server interaction where the client connects to the server, exchange information, and then disconnect.
  - Flow
    - The server first establishes (binds) an address (socket/endpoint) that clients can use to find the server.
    - The socket on the server process waits for requests from a client.
      - The client-to-server data exchange takes place when a client connects to the server through a socket.
    - The server performs the client's request and sends the reply back to the client.
- **Content Delivery Network (CDN)**
  - A geographically distributed group of servers that work together to provide fast delivery of Internet content.
  - Requests are serviced from the infrastructure closest to the client making the request.

## Reference
[Basics of Computer Networking | GeeksforGeeks](https://www.geeksforgeeks.org/basics-computer-networking/)  
[What is Computer Networking? - AWS](https://aws.amazon.com/what-is/computer-networking/)  
[What is a WAN? Wide-Area Network - Cisco](https://www.cisco.com/c/en/us/products/switches/what-is-a-wan-wide-area-network.html#~types)  
[How sockets work - IBM](https://www.ibm.com/docs/en/i/7.3?topic=programming-how-sockets-work)  
