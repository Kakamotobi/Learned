# Domain Name System(DNS)

## Table of Contents
- [What is DNS?](#what-is-dns)
  - [What is a Domain?](#what-is-a-domain)
  - [What is a Domain Name?](#what-is-a-domain-name)
- [DNS Hierarchy](#dns-hierarchy)
- [DNS Query](#dns-query)

## What is DNS?
- **The DNS is a hierarchical naming system for computers, or any resource participating on the Internet.**
- The DNS' role is to map/convert domain names to IP addresses.
### What is a Domain?
- **Domain is an area of control or a sphere of knowledge, whether narrowly or broadly defined, identified by a name.**
### What is a Domain Name?
- A domain name represents an entity's position within the structure of the DNS hierarchy.
- A domain name is essentially all the domains in the path from the local domain to the root, separated by periods.

## DNS Hierarchy
- Each node on the DNS tree represents a domain.
  - This means that any domain in a subtree is part of all the domains that are above it.
- Therefore, "google.com" means that the subdomain "google" is under the ".com" top-level domain.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/DNS-hierarchy.png" alt="DNS Hierarchy" width="80%" />
</p>

- **Root Domain**
  - An empty string, which all Fully Qualified Domain Names(FQDNs) end with.
    - Ex: "www.google.com" is "www.google.com." where there is an empty string after the last period.
  - However, this is not required to be explicit and is usually not included.
- **Top-Level Domains**
  - These are domains categorized into types of organizations that use them.
  - Ex: "com" for commercial organizations, "edu" for educational organizations.
- **Second-Level Domains**
  - These are individual organizations.
  - Ex: "google", "skku".
- **Subdomains**
  - The parent domain can be further divided into subdomains.
  - Ex: "www", "mail", "docs", "drive", etc for google.

## DNS Query
- A DNS lookup/query is a request sent by a client to the DNS servers to find the corresponding IP address of the specified domain name.
### DNS Query Process
- The user enters the page/resource in the browser's URL bar.
- The browser first checks its cache, then the OS cache, then the Router cache, then the Local DNS Server (ISP) cache.
- ***If at this point, the IP address has not been found yet, the DNS query process starts.***
- The IP address is looked for starting from the Root DNS server, to the TLD DNS server, and so on until the IP address if found on the Authoritative DNS server.
### Types of Queries
- The DNS server uses both iterative and recursive query to be efficient.
#### Iterative
- The DNS client contacts the name servers one by one until the IP address is found.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/DNS-query-iterative.png" alt="Iterative DNS Query" width="80%" />
</p>

#### Recursive
- The DNS client receives the IP address from the first DNS server, which will go on querying other DNS servers if needed.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/DNS-query-recursive.png" alt="Recursive DNS Query" width="80%" />
</p>

### Mitigating Bottleneck
- The entire domain maps to a single IP address, which may cause bottleneck.
#### Round-robin DNS
- Multiple IP addresses for the same domain is returned.
#### Load-balancer
- Hardware that listens on a specific IP address and forwards requests to other servers.
#### Geographic DNS
- Domain names are mapped to different IP addresses based on the client's geographic location.
#### Anycast
- A routing technique where a single IP address maps to multiple physical servers.

## Reference
[Novell Documentation: DNS/DHCP - DNS Hierarchy](https://www.novell.com/documentation/dns_dhcp/?page=/documentation/dns_dhcp/dhcp_enu/data/behdbhhj.html)  
[Root name server - Wikipedia](https://en.wikipedia.org/wiki/Root_name_server)  
[ClouDNS: What is a DNS query?](https://www.cloudns.net/wiki/article/254/#:~:text=A%20DNS%20query%20(also%20known,associated%20with%20a%20domain%20name.))  
[vasanthk/how-web-works: What happens behind the scenes when we type www.google.com in a browser?](https://github.com/vasanthk/how-web-works)  
