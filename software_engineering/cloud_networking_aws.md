# Cloud Networking (AWS)
## VPC
<img src="https://github.com/TravisH0301/learning/blob/master/images/vpc_network.png" width="800">

A Virtual Private Cloud (VPC) is a service that enables launching resources in a logically isolated virtual network. VPC provides complete control over the perimeter of the virtual networking environment, including selection of IP address range, creation of subnets, and confiugration of route tables and network gateways.

## Gateway VPC endpoint
Gateway VPC endpoints determine what kind of traffic from internet can access the VPC.

- Internet gateway: allows public internet traffic
- Virtual private gateway: allows protected internet traffic 
- AWS direct connect: provides a dedicated private connection

## Subnet
A subnet is a section of a VPC that groups resources within the VPC based on security or operational needs.

- Public subnet: resources that need to be accessed by public
- Private subnet: resources that should be accessed by only private network

Different types of subnet connects to different gateway VPC endpoints based on their security level. And within VPC, subnets can communicate with each other regardless of public subnet or private subnet.

### ACL
The Access Control List (ACL) allows or denies specific inbound or outbound traffic at the subnet level. By default, ACLs allow all inbound and outbound traffic. 

Network ACLs perform `stateless` traffic filtering - they remember nothing and checks the traffic inbound and outbound. (e.g., returning packet, that was allowed to leave subnet, getting checked again)

## Security Group
A security group is like a virtual firewall that controls inbound and outbound traffic for AWS resources. By default, it denies all inbound traffic and allows all outbound traffic.

Security groups perform `stateful` traffic filtering - they remember previous decisions made for incoming traffic, and allows the same traffic to exit.

## DNS
Domain Name Service (DNS) is a phonebook of the internet that allows humans to access websites with domain names. Amazon Route 53 enables user requests to internet applications running on AWS or on-premises.

<img src="https://github.com/TravisH0301/learning/blob/master/images/dns.png" width="600">
