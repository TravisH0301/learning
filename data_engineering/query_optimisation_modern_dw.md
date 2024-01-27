# Query Optimisation in Modern Data Warehouses
Query optimisation refers to the process of enhancing the efficiency of a query to minimise the use of computing resources and execution time.
There are several aspects of query optimisation that can be implemented in modern data warehouses.

- [Paritioning / Sharding / Distribution](#paritioning--sharding--distribution)
- [Clustering / Sorting](#clustering--sorting)
- [Materialised View / Caching](#materialised-view--caching)
- [Column Compression](#column-compression)
- [Statistic Collection](#statistic-collection)
- [Query Tuning](#query-tuning)
- [Query Execution Plan](#query-execution-plan)

## Paritioning / Sharding / Distribution
Paritioning involes dividing a large data table into smaller pieces and storing across multiple nodes. Partitioning can be used to achieve:
- Even distribution: prevents data skew (or hot spot) and improves overall query performance with parallel processing. However, it may not be effective for some queries that don't align with the partition key.
- Query optimisation: significantly speed up specific queries that filter or join on partition key (via data skipping). However, it may lead to uneven data distribution.

The approach of the partitioning can be decided based on the usecase. For example, one may want to achieve even distribution of data with paritioning and optimising
the query by using other optimisation techniques, such as clustering. Or one may choose a hybrid approach, considering both even data distribution and common query patterns.

## Clustering / Sorting
Clustering or Sorting refers to storing related records together within partitions. By colocating records based on column(s) that are frequently used for joining or ordering,
one can reduce data shuffling and lead to faster query performance. 

This requires a careful planning with good understanding of query patterns. One might require to re-cluster as query pattern evolves.

## Materialised View / Caching
Materialised view refers to storing results of a query physically. So when the query is re-executed, the data warehouse retrieves the results from the materialised view, rather
than running the execution again. This is particularly effective for complex and frequently executed queries. Additionally, materialised views are more flexible and 
easier to manage than storing query results as a physical table.

While it provides better performance and reduced loads, the materialised views may not always be up-to-date with the latest changes in the base tables.
Hence, one needs to choose a refresh strategy that balances between data freshness and system load.

Similarly, the data warehouse may cache results of the query for a certain period of time to improve query performance.

## Column Compression
Given modern data warehouses use columnar storage, each data column can be compressed and stored using encoding techniques.
This improves performance with less data reading from disk and more data fitting in memory. Generally, data is automatically compressed when loaded into the data warehouse.

## Statistic Collection
Collecting statistics in modern data warehouses is a vital aspect of query optimisation. 
Statistics provide the query optimiser (or planner) with crucial information about data distribution, size, and characteristics, which it uses to make informed 
decisions about the best way to execute a query. 

Many modern data warehouses automatically collect statistics, but in some cases, one may need to manually update statistics, especially after significant data changes in the tables.

## Query Tuning
Apart from the above optimisation techniques used in modern data warehouses, query tuning is a crucial practice in query optimisation. Some of best practises include:
- Reduce data processed by selecing only necessary columns and filtering data early.
- Simplify queries by breaking down complex queries into simpler ones.
- Use joins effectively with the right type of joins.
- Avoid self-join, which can lead to complex and hard-to-read query. Instead use window function if possible.
- Avoid too many subqueryies/CTEs that lead to multiple database passes. Instead use case statement if possible.
- Use appropriate optimisation methods to reduce data reading & shuffling - e.g., parititoning, clustering & etc.

## Query Execution Plan
Query execution plan allows one to understand how data is going to be processed in multiple steps. This can provide insights on possible bottleneck of the execution plan,
that can be addressed by tuning the query or applying optimisation methods.
