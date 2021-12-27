# Data Lineage & Data Quality

## Table of Contents
- [Data Lineage](#data-lineage)
  - [Data Lineage in Airflow](#data-lineage-in-airflow)
    - [Max Active Run in Airflow](#max-active-run-in-airflow)
    - [Data Partitioning](#data-partitioning)
- [Data Quality](#data-quality)
  - [Data Quality in Airflow](#data-quality-in-airflow)
    - [Service-Level Agreement (SLA)](#service-level-agreement-sla)

## Data Lineage
Data lineage is a description of discrete steps associated with a dataset. It includes creation, movement and calculation of the dataset. It is a important part of the 
data governence. 

It is important for:
- Confidence of dataset: downstream users (ex. analyst) can ensure that meaningful results are created with the correct dataset
- Defining Metrics: Clear data lineage allows the organisation to agree on the definition of the particular metrics that are calculated
- Debugging: Root of errors can be identified easily when each step of the data movement and transformation is well described

### Data Lineage in Airflow
Airflow DAGs are a natural representation for the movement and transformation of data. Data Lineage can be checked using graph view, tree view and the code.

Note that for the rendered template of the DAG which contains a code will always display the latest version, even for the historical DAGs. Hence, if a major change is 
to be made, it is recommended to create a new DAG instead of updating the DAG. 

Note that when DAG run history is cleared, Airflow will think the DAG didn't run and try to rerun the DAG. Beware of the effect on the downstream.

#### Max Active Run in Airflow
Airflow utilises parallel processing for tasks and DAG runs. If each DAG run depends on the previous run, it is important to limit the active run of the DAG. 
This can be done using the following code.

    dag = DAG(
        'first dag run',
        start_date=datetime.datetime(2021, 1, 1, 0, 0, 0, 0),
        end_date=datetime.datetime(2021, 11, 1, 0, 0, 0, 0),
        schedule_interval='@monthly',
        max_active_runs=1 # max active DAG run - important as there are more than 1 DAGs that can be run in parallel
    )

#### Data Partitioning
Data partitioning can help to prevent failure of data processing steps. In Airflow, partitioned DAG run is faster with its parallel processing. 

##### Schedule Partitioning
Timeframe can be set to reduce the amount of data that pipeline has to process and deliver relevant data.

##### Logical Partitioning
Conceptually related data can be partitioned into discrete segments and processed separately. ex) Product data & Transaction data

##### Size Partitioning
The size of the data can be limited to ensure the pipeline can handle the workload within the given memory capacity. 

## Data Quality
Data quality can be ensured by setting data quality requirements. <br>
Examples of data quality requirements:
- Data must be a certain size
- Data must be accurate to some margin of error
- Data must arrive within a given timeframe from the start of execution
- Pipelines must run on a particular schedule
- Data must not contain any sensitive information

### Data Quality in Airflow
In airflow, data quality check tasks can be added in the DAG to ensure data is processed as expected. 

#### Service-Level Agreement (SLA)
SLA can be set for the DAG in Airflow to ensure the DAG is run at the given condition, such as execution timeframe or execution missing. <br>
When SLA is missed, Airflow can send an alerting email that is set in the configuration.








