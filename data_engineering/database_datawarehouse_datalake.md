# Database vs Data Warehouse vs Data Lake
Three terms all describe a data storage, yet they have different stuctures and purposes. 

## Database
Usually consisted of structured data with a defined schema (logical configuration of database - how data are connected and structured). 
Data base is designed to be transactional (transaction: usage of database).

### Database System: Online Transaction Processiong (OLTP)
Database architecture that emphasises on fast operational query processing and characterised by large volume of short online transactions such as insert, update & delete.<br>
It provides no redundancy and high integrity, but it is slow for complex queries with JOINs.

### Database System: Online Analytical Processing (OLAP)
Database architecture for complex analytical queries with JOINs and characterised by relatively low volume of transactions. Data warehouse is OLAP system.<br>


## Data Warehouse
Built upon existing database (or several databases) and used for business intelligence. Data warehouse consumes data from databases to create 
a optimised structure to speed up queries for data analysis.

### Dimensional Modelling
Dimensional modelling is used in data warehouse to provide easily understandable database with fast analytical query performance.<br>
It consists of Fact tables and Dimension tables.

Fact tables has characteristics of:
- Holds transactional data (ex. sales events) with mostly numeric data

Dimension tables has characteristics of:
- Holds more detailed contextual information of the transactional data (ex. sales person information)

When the database with 3NF tables are converted into data warehouse, the tables are denormalised into dimension tables. So the query performance becomes 
better with simplied & denormalised tables. (ex. instead of querying data from address and city tables, query can be done using a single dim_location table)

### ETL vs ELT
#### ETL based Data Warehouse
During ETL (Extract, Transform, Load) process, the data is extracted from data sources and transformed into required formats for applications and then
finally loaded into the warehouse. This way, the data warehouse contains preprocessed data for analytics purpose. 

#### ELT based Data Warehouse
For ELT (Extract, Load, Transform) process, there is separate tool for ETL transformation. Instead, the data is extracted and loaded into the warehouse.
The transformation is handled inside the data warehouse itself. 

#### Advantages of ELT over ETL
- Quicker loading: as transformation occurs after loading, the data is loaded into the storage quicker
- Flexibility: only required data undergoes transformation and different transformations can be applied each time

#### Disadvantages of ELT
- Slower analysis: compared to pre-structured ETL, ELT may be slow and unstable in anaylsis when data is all loaded into the storage
- Compliance: any unfiltered sensitive information may be collected in the storage 

### Data Warehouse Architecture
The most widely used methods that define architecture of the data warehouse are:

#### Kimball Architecture:
- Single ETL process takes place to create shared dimensional model
- Dimension tables are created at atomic level rather than aggregated level
- Dimension tables are shared and organised by different business processes with conformed dimensions (Kimball's Bus)<br>
(ex. Sales team using dim_date, dim_product, dim_customer whereas Product team using dim_date, dim_product)

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

### OLAP Cubes
OLAP cubes are an aggregation of a fact metric on a number of dimensions (multi-dimensional data array). It can rapidly analyse and present data with the number of dimensions. The below table shows the total fact sales data in three dimensions of movie, month and branch. 

Month|Branch|Movie|Sales
--|--|--|--
Jan|NY|Batman|3000
Jan|NY|Bons|4000
Jan|NY|Marvels|6000

#### OLAP Cube Operations
- Roll-up: apply aggregation on a dimension, reducing columns in the dimension (ex. sum up sales of each city by country)
- Drill-down: decompose on a dimension, creating columns in the dimension (ex. decompose the sales of each city into smaller suburbs)
- Slice: reducing the number of dimension from N to N-1, restricting one dimenison to a single value (ex. Month dimension fixed to 'Apr') 
- Dice: reducing the size of cube by restricting values of the dimensions, while retaining the same dimension size (ex. Month dimension restricted to only 'Jan, Feb, Mar')

## Data Lake 
Centralised repository for both structured and unstructured data storage. It can contain raw and unprocess data. 
Usually data stored are not determined for their usages. <br>
Just like ELT based data warehouse, data lake can also use ELT process where transformation process occurs after data is loaded into the data lake. 
