# Data Vault
Data warehouse plays a crucial role in BI and data analytics as consolidated data from multiple data sources. Kimball's and Inmon's approaches are widely used to build data warehouses. However, it can be cumbersome and risky to refactor the dimensional models every time the business requirement changes. And this is where Data Vault comes in handy. Created by Dan Linstedt in 2001, the data vault approach combines the advantages of 3NF and dimensional modellings to provide a robust solution to the business requirement changes.

## Data Warehouse Architecture
The data warehouse architecture of the data vault modelling approach consists of three parts; Staging Area, Data Vaults (Raw & Business), and Data Marts.

-insert image-

- Staging Area: This layer supports the data loading process from heterogeneous data sources.
- Raw Data Vault / Business Data vault: The raw data vault holds the raw data that its contents have not been processed. And the business data vault contains modified data based on the business rules.
- Data Marts: This is an information delivery layer, where it can either be a physical model or a virtual model.


architecture
^ why it adapts well to the business requirement changes
components
examples


https://en.wikipedia.org/wiki/Data_vault_modeling
https://adatis.co.uk/a-gentle-introduction-to-data-vault/
* https://www.researchgate.net/publication/312486486_Comparative_study_of_data_warehouses_modeling_approaches_Inmon_Kimball_and_Data_Vault
* https://medium.com/@jryan999/data-vault-an-overview-27bed8a1bf9f
