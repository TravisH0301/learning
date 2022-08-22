# Delta Lake and Data Lakehouse

reference about data lakehouse architecture
- https://www.databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html?itm_data=lakehouse-link-lakehouseblog
- https://www.databricks.com/discover/pages/the-rise-of-the-lakehouse-paradigm?itm_data=lakehouse-link-riseoflakehouse

reference about delta lake framework for building data lakehouse on data lake
- https://delta.io/
- https://delta.io/learn/tutorials/

reference on how to build delta lake on S3
- https://medium.com/seek-blog/data-lakehousing-in-aws-7c76577ed88f

<img width=800px src=https://user-images.githubusercontent.com/46085656/185820259-7256d30e-892c-4e5b-8e28-7f27ce299f19.png>
<img width=1000px src=https://user-images.githubusercontent.com/46085656/185817783-9d99b1a0-d260-4b53-9179-e77971dd0502.png>

## Delta Lake
- Delta table provides warehousing features while data still stored as parquet files in S3.
- Delta lake creating warehousing layer on top of storage layer in data lake

- Data lake: storage + processing engine
- Data lakehouse: data lake + warehousing

- bronze(Landing, ingestion) -> silver(refined tables, 3NF) -> gold (featured tables, modelling)

## Data Lakehouse 

### Data Modelling in Lakehouse
- https://www.confessionsofadataguy.com/data-modeling-in-deltalake-databricks/
- https://www.databricks.com/blog/2022/06/24/data-warehousing-modeling-techniques-and-their-implementation-on-the-databricks-lakehouse-platform.html

### Lakehouse using Delta Lake

## Lakehouse on AWS
https://www.youtube.com/watch?v=vx9hW0ZUzOA

## Data Lakehouse on Databricks

<img width=600px src=https://user-images.githubusercontent.com/46085656/185817658-0376bcca-02bf-4d50-958b-d72f12c5b243.png>
