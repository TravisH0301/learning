# Internet

## What is internet and how does it work?
Internet is a decentralised network of networks. Computers share and access information through the backbone of networks,
and data centres store data and host applications and contents.

## IP address
Internet Protocol (IP) address is a number that identifies a computer. Two computers cannot have the same IP address.<br>
IPv6 is the current standard for IP address that forms the unique structure of IP address.

## Packet
Packet is the unit of information transmitted over internet.<br>
It is consisted of 2 parts.
- Header: contains information of source, destination, length and etc.
- Data: contains actual data

## Protocols
Protocols are the ways the communication is made on internet.<br>
Protocols on a computer are consisted of the hierarchical stacks:
1. Application protocols
2. Transmission Control Protocol (TCP)
3. Internet protocol (IP)
4. Hardware

When an application protocol such as an email sends a message, packets are created containing the message information. Packets go through TCP layer
to get TCP header containing source and destination port numbers. The packets go through IP layer to get IP header containing source and 
destination IP addresses. Lastly, hardware converts the packets into electronic signals before sending it off to the network.<br>
When the packets travel through the network backbone, it gets to the router, which will send the packets to the next router, where the destination
IP address exists. The packets go through a number of routers to finally reach to the destination computer.<br>
Then the packets are converted back to alphabetic message information followed by IP layer and TCP layer, where IP and TCP headers are stripped.
And finally the packets reach the destination application protocol for the message to arrive.

Here are some of most widely used protocols:
- Hypertext Transfer Protocol (HTTP): is an application protocol for World Wide Web (WWW). It is a connectionless text based protocol, where the
connection between the client (web browser) and host (web server) is only established and maintained while the HTTP request is being serviced.
- Simple Mail Transfer Protocol (SMTP): is an application protocol for email. SMTP is a connection orientated. The connection between the
mail client and mail server will be established when the first connection is made and be maintained until the client disconnects.
- Transmission Control Protocol (TCP): is responsible for correctly connecting the source application protocol to the destination application protocol.
To achieve this, port numbers are used. Different port numbers for different application protocols allow a computer to send and receive packets 
with different destination computers/hosts.<br>
TCP is connection-orientated and will establish a connection between two application protocols.
- Internet Protocol (IP): is a connectionless protocol. It is responsible for sending out the packets with IP header and allow packets to travel
through routers to reach the destination. As routers cannot gurantee that the destination IP address can be found, the packets can get lost and
be never get delivered. Hence, IP is an unreliable protocol, unlike TCP, where packets are ensured to be sent to the correct application protocols.

## World Wide Web
World Wide Web (WWW) is the application that works on top of internet to provide user-friendly interface,
while supporting hyperlinks, images, videos and many other contents.<br>
And web browsers are used to fetch and retrieve information from web sites to consume contents.

## SSL
Secure Sockets Layer (SSL) is an encrytion technology used on web sites to encrypting transitting data,
hence, only the recipient only knows how to decipher the encryption. 

## DNS
Domain Name System(DNS) is a naming system that translates domain name to IP address.<br>
This system is hierarchical.
1. 1st-level main domain: .com
2. 2nd-level sub domain: google.com
3. 3rd-level sub domain: support.google.com

The top-level domain names can be adminstered by private companies (ex. .com) or government authorities (ex. gov.au).
