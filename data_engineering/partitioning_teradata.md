# Parititioning in Teradata
Teradata is a relational database management system (RDBMS) based on MPP (Masively Parallel Processing) architecture. 
In Teradata, each node can contain multiple Access Module Processors (AMPs) that share nothing between each other to achieve parallel processing.

![image](https://user-images.githubusercontent.com/46085656/184534108-6e96dddb-6c34-454a-8a1a-f8c3058c2a2c.png)

## Primary Index
Primary Index (PI), or Distribution Key (DK) in other MPP systems, is the important aspect of Teradata to maximise performance.
 
Primary index works at a table level and it turns a column (or columns) into a key for each row of data. 
In a MPP system, the data is distributed to individual nodes based on the primary index. 

When the primary index is unique, the rows are distributed in a 'round-robin' manner to all nodes. However, when it's duplicated, the data
is grouped into selective nodes causing "skewness" of data. The node with high load is called "hot spot", and it becomes a bottle to the parallism.

The uniqueness of the primary index can be governed by using Unique Primary Index (UPI) in Teradata.

### Selection of Primary Index
When selecting a primary index, a primary key of the table is a good choice, given it is a unique column providing even distribution of the data throughout AMPs.

If no unique column exists, a combination of columns can be used instead to create an unique primary key

## Partitioning by Primary Index
In Teradata, partitioning is done by hash of key using a hash function. When a row is inserted, the key of the row will be hashed. And the row value will be allocated by the hash bucket to the AMP, where its hash is covered by the AMP's range of hash codes.

![image](https://user-images.githubusercontent.com/46085656/185113418-d9ec5871-1fcc-4521-a37f-bb05776086b0.png)








