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

- Hub: Hubs are tables containing business entity identifiers, including the business key. The business key has a real-world meaning.
- Link: Link tables act as a linkage between two or more hub's business keys.
- Satellite: Satellite tables hold descriptive attributes to the hub or the link. They are time-dimensional table that captures historical changes using SCD type 2. 
Each statellite can only have one parent (hub/link) table. The primary key of the statellite table is obtained by combining the key of the parent and the loading date.




======
component diagram
components
comparison to 3NF & dimension model

examples


https://en.wikipedia.org/wiki/Data_vault_modeling
https://adatis.co.uk/a-gentle-introduction-to-data-vault/
* https://www.researchgate.net/publication/312486486_Comparative_study_of_data_warehouses_modeling_approaches_Inmon_Kimball_and_Data_Vault
* https://medium.com/@jryan999/data-vault-an-overview-27bed8a1bf9f
