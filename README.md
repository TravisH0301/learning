# Learning
Repository containing brief notes made during learning.

## Table of Contents
1. [Software Engineering](#1-Software-Engineering)
2. [Backend Engineering](#2-Backend-Engineering)
3. [Data Engineering](#3-Data-Engineering)
4. [Data Science](#4-Data-Science)
5. [Miscellaneous](#5-Miscellaneous)

## 1. Software Engineering 
[Back to table of contents](#Table-of-Contents)
### Data Structures
- [Linked Lists](https://github.com/TravisH0301/learning/blob/master/software_engineering/linked_lists.md): Introduction to linked lists and Python implementation
### Algorithms
- [Time & Space Complexity](https://github.com/TravisH0301/learning/blob/master/software_engineering/time_space_complexity.md): Analysis method for time and space complexity

### Python
- [Context Manager](https://github.com/TravisH0301/learning/blob/master/software_engineering/context_manager.md): Use of context manager to manage external resources on Python
- [pre-commit](https://github.com/TravisH0301/learning/blob/master/software_engineering/pre_commit.md): How to set up Git hooks with pre-commit to review code automatically before the commit
- [Concurrency](https://github.com/TravisH0301/learning/blob/master/software_engineering/concurrency_python.md): How to achieve concurrency to process multiple tasks asynchronously using threading and asyncio in Python
- [Recursion](https://github.com/TravisH0301/learning/blob/master/software_engineering/recursion_python.md): Recursion in Python using examples

### Version Control
- [Git](https://github.com/TravisH0301/learning/blob/master/software_engineering/git.md): Instructions to version control using Git

### Network
- [SSH](https://github.com/TravisH0301/learning/blob/master/software_engineering/ssh.md): How to establish SSH session between server and client using public key authentication, and how to transfer files using SFTP

## 2. Backend Engineering
[Back to table of contents](#Table-of-Contents)
### Internet
- [Internet](https://github.com/TravisH0301/learning/blob/master/backend_engineering/internet.md): Basic explanation of what internet is, and how information is communicated through internet with different protocol layers
- [HTTP](https://github.com/TravisH0301/learning/blob/master/backend_engineering/http.md): Characteristics of HTTP, how communication is made between a client and a server using HTTP request and HTTP response, and HTTP/2 & HTTP/3

### API
- [REST API](https://github.com/TravisH0301/learning/blob/master/backend_engineering/rest_api.md): Architectural constraints of REST API

### Authentication
- [OAuth](https://github.com/TravisH0301/learning/blob/master/backend_engineering/oauth.md): Working mechanism of OAuth to delegate access to the applications

### System Design
- [System Design](https://github.com/TravisH0301/system_design): TBC

## 3. Data Engineering
[Back to table of contents](#Table-of-Contents)
### Database 
- [Database Engine & API](https://github.com/TravisH0301/learning/blob/master/data_engineering/database_engine_api.md): Definition of database engine in database management system and introduction of database engine API such as Open Database Connectivity (ODBC) and Object Linking and Embedding, Database (OLE DB)
- [Distributed Database](https://github.com/TravisH0301/learning/blob/master/data_engineering/distributed_database.md): Pros & Cons of distributed database with an introduction to the distributed NoSQL database, Apache Cassandra
- [MPP Database](https://github.com/TravisH0301/learning/blob/master/data_engineering/mpp_database.md): Introduction to Massively Parallel Processing (MPP) and its architectures of grid computing and clustering | Methods of table partitioning: Distribution style & Sorting key
- [Partitioning in Teradata](https://github.com/TravisH0301/learning/blob/master/data_engineering/partitioning_teradata.md): How data is partitioned in Teradata and how to optimise for queries by further partitioning data in nodes and collecting statistics

### Data Modelling
- [Datebase vs Data Warehouse vs Data Lake](https://github.com/TravisH0301/learning/blob/master/data_engineering/database_datawarehouse_datalake.md): Definition of relational database (OLTP & OLAP), data warehousing (architecture - Kimball's & Inmon's, dimensional data modelling, ETL vs ELT & OLAP Cube) and data lake
- [Data Modelling](https://github.com/TravisH0301/learning/blob/master/data_engineering/data_modelling.md): How to do data modelling (Entity Relationship Diagram) and aspects of relational database & non-relational (NoSQL) database
- [Relational Data Model](https://github.com/TravisH0301/learning/blob/master/data_engineering/relational_data_model.md): How to structure normalised/denormalised data models
- [Star Schema & Snowflake Schema](https://github.com/TravisH0301/learning/blob/master/data_engineering/star_snowflake_schema.md): Introduction to star schema & snowflake schema
- [Slowly Changing Dimension (SCD)](https://github.com/TravisH0301/learning/blob/master/data_engineering/slowly_changing_dimension.md): Types of slowly changing dimensions (SCDs) to adapt to changes in the data source
- [Data Vault](https://github.com/TravisH0301/learning/blob/master/data_engineering/data_vault.md): Data vault architecture and its components, and how data vault fits into the medallion architecture

### Data Pipeline
- [SQL-to-SQL ETL Design](https://github.com/TravisH0301/learning/blob/master/data_engineering/sql_to_sql_etl.md): Typical ETL process design for database to database data integration
- [Data Pipeline and Airflow](https://github.com/TravisH0301/learning/blob/master/data_engineering/data_pipeline_airflow.md): Introduction of Directed Acyclic Graphs (DAGs) in data pipeline and building DAGs with Apache Airflow
- [Data Lineage & Quality in Airflow](https://github.com/TravisH0301/learning/blob/master/data_engineering/data_lineage_data_quality_airflow.md): Managing data lineage and data quality in Apache Airflow

### Data Governance
- [Data Governance](https://github.com/TravisH0301/learning/blob/master/data_engineering/data_governance.md): What is data governance? Key components of data governance - processes, people & technology

### SQL
- [SQL Join](https://github.com/TravisH0301/learning/blob/master/data_engineering/slq_join.md): Examples for SQL joins; Inner Join, Left Join, Right Join, Full Join, Anti-Join & Cross Join
- [Window Functions in SQL](https://github.com/TravisH0301/learning/blob/master/data_engineering/window_functions_sql.md): Introduction to window functions in SQL with examples
  
### Parallel and Distributed Processing
- [Multiprocessing and Ray on Python](https://github.com/TravisH0301/learning/blob/master/data_engineering/multiprocessing_ray_python.md): Instruction on implementing parallel processing on Python using Multiprocessing and Ray, and their comparison 
- [Pandas Parallelism via Modin](https://github.com/TravisH0301/learning/blob/master/data_engineering/pandas_parallelism_modin.md): Instruction on how to run Pandas operations in parallel by using Modin

## 4. Data Science
[Back to table of contents](#Table-of-Contents)
### Statistics
- [Measure of Skewness and Kurtosis](https://github.com/TravisH0301/learning/blob/master/data_science/skewness_kurtosis.md): Understanding of skewness and kurtosis
- [Statistical Feature Selection Methods](https://github.com/TravisH0301/learning/blob/master/data_science/feature_selection_methods.md): Reference to feature selection methods for numercial and categorical data
- [Average of Average](https://github.com/TravisH0301/learning/blob/master/data_science/avg_of_avg.md): Interpretation of different average of average methods
- [Image Segmentation by K-Means Clustering](https://github.com/TravisH0301/learning/blob/master/data_science/image_segmentation_with_k_means_clustering.md): Unsupervised image segmentation by k-means clustering 

### Machine Learning
- [Time Series Forecasting](https://github.com/TravisH0301/learning/blob/master/data_science/time_series_forecasting.md): Time series forecasting using statistical modelling

### Deep Learning
- [Neural Network Optimisation](https://github.com/TravisH0301/learning/blob/master/data_science/neural_network_optimisation.md): Optimisation methods for neural networks
- [Convolution Neural Network](https://github.com/TravisH0301/learning/blob/master/data_science/convolutional_neural_network.md): Explantion of convolutional neural network
- [Convolutional Encoder Decoder](https://github.com/TravisH0301/learning/blob/master/data_science/convolutional_encoder_decoder.md): Variation of convolutional neural network
- [VGG model](https://github.com/TravisH0301/learning/blob/master/data_science/vgg_model.md): Variation of convolutional neural network
  
## 5. Miscellaneous
[Back to table of contents](#Table-of-Contents)
### Computer Science
- [Binary, Bit & Byte](https://github.com/TravisH0301/learning/blob/master/miscellaneous/binary_bit_byte.md): Explanation of binary, bit and byte, and how they are used in modern computer architecture and character encoding

### Linux
- [Linux Server](https://github.com/TravisH0301/learning/blob/master/miscellaneous/linux_server.md): Description of how to connect remote Linux server with some basic Linux terminal commands

### Anaconda
- [Anaconda Virtual Environment](https://github.com/TravisH0301/learning/blob/master/miscellaneous/conda_virtual_env.md): Instruction on how to setup Anaconda virtual environment

### Geographic Information System
- [Geographic Cooridnate System](https://github.com/TravisH0301/learning/blob/master/miscellaneous/geographic_coordinate_system.md): Explanation of commonly used geographic cooridnate system
