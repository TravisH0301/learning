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

## Methods of Table Partitioning
Tables can be partitioned using two methods. In AWS Redshift, the distribution style can be defined when creating a table. 

### Distribution Style
#### Round-Robin (Even)
Round-robin is used to distribute the table rows evening to the nodes.
- Pro: Achieves load-balancing (evenly distributed)
- Con: High cost of join with other table due to a lot of shuffling (affects parallel processing)

#### Broadcasting (All)
Small tables are broadcasted to the nodes (replicated on the entire table).
- Pro: Efficient join in parallel with other table due to no shuffling requirement
- Con: Can take up large space, hence, it is generally used for dimension tables (as they are smaller than fact tables)

#### Key
Rows with similar values are placed together among nodes (ex. rows with the same foreign key value).<br>
When creating a table, the column to be used for key distribution has to be defined.
- Pro: Partitioned tables on the nodes are joined in parallel with no shuffling as primary key and foreign key of the table rows are already matching
- Con: Load distribution can be skewed if data is skewed

### Sorting Key
A column or columns of a table can be defined as sorting key. When data is loaded, the rows are ordered by the sorting key prior to the distribution. <br>
The sorting key works like an index in the traditional database. It allows the nodes to scan the data faster. 
- Efficient query can be done if the column used the most frequent for sorting is defined as a sorting key
- Queries can become inefficient if the query is irrelevant to the sorting key column
