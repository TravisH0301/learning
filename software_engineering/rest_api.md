# REST API
REST (REpresentational State Transfer) API (Application Programming Interfaces) is a set of architectural constraints, not a protocol or a standard.
It allows computer systems to communicate and transmit data. The information is delivered in one of several formats via
HTTP: JSON (Javascript Object Notation), HTML, XLT, Python, PHP, or plain text. And HTTP: JSON is the most generally popular file format.

![image](https://user-images.githubusercontent.com/46085656/177357787-e70ef31f-5135-45ee-87b0-073d548e7ba6.png)

## Stateless constraints
The communication between the client and the server is stateless. The server doesn't need to know about the state the client is in.
The request from the client must contain all of the information neccessary to understand the request. Session state is therefore kept entirely on the client.

This provides:
- Quicker performance: no need to look back on previous requests
- Reliability: easier to recover from partial failure
- Scalability: no state storage and simplifies implementation with no need to manage resources across requests

However, this results in the repetitive data sent in multiple requests. And this reduces the server's control over consistent application behaviour, since
the application depends on the correct implementation of the client.

## Cache constraints
Cache constraints require the data within a response to be implicitly or explicitly labeled as cacheable or non-cacheable. If the data is cacheable, 
the client can reuse this cached data. This helps to reduce the interactions between the client and the server, improving efficiency and scalability. 

However, as a trade-off, the reliability can be impacted if the cache contains the stale data that differs from the data that response would deliver.

## Making requests
REST requires that a client make a request to the server in order to retrieve or modify data on the server. A request generally consists of:
- HTTP verb, which defines what kind of operation to perform
- Header, which allows the client to pass along information about the request
- Path to a resource
- Optional message body containing data

### HTTP Verbs
There are 4 common HTTP verbs that fullfill CRUD (Create, Read, Update, Delete) operations:
- GET: retrieves a specific resource or a collection of resources
- POST: create a new resource
- PUT: update a specific resource
- DELETE: remove a specific resource
