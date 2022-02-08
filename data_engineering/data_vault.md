# Data Vault
Data warehouse plays a crucial role in BI and data analytics as consolidated data from multiple data sources. Kimball's and Inmon's approaches are widely used to build data warehouses. However, it can be cumbersome and risky to refactor the dimensional models every time the business requirement changes. And this is where Data Vault comes in handy. Created by Dan Linstedt in 2001, the data vault approach combines the advantages of 3NF and dimensional modellings to provide a robust solution to the business requirement changes.

## Data Warehouse Architecture
The data warehouse architecture of the data vault modelling approach consists of three layers; Staging Area, Data Vaults (Raw & Business), and Data Marts.

![data_vault_architecture](https://github.com/TravisH0301/learning/blob/master/images/data_vault_architecture.png)

- Staging Area: This layer supports the data loading process from heterogeneous data sources.
- Raw Data Vault / Business Data vault: The raw data vault holds the raw data that its contents have not been processed. And the business data vault contains modified data based on the business rules.
- Data Marts: This is an information delivery layer, where it can either be a physical model or a virtual model.

Note that there are two data vaults in the architecture; raw data vault and business data vault. By having the raw data kept separated to the modified data, one can easily 
implement business changes by adding new data sources or dimensions. The business data vault also makes the data more understandable to the business users. 

## Data Vault Components
The data vaults is mainly consisted of three components. 

![data_vault_architecture](https://github.com/TravisH0301/learning/blob/master/images/data_vault_example.png)

### Hub
Hubs are tables containing business entity identifiers. Each row is uniquely identified by the business key (BK) which has either real business meaning or is a surrogate key
from the source system. When sourcing the hub data from multiple sources, one has to be careful not to collide business keys. It is suggested to use a meaningful business
key that is unique or use a compound key made up of more than one column. <br>
And hub can optionally have a surrogate key (sequence or hash) as a primary key (PK). A surrogate key is recommended when the business key is:
- too lengthy
- not unique enough
- meaning or source system may change over time

As of the data vault 2.0, the hash key is used over the sequence key due to the following reasons:
- sequence key can get very large and requires lookups on a row by row basis
- hash key provides parallel loading by removing referential integrity (hub, link and satellite can be loaded in parallel)
- hash key is deterministic and created using hash function on the business key

However, one should be wary of the possible hash collision depending on the hash function being used. 
The hub holds the load date time to represent the first date and time the business key was discovered. And the source of the business key is recorded.

## Link

Link tables (also defined as transaction, hierarchy or relationship) act as a linkage between two or more hubs' business keys. The link contains the primary keys of the connecting hubs as the foreign keys. And the link's 
primary key is generated by combining its foreign keys, typically with a delimiter. A surrogate key may be used if the primary key is too large. The link also contains
the first load datetime and the record source for the transaction.

## Satellite
Satellite tables hold descriptive attributes to the hub or the link. They are time-dimensional table that captures historical changes. 
Each statellite can only have one parent (hub/link) table, and can never be snowflaked. The satellite contains the primary key of the parent and the load datetime. 
The parent primary key and the load datetime are used as a composite primary key to keep a track of data over time. The source of the record is also tracked. 
And most importantly, the satellite contains descriptive elements of the hub or the link. <br>
The hash difference column may be used to speed up the Change Data Capture process. This column would contain a calculated hash on the combined descriptive elements. 
And this would be compared against the hash of the inbound data to decide whether to insert the inbound data or do nothing. <br>
Note that load end datetime is not permitted under the data vault 2.0 standards to prevent physical update of the table. 


<!-- 
- other tables
- comparison to 3NF & dimension model
  - what characteristics of 3nf and dim model does data vault holds?
  - adv & drawbacks 
-->

