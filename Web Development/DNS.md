# Domain Name System(DNS)

## Table of Contents
- [What is DNS?](#what-is-dns)
  - [What is a Domain?](#what-is-a-domain)
  - [What is a Domain Name?](#what-is-a-domain-name)
- [DNS Hierarchy](#dns-hierarchy)

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

## Reference
[Novell Documentation: DNS/DHCP - DNS Hierarchy](https://www.novell.com/documentation/dns_dhcp/?page=/documentation/dns_dhcp/dhcp_enu/data/behdbhhj.html)  
[Root name server - Wikipedia](https://en.wikipedia.org/wiki/Root_name_server)  
