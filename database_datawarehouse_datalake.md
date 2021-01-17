# Database vs Data Warehouse vs Data Lake
Three terms all describe a data storage, yet they have different stuctures and purposes. 

## Database
Usually consisted of structured data with a defined schema (logical configuration of database - how data are connected and structured). 
Data base is designed to be transactional (transaction: usage of database).

### Database System: Online Transaction Processiong (OLTP)
Database architecture that emphasises on fast query processing and characterised by large volume of short online transactions such as insert, update & delete.

### Database System: Online Analytical Processing (OLAP)
Database architecture for complex queries and characterised by relatively low volume of transactions. Data warehouse is OLAP system. 

## Data Warehouse
Built upon existing database (or several databases) and used for business intelligence. Data warehouse consumes data from databases to create 
a optimised structure to speed up queries for data analysis.

### ETL based Data Warehouse
During ETL (Extract, Transform, Load) process, the data is extracted from data sources and transformed into required formats for applications and then
finally loaded into the warehouse. This way, the data warehouse contains preprocessed data for analytics purpose. 

### ELT based Data Warehouse
For ELT (Extract, Load, Transform) process, there is separate tool for ETL transformation. Instead, the data is extracted and loaded into the warehouse.
The transformation is handled inside the data warehouse itself. 

#### Advantages of ELT over ETL
- Quicker loading: as transformation occurs after loading, the data is loaded into the storage quicker
- Flexibility: only required data undergoes transformation and different transformations can be applied each time

#### Disadvantages of ELT
- Slower analysis: compared to pre-structured ETL, ELT may be slow and unstable in anaylsis when data is all loaded into the storage
- Compliance: any unfiltered sensitive information may be collected in the storage 

## Data Lake 
Centralised repository for both structured and unstructured data storage. It can contain raw and unprocess data. 
Usually data stored are not determined for their usages. <br>
Just like ELT based data warehouse, data lake can also use ELT process where transformation process occurs after data is loaded into the data lake. 
