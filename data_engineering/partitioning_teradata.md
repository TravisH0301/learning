# Parititioning in Teradata
Teradata is a relational database management system (RDBMS) based on MPP (Masively Parallel Processing) architecture. 
The nodes, or also known as AMPs (Access Module Processors) share nothing between each other to achieve parallel processing.

![image](https://user-images.githubusercontent.com/46085656/184534108-6e96dddb-6c34-454a-8a1a-f8c3058c2a2c.png)

## Primary Index
Primary Index (PI), or Distribution Key (DK) in other MPP systems, is the important aspect of Teradata to maximise performance.
 
Primary index works at a table level and it turns a column (or columns) into a key for each row of data. 
In a MPP system, the data is distributed to individual nodes based on the primary index. 

When the primary index is unique, the rows are distributed in a 'round-robin' manner to all nodes. However, when it's duplicated, the data
is grouped into selective nodes causing "skewness" of data, and the parallelism is not fully utilised.

TBC
