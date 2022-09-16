# Apache Spark

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
<img width=600px src='https://user-images.githubusercontent.com/46085656/190640538-e2d8b71f-212d-49f6-99df-9614a6610624.png'>

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





