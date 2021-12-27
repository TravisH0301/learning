# SQL-to-SQL ETL Design
When moving data from one database to another database, Extract, Transform & Load (ETL) process is used.<br>
The below is an example of a typical ETL process between databases.

## Table of Contents
- [ETL Process](#etl-process)

## ETL Process
#### 1.Source DB ==> 2.Flat file storage ==> 3.Raw DB ==> 4.Staging DB ==> 5.DWH ==> 6.OLAP Cube ==> 7.BI/Anayltics
<b>1-2</b>. Data is pulled from the source DB and stored as flat files* such as csv files.<br>
*Flat files are used to prevent potential data loss and used as back-up. 

<b>2-3</b>. Data is then loaded from the flat files to the raw DB. The raw DB* is a duplication of the source DB and used in the transform process. <br>
*The raw DB is used to prevent affecting the source DB, which is often production/operational DB. 

<b>3-4</b>. This is the staging area where data transformation and quality assurance occur. While data is transformmed into the format required for DWH, it is also
checked for quality to ensure they are meeting the requirement. Also the data can be regulated to follow any regulations. <br>
In the staging DB, the tables are generally named using the prefix depending on their property and applied transformation. (ex. f: fact, agg: aggregated)<br>
On cloud, staging can take place in object storage (ex. AWS S3) and transform and move the data with tools (ex. AWS Data Pipeline or ETL workers orchestrated by Airflow).

<b>4-5</b>. The transformed data is then loaded into the DWH.

<b>5-6</b>. The pre-aggregated data can be created using OLAP cubes and stored in the additional databases.

<b>6-7</b>. Data is consumed by BI and analytics.


