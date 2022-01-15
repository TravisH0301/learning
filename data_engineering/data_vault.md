# Data Vault
Data warehouse plays a key role in BI and data analytics as a set of consolidated data from multiple data sources. Kimball's and Inmon's approaches have been
widely used to build data warehouses. However, it can be cumbersome and risky to refactor the dimensional models every time the business requirement changes. 
And this is where Data Vault comes in handy. Created by Dan Linstedt in 2001, the data vault approach combines the advantages of 3NF and dimensional modellings to 
provide a robust solution to data source or business requirement changes. 

## Data Warehouse Architecture
The data warehouse architecture using the data vault approach consists of three parts; Staging Area, Data Warehouse and Data Marts. 

