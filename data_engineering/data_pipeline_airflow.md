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

Note that if the scheduler misses executing a DAG, then it will execute the total number of missed interval executions. <br>
End date can be configured optionally, unless the DAG continues to run until it gets turned off manually.

#### Creating Operator
An operator is a task in a DAG. There are many types of operators. The below example uses a PythonOperator which takes in a Python function as a task.

    import logging
    from airflow import DAG
    from airflow.operators.python_operator import PythonOperator

    def hello_world():
        """
        This function logs a string.
        """
        logging.info(“Hello World”) # logging function allows Python to record data in the log - this will be shown in the Airflow DAG log
        
    def bye_world():
        """
        This function logs a string.
        """
        logging.info("Bye World")
        
    first_dag = DAG(
        'first dag', # DAG name
        description='This is a first DAG', # description
        start_date=datetime(2021, 1, 1), # start date
        schedule_interval='@daily') # interval
        
    task1 = PythonOperator(
        task_id='hello_world', # task id
        python_callable=hello_world, # python function to be called (executed)
        dag=first_dag) # DAG to include the operator (task)
        
    task2 = PythonOperator(
        task_id='bye_world',
        python_callable=bye_world, 
        dag=first_dag) 
        
    task1 >> task2 # this provides an edge information between task1 and task2, where task1 operates before task2
    # the below code also set an order of the tasks
    # task1.set_downstream(task2)

#### Hooks
Airflow provides hooks to connect with external systems and databases. <br>
Examples of the hook are HttpHook, PostgresHook (works with Redshift), MySqlHook, SlackHook, and etc.

Credentials are stored within Airflow's connection, hence, they are not stored in the code. <br>
Connections can be created for the hooks on Airflow UI: Admin > Connections > Create >  input Connection credential information <br>
In addition, key-value variable can be created on Airflow UI: Admin > Variables > Create > input Key and Value

    from airflow import DAG
    from airflow.hooks.postgres_hook import PostgresHook
    from airflow.operators.python_operator import PythonOperator

    def load():
        """
        Connects to S3 using stored connection credential and variables.
        """
        hook = S3Hook(aws_conn_id='aws_credentials') # connection
        bucket = Variable.get('s3_bucket') # variable
        prefix = Variable.get('s3_prefix') # variable
        logging.info(f"Listing Keys from {bucket}/{prefix}")
        keys = hook.list_keys(bucket, prefix=prefix)
        for key in keys:
            logging.info(f"- s3://{bucket}/{key}")

    dag = DAG(
        'lesson1.exercise4',
        start_date=datetime.datetime.now())

    list_task = PythonOperator(
        task_id="list_keys",
        python_callable=list_keys,
        dag=dag
    )

#### Task Context 
Airflow task can pass it's context information as a keyword argument to the function. This allows users to check more detailed task information 
such as execution date and time of the task. <br>
More templates can be found at [Airflow Documentation](https://airflow.apache.org/docs/apache-airflow/stable/templates-ref.html)

    from airflow import DAG
    from airflow.operators.python_operator import PythonOperator

    def display_date(*args, **kwargs):
        execution_date = kwargs['execution_date'] # method 1 of getting keyword argument context
        run_id = kwarg.get('run_id') # method 2 of getting keyword argument context
        print(f"Execution date: {execution_date}") # task execution date is displayed
        print(f"Run id: {run_id}") # task run id is displayed

    my_dag = DAG(...)
    task = PythonOperator(
        task_id='display date',
        python_callable=display_date,
        provide_context=True, # this configuration allows passing context info as kwargs
        dag=my_dag)








