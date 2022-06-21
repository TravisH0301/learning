# HTTP
The Hyper Text Transfer Protocol (HTTP) is the TCP/IP based application layer protocol, which standardises how the client and 
server communicate with each other. It defines how the content is requested and transmitted across the internet.<br>
Typical flow of HTTP would involve a client making a request to a server, which then sends a response.

- [Stateless and connection](#stateless-and-connection)
- [HTTP request](#http-request)
- [HTTP response](#http-response)
- [HTTP/2](#http2)
- [HTTP/3](#http3)

## Stateless and connection
HTTP is a stateless\* protocol, where there is no link between subsequent requests on the same connection. However, HTTP cookies can 
allow stateful sessions.<br>
\**Stateless protocol: Server doesn't save session information. There is no tight dependency between server and client.*<br>
\**Stateful protocol: Server remembers state and session information. There is tight dependency between server and client.*

HTTP is sent over Transport Control Protocol (TCP) (or Transport Layer Security (TLS)), and the connection is managed by TCP. 
TCP connection is established before HTTP requests and responses are exchanged between the client and the server.

Upto HTTP/1.0, the TCP connection was established and closed as per a request. But from HTTP/1.1, persistent connection was 
introduced to keep the connection alive to allow multiple sequential requests. The last request would contain the
header *connection:close* to close the connection

## HTTP request
The client communicates with the host by firstly sending a HTTP request. A HTTP request consists of the following:
- <b>HTTP version type</b>
- <b>URL</b>
- <b>HTTP method</b>: is the action that HTTP request expects. It's also known as HTTP verb. There are the following methods:
  - <b>GET</b>: requests a representation of the specified resource. Requests using GET should only retrieve data
  - <b>HEAD</b>: asks for a response identical to a GET request, but without the response body
  - <b>POST</b>: submits an entity to the specified resource, often causing a change in state or side effects on the server
  - <b>PUT</b>: replaces all current representations of the target resource with the request payload
  - <b>DELETE</b>: deletes the specified resource
  - <b>CONNECT</b>: establishes a tunnel to the server identified by the target resource
  - <b>OPTIONS</b>: describes the communication options for the target resource
  - <b>TRACE</b>: performs a message loop-back test along the path to the target resource
  - <b>PATCH</b>: applies partial modifications to a resource
- <b>HTTP request header</b>: contains core information in text in key-value pairs. (ex. browser information, HTTP method & encoding)
- <b>Optional HTTP body</b>: contains information that HTTP request is transferring. (ex. username and password for POST method)

## HTTP response
HTTP response is what the client receives from the server as the response to the request made. A HTTP response consists of the following:
- <b>HTTP status code</b>: is a 3-digit code that displays the outcome status of the request. Status code can break down to 5 different blocks (xx ranges from 00 to 99):
  - 1xx Informational
  - 2xx Success
  - 3xx Redirection
  - 4xx Client Error
  - 5xx Server Error
- <b>HTTP response headers</b>: contains important information such as encoding, content type, date & status code
- <b>Optional HTTP body</b>: contains requested information (ex. for GET request method)

## HTTP/2
In 2015, Google created HTTP/2 to low latency transport of content. The following features were added on top of HTTP/1.1:
- <b>Binary protocol</b>: HTTP/2 contents are in binary and the major blocks of HTTP/2 are frames and streams.
  - <b>Frame</b>: is binary piece of data containing HTTP parts like headers, data, setting & etc.
  - <b>Stream</b>: is a collection of frames. Each frame has stream id to identify which stream it belongs to, and shares the common headers among other frames within the same stream. Both client and server assigns stream ids.
- <b>Multiplexing</b>: In a single connection, a client sends all the streams asynchronously without opening additional connection. A server responses asynchronously too with order. The client uses the assigned stream id to identify the stream to which a specific packet belongs.
- <b>HPACK header compression</b>: header compression is done to reduce the redundant header information. Huffman coding is used for this compression.
- <b>Server push</b>: is when a server, knowing that a client is going to ask for a certain resource, can push it to the client before client asking for it. The server sends a frame called *PUSH_PROMISE* to notify the client about the resource that the server is going to send, so that the client won't ask for it. The server then pushes the resource with the same stream id. This decreases the roundtrip of the data.
- <b>Request priortisation</b>: A client can send a frame, *PRIORITY* to set processing priority for a server. Otherwise, the server processes the requests asynchronously.
- <b>Security</b>: The use of Transport Layer Security (TLS) is not mandatory in HTTP/2. However, most vendors only support HTTP/2 used over TLS.

## HTTP/3
Internet Engineering Task Force (IETF)'s HTTP and QUIC Working Groups developed QUIC to improve performance of the existing TCP. In doing so, HTTP/3 was developed to be make HTTP compatible with QUIC.

QUIC is a generic transport protocol, that runs on top of User Datagram Protocol (UDP)\*. Although UDP is fast, it is less reliable than TCP. Therefore, QUIC provides almost all features of TCP to make the reliable connection by using acknowledgements for received packets and retransmissions to make sure lost ones still arrive. QUIC also still sets up a connection and has a highly complex handshake.<br>
\**UDP is a simpler and efficient but less reliable (due to no gurantee of data transfer and no handshake connection) protocol in comparison to TCP. It is thus usually used for live traffic process, where missing data is accpectable as outdated data is not required.*

These are the key features of QUIC:
- <b>Integration with TLS</b>: Instead of using TLS on top of TCP, QUIC encapsulates TLS to make QUIC fully encrypted. Additionally, when establishing a handshake connection, QUIC combines data transport and cryptographic handshake into one, saving a round trip compared to TCP + TLS.
- <b>Supporting multiple independent byte streams</b>: In HTTP/2 and HTTP/3, multiplexing is supported, where multiple files can be broken into byte streams and sent over a single connection asynchrously. However with TCP, it interprets all byte streams as a single file. Thus, when a packet is lost and retransimission occurs, TCP holds on to all of the byte streams until the lost packet is recovered. This causes "head-of-line (HOL) blocking". QUIC resolves this issue by identifying individual streams, and recover any lost packet for individual streams. QUIC overwrites HTTP stream logic to use its advanced stream logic.
- <b>Use of connection ID</b>: QUIC uses "connection identifier (CID)" in the packet header to continue using the same connection even IP addresses or port numbers at either of client or server changes. (ex. change of network from 4G to WiFi). Hence, if IP address or port number changes, the client and server can keeping using the same old connection, and this is called "connection migration". QUIC uses a pool of CIDs which map to the same connection to switch CID for a new network to protect privacy.
- <b>Use of frames</b>








