# Data Modelling (Database modelling)
An abstrat that organises elements of data and they relate to each other. End state is a database.

## Process
- Gather requirements
- Build data model using Entity Relationship (ER) Model or Unified Modelling Language (Typically Entity Relationship Diagram is used)
  - Conceptual data modelling: establishes entities, attributes and relationships. (no cardinality)
  - Logical data modelling: defines column information (type, length, etc) and cadinality of relationships. 
  - Physical data modelling: described database-specific implementation of the data model. Primary & foreign keys, view, indexes and authorisation, etc. are defined.

*Entity: Basic object of entity relationship diagram. (ex. tables in database)<br>
*Attribute: facts of entities. (ex. columns in tables)<br>
*Cardinality: the possible occurence numbers of attributes between tables. (ex. one-to-one => 1 primary key in table A & 1 foreign keys in table B)

## Relational Model 
This model organises data into rows and columns with a unique key identifying each row (ex. primary key). Each table represents one entity type (ex. Customer).<br>
ER model converts into relational model through physical data modelling. <br>
Database/schema is a collection of tables, and SQL (Structured Query Language) is used for querying or maintaining the database.

### Difference between Entity Relationship Model and Relational Model
- ER model contains cardinality as the relationship between entities and relational model has less contraint in the relationship between tables.
- ER model is high-level model of entity relationship (with ER diagram) and relational model is implementation model.
- ER model: Entity | Attribute (no Domain & Key refers to unique identifier) | Relationship btw entities 
- Relational model: Relation/Table | Column/Attribute (Domain represents acceptable values & Degree refers to # of attributes) | Row/Record/Tuple

### Advantage of Relational Database
- Enables joins, aggregations and quick adhoc analysis with flexible queries
- ACID transaction: 
  - Atomicity (whole transaction is processed together)
  - Consistency (consistent rules and constraints)
  - Isolation (transactions are processed independently and isolation can to applied at low level to enable multiple concurrent transactions)
  - Durability (completed transactions are saved to the database even in case of system failure)

### Disadvatange of Relational Database
- Not a distributed system, hence, it can only scale vertically
- Cannot handle unstructured data (ex. videos, images)
- No high throughput due to ACID transactions
- No flexible schema (flexible schema can have columns that are not used in every rows)
- No high availability due to non-distributed system, thus, single point of failure

## Non-relational Database (NoSQL)
Non-relational database stores data in a non-tabular form. It is often used for large, complex or diverse data. It also performs faster because query doesn't have to view several tables.

### Advantage of Non-relational Database
- Massive dataset organisation: horizonal scaling is supported to allow data growth
- Flexible database expansion: newly formed data doesn't have to fit the data types of previously existing information
- Multiple data structures: different data types can be collated together

### Common Types of NoSQL Databases
- Apache Cassandra: *Partition row store*; data is distributed by partitions across nodes or servers & data is organised with rows & columns 
- Mongo DB: *Document store*; in addition to the key-value store, it offers API or query language that retrieves documents based on its contents
- Dynamo DB: *Key-Value store*; data is represented in key-value format
- Apache HBase: *Wide column store*; supports table, column and row, but unlike relational database, it allows columns to have different names & formats from row to row in the same table (flexible schema)
- Neo4J: *Graph database*; data is represented as nodes and edges, and the relationship between entities is the focus























