# Database vs Data Warehouse vs Data Lake
Three terms all describe a data store, yet they have different stuctures and purposes. 

## Table of Contents
- [Database](#Database)
  - [Database System: Online Transaction Processiong (OLTP)](#Database-System-Online-Transaction-Processiong-OLTP)
  - [Database System: Online Analytical Processing (OLAP)](#Database-System-Online-Analytical-Processing-OLAP)
- [Data Warehouse](#Data-Warehouse)
  - [Dimensional Modelling](#Dimensional-Modelling) 
  - [ETL vs ELT(EtLT)](#ETL-vs-ELT-EtLT)
  - [Data Warehouse Architecture](#Data-Warehouse-Architecture)
  - [OLAP Cubes](#OLAP-Cubes)
  - [Columnar Storage](#Columnar-Storage)
- [Data Lake](#Data-Lake)

## (Relational) Database
Consisted of structured data with a defined schema (logical configuration of database - how data are connected and structured). 
Data base is designed to be transactional (transaction: usage of database).

### Database System Online: Transaction Processiong (OLTP)
Database architecture that emphasises on fast operational query processing and characterised by large volume of short online transactions such as insert, update & delete.<br>
It provides no redundancy and high integrity, but it is slow for complex queries with JOINs.

### Database System: Online Analytical Processing (OLAP)
Database architecture for complex analytical queries with JOINs and characterised by relatively low volume of transactions. Data warehouse is OLAP system.<br>


## Data Warehouse
Data warehouse is a centralised data storage built upon multiple relational databases, and is used for Business Intelligence.
Data warehouse can have different architectural designs depending on business requirements and purposes. 
And the data can be stored in various data models (ex. 3NF, dimensional model & data vault) for its optimised usage.

### Data Warehouse Architecture
The most widely used methods that define architecture of the data warehouse are:

#### Kimball Architecture:
- Single ETL process takes place to create shared dimensional model
- Dimension tables are created at atomic level rather than aggregated level
- Dimension tables are shared and organised by different business processes with conformed dimensions* (Kimball's Bus)<br>
(ex. Sales team using dim_date, dim_product, dim_customer whereas Product team using dim_date, dim_product)<br>
*Conformed dimensions: identical dimensions to every different fact tables, used in different business processes (common dimension tables are shared)

#### Independent Data Marts
- Independent ETL processes take place to create specific dimensional models for different business processes
  - These separate and smaller dimensional models are called "Data Marts"
- Generally not encouraged due to inconsistent views of data and extra storage & workload required

#### Inmon's Corporate Information Factory (CIF)
- Initial ETL process takes place to create a common "Enterprise Data Warehouse" with 3NF database
- Individual subsequent ETL processes take place to create specific "Data Marts" for different business processes
  - Enterprise data warehouse acts as a single source of truth for the data marts (unlike data marts with different sources in "Independent Data Marts architecture")
- The dimensional models in data marts are mostly aggregated unlike "Kimball" architecture

#### Hybrid Kimball Bus & Inmon CIF
- Integrates Kimball architecture and Inmon's CIF architecture
- Inital ETL process takes place to create a common "Enterprise Data Warehouse" with 3NF database
- Subsequent ETL process takes place to create a common dimensional model with conformed dimensions (Kimball's Bus)

### Dimensional Modelling
Dimensional modelling is widely used in data warehouse to provide easily understandable database with fast analytical query performance.<br>
It consists of Fact tables and Dimension tables.

Fact tables has characteristics of:
- Holds transactional data (ex. sales events) with mostly numeric data

Dimension tables has characteristics of:
- Holds more detailed contextual information of the transactional data (ex. sales person information)

When the database with 3NF tables are converted into data warehouse, the tables are denormalised into dimension tables. So the query performance becomes 
better with simplied & denormalised tables. (ex. instead of querying data from address and city tables, query can be done using a single dim_location table)

### ETL vs ELT (EtLT)
#### ETL
During ETL (Extract, Transform, Load) process, the data is extracted from data sources into a staging area, where the data is transformed into required formats
using a separete engine (ex. ETL tool or Python). The transformed data is then loaded into the destinated dimensional model. This way, only the processed 
data is accessible in the data warehouse.

Addtionally, prior to the modern breed of data warehouses, the data warehouses didn't not have storage and compute power necessary to handle loading and transforming vase amount of raw data. Hence, the transformation took place elsewhere before loading the data into the data warehouse.

#### ELT
For ELT (Extract, Load, Transform) process, there is no separate engine/tool required for the transformation. The data is extracted and loaded into the destinated database and transformed within the database using its own power. 

With the emerging of modern data warehouses, which are scalable (and even columnar), both storing and transformation can be done on the data warehouses. <br>
Also the data lakes make use of ELT process, where the raw data is loaded into the bucket and transformed afterwards.

Note that, the term "ELT" was introduced from around 2020. So, some may use ELT and ETL interchangeably.

#### EtLT
Additionally, small transformation is often conducted after extraction where simple data filterings are conducted to:
- remove duplication
- parse values
- mask or obfuscate sensitive data

Usually, transformation involving business logic or data modelling is not conducted. Hence, it is called "EtLT".

#### Advantages of ELT over ETL
- Quicker loading: as transformation occurs after loading, the data is loaded into the storage quicker
- Flexibility: only required data undergoes transformation and different transformations can be applied each time
- Split of responsibility: at some companies, data engineers take care of the data ingestion (Extraction & Loading) and data analytics or analytics engineers take care of the transformation process for the analytics works

#### Disadvantages of ELT
- Slower analysis: compared to pre-structured ETL, ELT may be slow and unstable in anaylsis when data is all loaded into the storage
- Compliance: any unfiltered sensitive information may be collected in the storage 

### OLAP Cubes
OLAP cubes are an aggregation of a fact metric on a number of dimensions (multi-dimensional data array). It can rapidly analyse and present data with the number of dimensions. The below table shows the total fact sales data in three dimensions of month, city and movie.

Month|City|Movie|Sales
--|--|--|--
Jan|NY|Batman|3000
Jan|NY|Bons|4000
Jan|NY|Marvels|6000

#### OLAP Cube Technology
OLAP cubes can be served in 2 different ways:
- Approach 1: Pre-aggregate the OLAP cubes and save them on a special purpose non-relational database (MOLAP). This service is provided by major vendors.
- Approach 2: Compute OLAP cubes on the fly on the existing relational databases where dimension model resides (ROLAP).

#### OLAP Cube Operations
- Roll-up: apply aggregation on a dimension, reducing the dimension size (ex. sum up sales of each city by country)
- Drill-down: decompose on a dimension, increasing the dimension size (ex. decompose the sales of each city into smaller suburbs)
- Slice: reducing the number of dimension from N to N-1, restricting one dimenison to a single value (ex. Month dimension fixed to 'Apr') 
- Dice: reducing the size of cube by restricting values of the dimensions, while retaining the same dimension size (ex. Month dimension restricted to only 'Jan, Feb, Mar')

#### SQL Implementation of OLAP Cube
Creating OLAP cube that has an aggreagated fact sales data on dimensions of month, branch and movie.

    %%sql
    select month, city, movie, sum(sales) as sales
    FROM factsales f
         join dimdate d on (f.date_key = d.date_key)
         join dimmovie m on (f.movie_key = m.movie_key)
         join dimstore s on (f.store_key = s.store_key)
    group by (month, city, movie);

Slicing on the month dimension. (Reducing dimension by fixing the month dimension)

    %%sql
    select month, city, movie, sum(sales) as sales
    FROM factsales f
         join dimdate d on (f.date_key = d.date_key)
         join dimmovie m on (f.movie_key = m.movie_key)
         join dimstore s on (f.store_key = s.store_key)
    group by (month, city, movie)
    having month = '5';
    
Dicing on the month and branch dimensions. (Creating sub-cube by restricting values of the dimensions)

    %%sql
    select month, city, movie, sum(sales) as sales
    FROM factsales f
         join dimdate d on (f.date_key = d.date_key)
         join dimmovie m on (f.movie_key = m.movie_key)
         join dimstore s on (f.store_key = s.store_key)
    group by (month, city, movie)
    having month in (1,5)
           and city in ('NY', 'SF');
           
Rolling up on the city dimension to the country dimension. (Aggregation on city dimension)

    %%sql
    select month, country, movie, sum(sales) as sales
    FROM factsales f
         join dimdate d on (f.date_key = d.date_key)
         join dimmovie m on (f.movie_key = m.movie_key)
         join dimstore s on (f.store_key = s.store_key)
    group by (month, country, movie)
    
Drilling down on the city dimension to the suburb dimension. (Decomposition on city dimension)

    %%sql
    select month, suburb, movie, sum(sales) as sales
    FROM factsales f
         join dimdate d on (f.date_key = d.date_key)
         join dimmovie m on (f.movie_key = m.movie_key)
         join dimstore s on (f.store_key = s.store_key)
    group by (month, suburb, movie)

### Columnar Storage
Columnar (or column-oriented) storage stores data by serialising column data, rather than serialising the row data. Hence, when it comes to column-wise database operations, such as aggregation, it provides faster performance than row-oriented storage. Also, unless entire data table is to be retrieved or the operation processes only one single record at one time, columnar storage provides higher efficiency. 

Columnar storage is widely used in data warehouses and NoSQL databases. 

## Data Lake 
Centralised repository for both structured and unstructured data storage. It can contain raw and unprocess data. 
Usually data stored are not determined for their usages. <br>
Just like ELT based data warehouse, data lake can also use ELT process where transformation process occurs after data is loaded into the data lake. 
