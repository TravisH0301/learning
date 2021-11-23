# Airflow Plugins
Airflow has built-in plugins that allow users to increase reusability of the code and to easily interact with external systems. 
The most common types of plugins are Operators and Hooks. 

[Airflow Contrib](https://github.com/apache/airflow/tree/main/airflow/contrib) is an open source community with custom made Airflow plugins.

## Operators
Operators allow Airflow to easily create a task with custom processing. For example, instead of creating PythonOperator to copy data from AWS S3 to AWS Redshift, 
S3ToRedShiftOperator can be used to reduce the coding and increase reusability.

## Hooks
Hooks allow Airflow to integrate with non-standard data stores and systems. For example, MailChimp can be integrated by using Hooks. 
