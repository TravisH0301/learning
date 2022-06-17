# HTTP
The Hyper Text Transfer Protocol (HTTP) is the TCP/IP based application layer protocol, which standardises how the client and 
server communicate with each other. It defines how the content is requested and transmitted across the internet.<br>
Typical flow of HTTP would involve a client making a request to a server, which then sends a response.

## Stateless and connection
HTTP is a stateless\* protocol, where there is no link between subsequent requests on the same connection. However, HTTP cookies can 
allow stateful sessions.<br>
\**Stateless protocol: Server doesn't save session information. There is no tight dependency between server and client.*<br>
\**Stateful protocol: Server remembers state and session information. There is tight dependency between server and client.*

HTTP is sent over Transport Control Protocol (TCP) (or Transport Layer Security (TLS)), and the connection is managed by TCP. 
TCP connection is established before HTTP requests and responses are exchanged between the client and the server.

## HTTP request
The client communicates with the host by firstly sending a HTTP request. A HTTP request consists of the following:
- HTTP version type
- URL
- HTTP method: is the action that HTTP request expects. It's also known as HTTP verb. There are the following methods:
  - GET: requests a representation of the specified resource. Requests using GET should only retrieve data
  - HEAD: asks for a response identical to a GET request, but without the response body
  - POST: submits an entity to the specified resource, often causing a change in state or side effects on the server
  - PUT: replaces all current representations of the target resource with the request payload
  - DELETE: deletes the specified resource
  - CONNECT: establishes a tunnel to the server identified by the target resource
  - OPTIONS: describes the communication options for the target resource
  - TRACE: performs a message loop-back test along the path to the target resource
  - PATCH: applies partial modifications to a resource
- HTTP request header: contains core information in text in key-value pairs. (ex. browser information, HTTP method & encoding)
- Optional HTTP body: contains information that HTTP request is transferring. (ex. username and password for POST method)

## HTTP response
HTTP response is what the client receives from the server as the response to the request made. A HTTP response consists of the following:
- HTTP status code: is a 3-digit code that displays the outcome status of the request. Status code can break down to 5 different blocks (xx ranges from 00 to 99):
  - 1xx Informational
  - 2xx Success
  - 3xx Redirection
  - 4xx Client Error
  - 5xx Server Error
- HTTP response headers: contains important information such as encoding, content type, date & status code
- Optional HTTP body: contains requested information (ex. for GET request method)

## HTTP/2


