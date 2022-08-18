# Partitioning in Teradata
Teradata is a relational database management system (RDBMS) based on MPP (Massively Parallel Processing) architecture. 
In Teradata, each node can contain multiple Access Module Processors (AMPs) that share nothing with each other to achieve parallel processing.

<img src="https://user-images.githubusercontent.com/46085656/185266803-d47fd0e9-704c-4380-b517-80bbe1602f59.png" width="600px">

## Primary Index
Primary Index (PI), or Distribution Key (DK) in other MPP systems, is the important aspect of Teradata to maximise performance.
 
The primary index works at a table level and it turns a column (or columns) into a key for each row of data. 
In the MPP system, the data is distributed to individual nodes based on the primary index. 

When the primary index is unique, the rows are distributed in a 'round-robin' manner to all nodes. However, when it's duplicated, the data
is grouped into selective nodes causing "skewness" of data. The node with high load is called "hot spot", and it becomes a bottle to the parallelism.

The uniqueness of the primary index can be governed by using Unique Primary Index (UPI) in Teradata.

    CREATE TABLE SALES (
      SALE_ID INTEGER NOT NULL
      ,PROD_ID INTEGER NOT NULL
      ,TOT_SOLD INTEGER
      ,REVENUE INTEGER
      ,SALE_DT DATE FORMAT 'YYYY-MM-DD' NOT NULL
    )
    UNIQUE PRIMARY INDEX (SALE_ID); -- The term "UNIQUE" governs uniqueness of the primary index "SALES_ID"

### Selection of Primary Index
When selecting a primary index, a primary key of the table is a good choice, given it is a unique column providing even distribution of the data throughout AMPs.

If no unique column exists, a combination of columns can be used instead to create a unique primary key

## Partitioning by Primary Index
In Teradata, partitioning is done by hash of key using a hash function. When a row is inserted, the key of the row will be hashed. And the row value will be allocated by the hash bucket to the AMP, where its hash is covered by the AMP's range of hash codes.

<img src="https://user-images.githubusercontent.com/46085656/185113418-d9ec5871-1fcc-4521-a37f-bb05776086b0.png" width="600px">

## Partitioned Primary Index
Hash partitioning can help to achieve even distribution when key values are not evenly distributed. However, partitioning by hash of key removes the order of the key values - which can improve data scanning. Teradata's Partitioned Primary Index (PPI) can help to compensate for this by further partitioning the data within AMPs.

Partitioning data within AMPs removes the need of full table scan and improves query performance. <br>
This can be achieved using "Partition By" clause. Some examples include:

### Direct partitioning on a numeric column
    CREATE TABLE SALES (
      SALE_ID INTEGER NOT NULL
      ,PROD_ID INTEGER NOT NULL
      ,TOT_SOLD INTEGER
      ,REVENUE INTEGER
      ,SALE_DT DATE FORMAT 'YYYY-MM-DD' NOT NULL
    )
    UNIQUE PRIMARY INDEX (SALE_ID)
    PARTITION BY EXTRACT(MONTH FROM SALE_DT);

In this case, the records in AMPs are partitioned by the month. This is useful when read queries are made on a monthly level.

### Expression partitioning on RANGE_N function
    CREATE TABLE SALES (
      SALE_ID INTEGER NOT NULL
      ,PROD_ID INTEGER NOT NULL
      ,TOT_SOLD INTEGER
      ,REVENUE INTEGER
      ,SALE_DT DATE FORMAT 'YYYY-MM-DD' NOT NULL
    )
    UNIQUE PRIMARY INDEX (SALE_ID)
    PARTITION BY RANGE_N(
      SALE_DT BETWEEN DATE '2020-01-01' 
      AND DATE '2022-12-31'
      EACH INTERVAL '1' DAY
    );

The records in AMPs are partitioned by SALE_DT on a 1 day interval. This makes the query with a range of days efficient.

### Expression partitioning on CASE_N function
    CREATE TABLE SALES (
      SALE_ID INTEGER NOT NULL
      ,PROD_ID INTEGER NOT NULL
      ,TOT_SOLD INTEGER
      ,REVENUE INTEGER
      ,SALE_DT DATE FORMAT 'YYYY-MM-DD' NOT NULL
    )
    UNIQUE PRIMARY INDEX (SALE_ID)
    PARTITION BY CASE_N(
      REVENUE < 100
      ,REVENUE < 1000
      ,NO CASE
      ,UNKNOWN
    );
    
In AMPs, the records are partitioned into 4 partitions.
1. Records with REVENUE less than 100
2. Records with REVENUE greater or equal to 100, and less than 1000
3. Records with REVENUE greater or equal to 1000
4. Records with NULL REVENUE

This partitioning helps when querying data with a filter on REVENUE.

Note that "NO CASE" creates a partition when a record doesn't fit into any of the defined conditions. <br>
And "UNKNOWN" creates a partition for NULL values.

### Multilevel partitioning on RANGE_N or CASE_N functions
Multilevel partitoning can be achieved using RANGE_N or CASE_N functions.

    CREATE TABLE SALES (
      SALE_ID INTEGER NOT NULL
      ,PROD_ID INTEGER NOT NULL
      ,TOT_SOLD INTEGER
      ,REVENUE INTEGER
      ,SALE_DT DATE FORMAT 'YYYY-MM-DD' NOT NULL
    )
    UNIQUE PRIMARY INDEX (SALE_ID)
    PARTITION BY (
      CASE_N(
        REVENUE < 100
        ,REVENUE < 1000
        ,NO CASE
        ,UNKNOWN)
      ,RANGE_N(
        SALE_DT BETWEEN DATE '2020-01-01' AND DATE '2022-12-31'
        EACH INTERVAL '1' DAY
      )
    );
    
In this case, the records in each partition level defined by CASE_N will be further partitioned on SALE_DT with a 1 day interval.

This helps when filtering data on both REVENUE and SALE_DT.

Note that only one CASE_N or one RANGE_N can be used for each partition level.

## Collecting Statistics
In Teradata, the query optimiser can use pre-calculated statistics on the data demographics to optimise the query. 

The statistics can be collected for primary index, partition column and any columns that are frequently used in WHERE or JOIN statements. <br>
It is recommended to refresh statistics when 10% of the data demographics changes. And any unused statistics can be dropped to save space & CPU consumption.

    COLLECT STATS INDEX ({primary index(es)}) ON {databasename}.{tablename}; 
    COLLECT STATS COLUMN (PARTITION) ON {databasename}.{tablename};
    COLLECT STATS COLUMN ({column(s)}) ON {databasename}.{tablename};
    
    -- The above statements can be combined as below
    COLLECT STATS 
    INDEX ({primary index(es)})
    ,COLUMN (PARTITION)
    ,COLUMN ({column(s)}) 
    ON {databasename}.{tablename};
    
    -- Check definition of the defined statistics
    SHOW STATS ON {databasename}.{tablename};
    
    -- Check details of the collected statistics
    HELP STATS {databasename}.{tablename};
    
    -- Refresh statistics
    COLLECT STATS ON {databasename}.{tablename}; -- all statistics
    COLLECT STATS INDEX ({primary index(es)}) ON {databasename}.{tablename}; -- specific statistics
    
    -- Drop statistics
    DROP STATISTICS ON {databasename}.{tablename}; -- all statistics
    DROP STATISTICS INDEX ({primary index(es)}) ON {databasename}.{tablename}; -- specific statistics








