# Data Pipeline
Data pipeline is a sequence of data processing. This may be consisted of an ETL, ELT or other processes.

## Directed Acyclic Graphs (DAGs)
DAGs are the conceptual framework of data pipelines to better organise data engineering tasks. It is a graphical representation of the nodes and edges, where nodes represent
data processing task and edges represent the sequence. In DAGs, edges are only single directional and there is no cycles. 

<img width="500" src="https://github.com/TravisH0301/learning/blob/master/images/dag_diagram.jpg"></img>

## Airflow
Airflow is an open-source tool to build data pipelines as DAGs. The workflows are defined as Python based code and pipelines are easily visualised for monitoring and management.

### Architecture
Airflow is largely consisted of 5 components as show below.

<img width="500" src="https://github.com/TravisH0301/learning/blob/master/images/airflow_architecture.jpg"></img>

- Scheduler: orchestrates the execution of tasks based on schedule or external triggers
- Work Queue: is used by scheduler to store tasks that are to be executed
- Worker: processes the tasks from the work queue, and records the outcomes
  - Note that works can run in parallel, yet, memory and processing power may be limited. Hence, it can trigger other frameworks such as Apache Spark to handle the heavy work
- Database: saves meta data such as credentials, connections, history, logs and configuration
- Web Interface: provides UI to the users for executing and monitoring the DAGs, and configuring Airflow

### Building Data Pipeline
#### Creating a DAG
A DAG can be defined in Python by giving its a name, a description, a start date and an interval.
    
    from airflow import DAG

    first_dag = DAG(
        'first dag', # DAG name
        description='This is a first DAG', # description
        start_date=datetime(2021, 1, 1), # start date
        schedule_interval='@daily') # interval

The schedule interval can be defined using the following formats:
- @once: Run a DAG once and then never again
- @hourly: Run the DAG every hour
- @daily: Run the DAG every day
- @weekly: Run the DAG every week
- @monthly: Run the DAG every month
- @yearly: Run the DAG every year
- None: Only run the DAG when the user initiates it

Note that if the scheduler misses executing a DAG, then it will execute the total number of missed interval executions. 

#### Creating Operator
An operator is a task in a DAG. There are many types of operators. The below example uses a Python operator which takes in a Python function as a task.

    from airflow import DAG
    from airflow.operators.python_operator import PythonOperator

    def hello_world():
        """
        This Python function is to be passed to the Python Operator.
        """
        print(“Hello World”)

    first_dag = DAG(
        'first dag', # DAG name
        description='This is a first DAG', # description
        start_date=datetime(2021, 1, 1), # start date
        schedule_interval='@daily') # interval
        
    task = PythonOperator(
        task_id=’hello_world’, # task id
        python_callable=hello_world, # python function to be called (executed)
        dag=first_dag) # DAG to include the operator (task)











