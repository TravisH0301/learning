# Distributed Database
Database is distributed horizontally with copies of data to provide high availability.<br>
Typically NoSQL databases are distributed database system, allowing:
- Massive dataset organisation: horizonal scaling is supported to allow data growth
- Flexible database expansion: newly formed data doesn't have to fit the data types of previously existing information (flexible schema)
- Multiple data structures: different data types can be collated together
- High availability
- High throughput with low latency
- Linear scalability

On the other hand, there are limitations:
- Since it has flexible schema and there is no relational tables, adhoc queries such as join or aggregate cannot be done.
  - This is because for example, in join, common colums with the same data type are required between tables related to each other. But in non-relational database, there is no relations between tables. But, it maybe done in some circumstances like joining the keys for two tables that are in the same field, or table can be newly created to meet the requirement.
  - Also join/aggregation requires scanning of whole data and it's allowed as the data is spread across multiple nodes. It is not done without optimisations. Hence, it's usually done in the data processing app, such as, Apache Spark.
- No ACID transactions, but some NoSQL DB supports ACID transactions.

## Eventual Consistency
A consistency model in distributed computing to achieve high availability that 
informally guarantees the same data item in different nodes (machines), if, no new
updates are made to the data item.<br>
If a new change is made, the data item may not be consistent in different locations.

If all nodes get constantly updated, this may not be a big issue. 

## The CAP Theorem
A theorem in computer science that states it is impossible for a distributed data
store to simultaneoulsy provide more than 2 of the following three guarantees:
- Consistency: every read returns the latest and correct data or an error
- Availability: response is given to every requests without consistency guarantee
- Partition tolerance: system continues to work regardless of losing network
connectivity between nodes

Since there the distributed database always need to tolerate network issues, 
one can only have either Consistency & Partition tolerance (CP) or Availability &
Partition tolerance (AP).

## Distributed Database Modelling (Apache Cassandra)
Unlike relational database, distributed database (or NoSQL database) does not allow JOINs due to its distributed structure. 
Hence, a denormalised table has to be created based on the query's purpose.<br>
For example, one needs to consider the followings for the NoSQL database, Apache Cassandra:
- Apache Cassandra does not allow JOINs
  - In addition, GROUP BY & subqueries are not supported in Cassandra Query Language (CQL).
- Denormalised tables must be used
  - In relational database, denormalisation slows down write performance due to data redundancy. However, Apache Cassadra is optimised for denormalisation to allow fast writes.
- Due to denormalisation requirement, queries need to be decided before creating a table
- One table per query is a great strategy
  - This will result in data duplication and thus higher storage cost. But, Cassandra outweighs this issue with high performance and high availability. 


