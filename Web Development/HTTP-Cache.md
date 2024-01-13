# HTTP Cache

## Table of Contents
- [What is HTTP Cache?](#what-is-http-cache)
- [Types of HTTP Cache](#types-of-http-cache)
  - [Private Cache](#private-cache)
  - [Shared Cache](#shared-cache)
    - [Proxy Cache](#proxy-cache)
    - [Managed Cache](#managed-cache)
  - [Illustration](#illustration)
- [Reference](#reference)

## What is HTTP Cache?
- Stores a response associated with a request and reuses the stored response for subsequent requests.

## Types of HTTP Cache
### Private Cache
- A cache that is tied to a specific Client (Ex: browser cache).
- Can store personalized response for the user.
- `Cache-Control: private`
### Shared Cache
- A cache that is located between the Client and the Server.
- Can store responses that can be shared among users.
- Subclasses of Shared Cache: Proxy Cache and Managed Cache
#### Proxy Cache
  - Some proxies implement caching to reduce traffic out of the network.
  - Over HTTPS, proxy caches in the network path can only tunnel a response and cannot behave as a cache.
#### Managed Cache
  - Caches that are deployed by us to offload the origin server and to deliver content efficiently (Ex: CDN).
### Illustration
<div align="center">
  <img src="https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching/type-of-cache.png" alt="Types of Cache" width="80%" />
  <p><i>MDN</i></p>
</div>

## Reference
[HTTP caching - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)  
