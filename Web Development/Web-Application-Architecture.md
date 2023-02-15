# Web Application Architecture

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/web-application-architecture.png" alt="Web Application Architecture" width="80%" />
</p>

- **Client**
  - The Client (browser) receives a gzip file from the Web Server.
  - It checks the `Content-encoding` header to check the specified compression algorithm and decompresses the gzip containing HTML.
  - It parses through HTML, send requests for resources mentioned in the HTML, and renders the web page onto the screen.
- **Proxy Server**
  - A Proxy Server is an intermediary server between the client and a server that receives and sends the request on behalf of the client.
  - Uses:
    - Cache frequently requested resources (Ex: static resources such as images).
    - Load balancing.
    - Security.
    - Anonymity.
- **Web Server**
  - A Web Server is responsible for delivering static compressed web content (Ex: HTML, CSS, images, videos, files, cache, etc.).
  - When requesting for these resources, the request ends here.
  - Ex: Apache, Nginx.
- **Web Application Server(WAS)**
  - A WAS is responsible for providing interaction between clients and server-side application code (Ex: business logic, DB read/write).
  - Ex: Apache Tomcat, Oracle WebLogic.

## Reference
[Web Server vs. Application Server | IBM](https://www.ibm.com/topics/web-server-application-server)  
