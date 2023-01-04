# IP Address

## Table of Contents
- [What is an IP Address?](#what-is-an-ip-address)
- [Private IP Address](#private-ip-address)
- [Public IP Address](#public-ip-address)
- [Loopback IP Address (localhost)](#loopback-ip-address-localhost)

## What is an IP Address?
- An IP Address is a unique numerical label assigned to a device on a computer network that uses the Internet Protocol.

## Private IP Address
- **Private IP Addresses** are addresses used within a network, and are therefore, unreachable from outside the network (Ex: LAN).
- It is assigned to each device in the network by the network router.
- Private IP Addresses range:
  - 10.0.0.0 - 10.255.255.255
  - 172.16.0.0 - 172.31.255.255
  - 192.168.0.0 - 192.168.255.255
### How to Get your Private IP Address
- Private IP addresses can be found in the device.
- MacOS: `$ ifconfig`
- Linux: `$ ip -4 addr` or `$ ip route get 1.1.1.1 | awk '{print $7}'`

## Public IP Address
- **Public IP Addresses** are addresses that can be accessed directly over the Internet, and are therefore, used for connecting to any computer on the Internet.
- It is assigned to your network router by your ISP.
- Public IP Addresses range is every number that is not reserved for the Private IP range.
### How to Get your Public IP Address
- Public IP addresses can only be retrieved when the device reaches outside its network.
  - i.e. send a web request to an external system and get the Public IP address from the response.
- `$ curl https://ifconfig.me` or google "what is my ip".

## Loopback IP Address (localhost)
- A **Loopback IP Address** is an address that a device uses to refer to itself.
- It is used when a device both transmits and receives the data packets.
- The address 127.0.0.1 is known as localhost (they are internally mapped).
- Loopback IP Address range:
  - 127.0.0.0 - 127.255.255.255

## Reference
[Private vs. Public IP Addresses | Differences Explained | Avast](https://www.avast.com/c-ip-address-public-vs-private)  
[What is a Loopback Address? - GeeksforGeeks](https://www.geeksforgeeks.org/what-is-a-loopback-address/)  
