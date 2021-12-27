# Relational Data Model

## Table of Contents
- [Types of Relational Databases](#types-of-relational-databases)
- [Structuring Database](#structuring-database)
  - [Normalisation](#normalisation)
    - [Process of Normalisation](#process-of-normalisation)
  - [Denormalisation:](#denormalisation)

## Types of Relational Databases
- Online Analytical Processing (OLAP): database optimised for complex analytics and adhoc queries, including aggregations. It is optimised for reads.
- Online Transactional Processing (OLTP): database optimised for workloads with less complex queries in large volume. The types of quries for OLTP are read, insert, update, and delete.

## Structuring Database
### Normalisation
Normalisation is done to reduce data redundancy and increase data integrity

#### Process of Normalisation
- First Normal Form (1NF):
  - Atomic values: each cell contains unique and single values (ex. no list of values in a cell)
  - To be able to add data without altering tables
  - Separate different relations to different tables (ex. customer table & sales table) 
  - Keep relationship between tables using foreign keys
  - Each row must be unique
- Second Normal Form (2NF):
  - All columns must rely on the primary key (ex. unique primary key for each row)
- Third Normal Form (3NF):
  - No transitive dependencies (transitive dependency is when a non-key attribute determines columns)
  ex.
  Student ID is a primary key determining Name, Campus ID and Campus Name. However, Campus Name is also dependent on Campus ID, which is a non-key attribute. 
  Since Campus Name depends on Campus ID, which is not unique, If Campus Name has to be changed, all rows with the identical Campus ID has to be changed too.
  
  |Student ID|Name|Campus ID|Campus Name|
  |--|--|--|--|
  |001|John Osborn|111|Caulfield|
  |002|Mark Williams|111|Caulfield|
  
  The above table can be separated into the below two tables to prevent transitive dependency and achieve 3NF.
  |Student ID|Name|Campus ID|
  |--|--|--|
  |001|John Osborn|111|
  |002|Mark Williams|111|
  
  |Campus ID|Campus Name|
  |--|--|
  |111|Caulfield|
  
### Denormalisation: 
Sometimes, denormalisation must be done in read (read) heavy workloads to increase performance. This is done at the expense of losing write (insert, update, delete) performance. This is because normalisation results in expansion of the database with more number of tables.

Denormalisation is done through modelling the dataset to ensure read performance is enhanced by denormalising tables.




