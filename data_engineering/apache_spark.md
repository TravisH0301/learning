# Apache Spark
- [Spark Ecosystem](#spark-ecosystem)
- [Spark's Distributed Execution](#sparks-distributed-execution)
  - [Deployment modes:](#deployment-modes)
  - [Distributed data and partitions](#distributed-data-and-partitions)

## Spark Ecosystem
Apache Spark is a unified engine for distributed data processing. 
Spark uses in-memory storage for computations, and the Spark ecosystem consists of the followings:

<img width=600px src='https://user-images.githubusercontent.com/46085656/190637030-ad9c6eea-755b-4b60-84c4-42a8e07cfa4c.png'>

- Spark Core & Spark SQL Engine: Underlying execution engine that schedules and dispatches tasks and coordinates input and output (I/O) operations
- Spark SQL & Dataframes + Datasets: SQL for interactive queries
- Spark streaming (Structured streaming): Enables processing real-time streaming data
- Machine learning library (MLlib): Machine learning capabilities and tools to build ML pipelines
- GraphX: Building and analysing graph-structured data

## Spark's Distributed Execution
<img width=600px src='https://user-images.githubusercontent.com/46085656/190648410-ca54e996-617d-49d0-b9fc-19e3deb6d4ea.png'>

- Spark driver:
  - part of the Spark application responsible for instantiating a SparkSession
  - communicates with the cluster manager
  - requests resources from the cluster manager to allocate them to Spark executors (JVMs)
  - transforms Spark operations into DAG computations, schedules them, and distribute them across Spark executors

- SparkSession:
  - a unified entry point to all Spark operations and data

- Cluster manager:
  - manages and allocates resources for the cluster of nodes that Spark application runs
  - 4 Cluster managers can be used; built-in standalone cluster manager, Apache Hadoop YARN, Apache Mesos and Kubernetes

- Spark executor:
  - runs on each worker node in the cluster
  - Communicates with the driver program to execute tasks

### Deployment modes:
  - Local:
    - Spark driver: runs on single JVM (single node)
    - Spark executor: runs on same JVM as the driver
    - Cluster manager: runs on the same host
  - Standalone:
    - Spark driver: can run on any node in the cluster
    - Spark executor: each node will execute its own executor JVM
    - Cluster manager: can be allocated arbitrarily to any host in the cluster
  - YARN (client):
    - Spark driver: runs on a client (not part of the cluster)
    - Spark executor: YARN's NodeManager's container
    - Cluster manager: YARN's Resource Manager works with YARN's Application Master to allocate the containers on NodeManagers for executors
  - YARN (cluster):
    - Spark driver: Runs with the YARN Application Master
    - Spark executor: Same as YARN client mode
    - Cluster manager: Same as YARN client mode
  - Kubernetes:
    - Spark driver: Runs in a Kubernentes pod
    - Spark executor: Each workers runs within its own pod
    - Cluster manager: Kubernetes Master

### Distributed data and partitions
<img width=600px src='https://user-images.githubusercontent.com/46085656/190647615-7a5ac5d0-9975-4251-a466-beae39b95df7.png'>

Data is distributed across the cluster as partitions. 
Spark treats each partition as a high-level logical data abstraction-as a DataFrame in memory. <br>
Perferably, a task is allocated to the executor for the data closest to the executor. Hence, this can minimise network bandwidth.
And each executor's core is assigned its own data partition.

## Spark Dataframes
### Create Dataframe using List
    # Define a list
    id_list = [1, 2, 3]
    
    # Create a dataframe
    ## Giving string as a schema
    df = spark.createDataframe(id_list, "int")  # parameter: data, schema
    
    ## Giving sql type as a schema
    from pyspark.sql.types import IntegerType
    df = spark.createDataframe(id_list, IntegerType())
    
### Create Multi Column Dataframe using List
    # Define a list
    user_list = [(1, "Scott"), (2, "David"), (3, "Tom")]
    
    # Create a dataframe
    """id & first_name are column names
    int & string are data types
    """
    df = spark.createDataframe(user_list, "id int, first_name string")
    
### Create Dataframe with List of Lists using Row
    # Define a list of lists
    user_list = [[1, "Scott"], [2, "David"], [3, "Tom"]]
     
    # Convert to a list or rows
    """Row(*args) takes in varying arguments = takes in multiple arguments as a tuple
    Row(*user) is used to expand the list (ex. [1, "Scott"]) to individual arguments
    and pass as the varying arguments.
    Hence, the function will have (1, "Scott") instead of ([1, "Scott"],).
    Note that * will work the same way with a tuple too.
    """
    from pyspark.sql import Row
    user_rows = [Row(*user) for user in user_list]
    
    # Create a dataframe using rows
    df = spark.createDataframe(user_rows, "id int, first_name string") 
    
### Create Dataframe with List of Dictionaries using Row
    # Define a list of dictionaries
    user_list = [
      {"id": 1, "first_name": "Scott"},
      {"id": 2, "first_name": "David"},
      {"id": #, "first_name": "Tom"}
    ]
    
    # Convert to a list of rows
    """Row(**args) takes in keyword arguments = takes in multiple key:value arguments as a dictionary
    ** is used to expand the dictionary as key arguments.
    """
    from pyspark.sql import Row
    user_rows = [Row(**user) for user in user_list]
    
    # Create a dataframe using rows
    df = spark.createDataframe(user_rows, "id int, first_name string")
    
### Specifying Schema using String


### Specifying Schema using List


### Specifying Schema using Spark Types
    
    
    
    

