# Massively Parallel Processing (MPP) Database
An MPP database is a storage structure designed to process multiple operations simultaneously using several processing units in a distributed computer network.
This allows MPP databases to provide much fast query performance on large datasets.

## MPP Architecture
MPP architecture consists of multiple computers (nodes) in the distributed networks working together. And there are two types of MPP architecture.

### Grid Computing
- Nodes are connected through internet
- Nodes can have different OS and hardware (heterogenous)
- Each node manages its resource
- Subtasks are given to the nodes to work autonomously

### Clustering
- Nodes are connected through fast local area network
- Nodes have the same OS and hardware (homogenous)
- Resources are shared by the centralised resource manager among nodes
- Nodes perform the same task 

## Amazon Redshift
Amazon Redshift is one of the databases that uses MPP. It uses a cluster of nodes to process queries in parallel. 

![](https://github.com/TravisH0301/learning/blob/master/images/redshift_architecture.png)

To do this, the table is divided into partitioned tables and
each node slice (node CPU) performs the query on its own partitioned table. This process allows the database to execute queries efficiently in parallel. <br>
The number of partitioned tables depends on the number of node slices. And the cluster has a lead node which commnicates with the client applications. 
