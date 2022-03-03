# Computer Network

## Table of Contents
- [What is Computer Networking?](#what-is-computer-networking)
- [Types of Computer Network Architecture](#types-of-computer-network-architecture)
- [Types of Networks](#types-of-networks)
- [Networking Devices](#networking-devices)
- [Related Concepts](#related-concepts)
- [The Internet vs The Web](#the-internet-vs-the-web)

## What is Computer Networking?
> An interconnection of multiple devices, also known as hosts, that are connected using multiple paths for the purpose of sending/receiving data or media. Computer networks can also include multiple devices/mediums which help in the communication between two different devices; these are known as **Network devices** and include things such as routers, switches, hubs, and bridges. | GeeksforGeeks

> Computer networking refers to interconnected computing devices that can exchange data and share resources with each other. These networked devices use a system of rules, called communications protocols, to transmit information over physical or wireless technologies. | AWS

- Computer networks are essentially made up of nodes and links. 
  - A network node may be some sort of data communication equipment (ex: modem, hub, switch, or data terminal equipment such as two or more computers and printers).
  - A network link is the transmission media between two nodes (ex: physical hardware such as cable wires, optical fibers, or wireless).

- Open System: a system that is connected to the network and is ready for communication.
- Closed System: a system that is not connected to the network and hence, cannot be communicated with.

## Types of Computer Network Architecture
### Client-Server Architecture
- Network nodes may be either clients or servers.
- Servers provide resources like memory, processing power, or data to clients.
- Clients may communicate with each other but do not share resources.
### Peer-to-Peer (P2P) Architecture
- Computers have equal powers and priviledges.
- Each device in the network can act as either a client or server.
- Each peer may share some of its resources (ex: memory, processing power) with the entire network.
  - Ex: some companies use P2P architecture to host memory-consuming applications.

## Types of Networks
### Local Area Network (LAN)
- Interconnect computers within a limited area (ex: home, school, office).
### Metropolitan Area Network (MAN)
- Interconnect computers within a geographic region the size of a metropolitan area (ex: city).
### Wide Area Network (WAN)
- Interconnect computers across the world (ex: the Internet).
- A network of networks.

## Networking Devices
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

## Related Concepts
- **TCP/IP Protocol Suite**
  - The set of rules or algorithms which define the ways of how two entities can communicate across the network.
  - i.e., defines how data should travel across the Internet.
- **Domain Name System(DNS) Server**
  - Special servers that match up the web address(URL) that we type in our browsers to their actual IP addresses.
  - The browser looks up the domain name in the DNS to find the website's real IP address in order to retrieve the website.
- **Packet**
  - Format in which data is sent from the server ot the client.
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

## Reference
[Basics of Computer Networking | GeeksforGeeks](https://www.geeksforgeeks.org/basics-computer-networking/)  
[What is Computer Networking? - AWS](https://aws.amazon.com/what-is/computer-networking/)  
[What is a WAN? Wide-Area Network - Cisco](https://www.cisco.com/c/en/us/products/switches/what-is-a-wan-wide-area-network.html#~types)  
[How sockets work - IBM](https://www.ibm.com/docs/en/i/7.3?topic=programming-how-sockets-work)  
