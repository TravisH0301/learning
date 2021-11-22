# Data Pipeline
Data pipeline is a sequence of data processing. This may be consisted of an ETL, ELT or other processes.

## Directed Acyclic Graphs (DAGs)
DAGs are the conceptual framework of data pipelines to better organise data engineering tasks. It is a graphical representation of the nodes and edges, where nodes represent
data processing task and edges represent the sequence. In DAGs, edges are only single directional and there is no cycles. 

![](https://github.com/TravisH0301/learning/blob/master/images/dag_diagram.jpg)

## Airflow
Airflow is an open-source tool to build data pipelines as DAGs. The workflows are defined as Python based code and pipelines are easily visualised for monitoring and management.

### Architecture
Airflow is largely consisted of 5 components as show below.

![](https://github.com/TravisH0301/learning/blob/master/images/airflow_architecture.jpg)

- Scheduler: orchestrates the execution of tasks based on schedule or external triggers
- Work Queue: is used by scheduler to store tasks that are to be executed
- Worker: processes the tasks from the work queue, and records the outcomes
  - Note that works can run in parallel, yet, memory and processing power may be limited. Hence, it can trigger other frameworks such as Apache Spark to handle the heavy work
- Database: saves meta data such as credentials, connections, history, logs and configuration
- Web Interface: provides UI to the users for executing and monitoring the DAGs, and configuring Airflow

### Building DAGs
