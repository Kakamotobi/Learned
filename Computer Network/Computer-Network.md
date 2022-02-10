# Computer Network

## Table of Contents
- [What is Computer Networking?](#what-is-computer-networking)
- [Types of Networks](#types-of-networks)
- [Networking Devices](#networking-devices)
- [Related Concepts](#related-concepts)

## What is Computer Networking?
> An interconnection of multiple devices, also known as hosts, that are connected using multiple paths for the purpose of sending/receiving data or media. Computer networks can also include multiple devices/mediums which help in the communication between two different devices; these are known as **Network devices** and include things such as routers, switches, hubs, and bridges. | GeeksforGeeks
- Open System: a system that is connected to the network and is ready for communication.
- Closed System: a system that is not connected to the network and hence, cannot be communicated with.

## Types of Networks
- Local Area Network (LAN)
  - Interconnect computers within a limited area (ex: home, school, office).
- Metropolitan Area Network (MAN)
  - Interconnect computers within a geographic region the size of a metropolitan area (ex: city).
- Wide Area Network (WAN)
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

## Reference
[Basics of Computer Networking | GeeksforGeeks](https://www.geeksforgeeks.org/basics-computer-networking/)  
[What is a WAN? Wide-Area Network - Cisco](https://www.cisco.com/c/en/us/products/switches/what-is-a-wan-wide-area-network.html#~types)  
[How sockets work - IBM](https://www.ibm.com/docs/en/i/7.3?topic=programming-how-sockets-work)  