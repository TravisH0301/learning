# Star Schema & Snowflake Schema
These shemas are widely used as data warehouse schemas, using conceptual fact and dimension tables.<br>
Fact table contains unique rows of information, whereas, dimension table contains multiple transaction information. Fact table and dimension table forms one-to-one 
or one-to-many cardinality.

## Data Ingestion Order
When data is populated into the dimensional model, the dimension tables should be ingested prior to the fact tables. This is to provide the related key value (surrogate key or
primary-foreign key) from the dimension table to the fact table. 

In case of the <strong>'Late Arriving Dimensions'</strong>, dummy values are to be ingested into the dimensions and to be linked to the fact. And when the dimension data arrives, the dummy values can be updated.

## Star Schema
The star schema consists of one or more fact tables referencing to any number of dimension tables.<br>
Its name comes from how tables are modelled. Usually, a fact table is surrounded by multiple dimension tables forming a star.<br>
<img src="https://github.com/TravisH0301/learning/blob/master/images/star_schema_example.jpg" width="800">
### Advantage
- As the fact table connects all the information sources from multiple dimension tables, query becomes faster
### Disadvantage
- Using a fact table means duplication of primary key information of the dimension tables in the fact table. Hence, this denormalisation
slows down write process (as more tables need to be updated).
- Since star schema is usally built for a specific analytics purpose, the query may not be flexible.
- Due to the nature of the conceptual tables (fact, dimension), many-to-many relationships are limited.

## Snowflake Schema
Snowflake schema is a re-arrangement of star schema, where dimension tables are normalised with multi-dimensional structure. 
Still, fact tables and dimension tables are used.<br>
<img src="https://github.com/TravisH0301/learning/blob/master/images/snowflake_schema_example.jpg" width="800">
### Advantage
- Storage space can be saved by normalising dimension tables. Especially, when it contains long non-numerical strings.
### Disadvantage
- Multi-dimensional structure can create high complexity, leading to longer query time.
- Lower data integrity level compared to the traditional highly-normalised database (Snowflake schame is not 3NF normalised).
