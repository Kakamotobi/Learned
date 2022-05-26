# Uniform Resource Locator(URL)
A URL is essentially an address for a particular resource on a particular server.

### URL Anatomy
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Network/refImg/URL-anatomy.png" alt="URL Anatomy" width="80%" />
</p>

- **Protocol/Scheme**
  - The protocol that the browser must use to request the resource.
  - Usually HTTP or HTTPS.
- **Authority**
  - **Domain Name**
    - The web server that is being requested.
    - Consists of the subdomain (if any), second-level domain, and top-level domain separated by periods.
    - Preceded by "://".
  - **Port**
    - The technical "gate" that is used to access the resources on the server.
    - HTTP standard port: 80.
    - HTTPS standard port: 443.
    - Preceded by a ":".
- **Path to Resource**
  - The path to a particular resource on the server.
  - Previously used to represent a physical file location on the server.
  - Nowadays, it is mostly an abstraction without any physical reality that is handled by the servers.
- **Parameters**
  - A list of key-value pairs separated with "&" representing extra information for the resource being requested.
  - Preceded by a "?".
- **Fragment**
  - An anchor to another part of the resource itself.
    - Ex: table of contents, timestamp.
  - The fragment portion of the URL is not sent to the server.
### URI, URL, URN
#### Uniform Resource Identifier(URI)
- Refers to nothing more than a string that identifies a particular resource.
  - No directions. Simply information that identifies a resource.
- URLs are a subset of URIs.
#### Uniform Resource Name(URN)
- A URN simply identifies a resource in a permanent way, even after the resource does not exist anymore.
  - No directions or information. Simply identifies.
- URNs are a subset of URIs.

## Reference
[What is a URL? - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL)  
[URL, URI, URN: What's the Difference?](https://auth0.com/blog/url-uri-urn-differences/)  
