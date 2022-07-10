# REST API
REST (REpresentational State Transfer) API (Application Programming Interfaces) is a set of architectural constraints, not a protocol or a standard.
It allows computer systems to communicate and transmit data. The information is delivered in one of several formats via
HTTP: JSON (Javascript Object Notation), HTML, XLT, Python, PHP, or plain text. And HTTP: JSON is the most generally popular file format.

The offical format of the JSON can be found at [Official JSON:API](https://jsonapi.org/).

![image](https://user-images.githubusercontent.com/46085656/177357787-e70ef31f-5135-45ee-87b0-073d548e7ba6.png)

- [Stateless constraints](#stateless-constraints)
- [Cache constraints](#cache-constraints)
- [Making requests](#making-requests)
  - [HTTP Verbs](#http-verbs)
  - [Headers](#headers)
    - [Paths](#paths)
    - [Accept parameter](#accept-parameter)
- [Sending responses](#sending-responses)
  - [Content-type parameter](#content-type-parameter)
  - [Response status codes](#response-status-codes)
- [Layered system](#layered-system)
- [Code-on-demand](#code-on-demand)

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

### Headers
#### Paths
Requests must contain a path to a resource that the operation should be performed on. Paths must be constructed in a way that the client can
understand the hierarchy. Conventionally, plural words are used (ex. ids, customers).

    GET fashionboutique.com/customers/223/orders/12
    
#### Accept parameter
In the header of the request, the client sends the type of content that it is able to receive from the server. This is called the Accept field.
MIME types (Multipurpose Internet Mail Extensions) are used to define the content types, consisting of "types" and "sub types".

The below example header says it can accept text/html & application/xhtml contents.

    GET /articles/23
    Accept: text/html, application/xhtml
    
## Sending responses
### Content-type parameter
When the server sends a data payload to the client, the content-type must be defined using MIME types. And this should be one of the types
defined in the respective request's accept parameter.

    Request:
    GET /articles/23 HTTP/1.1
    Accept: text/html, application/xhtml
    
    Response:
    HTTP/1.1 200 (OK)
    Content-Type: text/html
    
### Response status codes
Responses from the server contain status codes to alert the client to information about the success of the operation.

    Example response status codes
    GET — return 200 (OK)
    POST — return 201 (CREATED)
    PUT — return 200 (OK)
    DELETE — return 204 (NO CONTENT)

- 200 (OK): This is the standard response for successful HTTP requests.
- 201 (CREATED): This is the standard response for an HTTP request that resulted in an item being successfully created.
- 204 (NO CONTENT): This is the standard response for successful HTTP requests, where nothing is being returned in the response body.
- 400 (BAD REQUEST): The request cannot be processed because of bad request syntax, excessive size, or another client error.
- 403 (FORBIDDEN): The client does not have permission to access this resource.
- 404 (NOT FOUND): The resource could not be found at this time. It is possible it was deleted, or does not exist yet.
- 500 (INTERNAL SERVER ERROR): The generic answer for an unexpected failure if there is no more specific information available.

## Layered system
The layers of the system (ex. load balancer) involved in the retrieval of the requested information are invisible to the client.
This allows the REST API to improve functionalities and scalability with the independence to the client. However, layered system can add overhead to the
system and reduce the user-perceived performance.

## Code-on-demand
Optionally, the server can send an executable code to the client when requested, extending client functionality. 
This allows the client to reduce the number of features to pre-implement by downloading features from the server. However, this will limit the visibility 
of the system.
