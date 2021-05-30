# Apache-Spark-with-Scala---Hands-On-with-Big-Data


## Introduction to Apache Spark
## What is Spark?
## Spark Eco-System
## Why RDD?
### Properties of RDDs
* In memory Computations
* Fault Tolerant 
* Lazy Evaluation
* Schemaless structures
* Immutability
* Partitioning
* Persistance
* Coarse Graind Approach - Through Maps or Group by operations

### Ways to creates RDDs
1. Parallel Collections
* TBD
* TBD  https://towardsdatascience.com/3-methods-for-parallelization-in-spark-6a1a4333b473

2. From existing RDDs

```
var a1 = Array(1,2,3,4,5,6,7,8,9,10)
val newRDD = a1.map(data => (data*2))


a1: Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
newRDD: Array[Int] = Array(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)

```
3. External Datas

```
import org.apache.spark.SparkContext

val path="D:\\Cloud\\SparkScala\\Workspaces\\SparkScalaCourse1\\data\\1800.csv"
val sc = new SparkContext("local[*]", "HelloWorld")
val lines = sc.textFile(path)
val numLines = lines.count()
println("Hello world! The u.data file has " + numLines + " lines.")

```

```
path: String = D:\Cloud\SparkScala\Workspaces\SparkScalaCourse1\data\1800.csv
numLines: Long = 1825
Hello world! The u.data file has 1825 lines.

// https://www.tutorialkart.com/apache-spark/spark-parallelize-example/
// https://sparkbyexamples.com/spark/apache-spark-installation-on-windows/

```
## RDD Operations
![image](https://user-images.githubusercontent.com/4485129/119278753-f3433c80-bc44-11eb-95a8-350ba1e69e83.png)


Transformation -> new RDD
Action -> Result
### Ways pf Operations 
* Batch Mode
* Interative mode 
* Streaming Mode

## Apache Spark Architecture

![SparkArchitecture](https://user-images.githubusercontent.com/4485129/119298265-497ea280-bc7a-11eb-90c3-5e38bfc5ed63.PNG)


Spark has 
* Spark SQL
* Spark Streaming
* Spark MLib
* GraphX
* Spark R

Spark code can be written in 
* Python
* R
* Java
* Scala - Core spark language on which spark is written


## RDD

The original paper that gave birth to the concept of RDD is Resilient Distributed Datasets: [A Fault-Tolerant Abstraction for In-Memory Cluster Computing](https://www.cs.berkeley.edu/~matei/papers/2012/nsdi_spark.pdf) by Matei Zaharia, et al.

*Resilient Distributed Datasets (RDDs) are a distributed memory abstraction that lets programmers perform in-memory computations on large clusters in a fault-tolerant manner.*

Learning about RDD by its name:

* **Resilient**, i.e. fault-tolerant with the help of RDD lineage graph and so able to recompute missing or damaged partitions due to node failures.
* **Distributed** with data residing on multiple nodes in a cluster.
* **Dataset** is a collection of partitioned data with primitive values or values of values, e.g. tuples or other objects (that represent records of the data you work with).


Beside the above traits (that are directly embedded in the name of the data abstraction - RDD) it has the following additional traits:

* In-Memory, i.e. data inside RDD is stored in memory as much (size) and long (time) as possible.
* Immutable or Read-Only, i.e. it does not change once created and can only be transformed using transformations to new RDDs.
* Lazy evaluated, i.e. the data inside RDD is not available or transformed until an action is executed that triggers the execution.
* Cacheable, i.e. you can hold all the data in a persistent "storage" like memory (default and the most preferred) or disk (the least preferred due to access speed).
* Parallel, i.e. process data in parallel.
* Typed, i.e. values in a RDD have types, e.g. RDD[Long] or RDD[(Int, String)].
* Partitioned, i.e. the data inside a RDD is partitioned (split into partitions) and then distributed across nodes in a cluster (one partition per JVM that may or may not correspond to a single node).

## Spark Architecture
![image](https://user-images.githubusercontent.com/4485129/119299413-8055b800-bc7c-11eb-93ba-8497aaa887f3.png)

## Spark Architecture
![image](https://user-images.githubusercontent.com/4485129/119299413-8055b800-bc7c-11eb-93ba-8497aaa887f3.png)
<details>
### Spark Architecture Overview
Apache Spark follows a master/slave architecture with two main daemons and a cluster manager –

* Master Daemon – (Master/Driver Process)
* Worker Daemon –(Slave Process)

A spark cluster has a single Master and any number of Slaves/Workers. The driver and the executors run their individual Java processes and users can run them on the same horizontal spark cluster or on separate machines i.e. in a vertical spark cluster or in mixed machine configuration.
### Role of Driver in Spark Architecture
#### Spark Driver – Master Node of a Spark Application

 It is the central point and the entry point of the Spark Shell (Scala, Python, and R). The driver program runs the main () function of the application and is the place where the Spark Context is created. Spark Driver contains various components – DAGScheduler, TaskScheduler, BackendScheduler and BlockManager responsible for the translation of spark user code into actual spark jobs executed on the cluster.

* The driver program that runs on the master node of the spark cluster schedules the job execution and negotiates with the cluster manager.
* It translates the RDD’s into the execution graph and splits the graph into multiple stages.
* Driver stores the metadata about all the Resilient Distributed Databases and their partitions.
* Cockpits of Jobs and Tasks Execution -Driver program converts a user application into smaller execution units known as tasks. Tasks are then executed by the executors i.e. the worker processes which run individual tasks.
* Driver exposes the information about the running spark application through a Web UI at port 4040.

#### Role of Executor in Spark Architecture
Executor is a distributed agent responsible for the execution of tasks. Every spark applications has its own executor process. Executors usually run for the entire lifetime of a Spark application and this phenomenon is known as “Static Allocation of Executors”. However, users can also opt for dynamic allocations of executors wherein they can add or remove spark executors dynamically to match with the overall workload.

* Executor performs all the data processing.
* Reads from and Writes data to external sources.
* Executor stores the computation results data in-memory, cache or on hard disk drives.
* Interacts with the storage systems.

#### Role of Cluster Manager in Spark Architecture
An external service responsible for acquiring resources on the spark cluster and allocating them to a spark job. There are 3 different types of cluster managers a Spark application can leverage for the allocation and deallocation of various physical resources such as memory for client spark jobs, CPU memory, etc. Hadoop YARN, Apache Mesos or the simple standalone spark cluster manager either of them can be launched on-premise or in the cloud for a spark application to run.

Choosing a cluster manager for any spark application depends on the goals of the application because all cluster managers provide different set of scheduling capabilities. To get started with apache spark, the standalone cluster manager is the easiest one to use when developing a new spark application.


#### Understanding the Run Time Architecture of a Spark Application
###### What happens when a Spark Job is submitted?
When a client submits a spark user application code, the driver implicitly converts the code containing transformations and actions into a logical directed acyclic graph (DAG). At this stage, the driver program also performs certain optimizations like pipelining transformations and then it converts the logical DAG into physical execution plan with set of stages. After creating the physical execution plan, it creates small physical execution units referred to as tasks under each stage. Then tasks are bundled to be sent to the Spark Cluster.

The driver program then talks to the cluster manager and negotiates for resources. The cluster manager then launches executors on the worker nodes on behalf of the driver. At this point the driver sends tasks to the cluster manager based on data placement. Before executors begin execution, they register themselves with the driver program so that the driver has holistic view of all the executors. Now executors start executing the various tasks assigned by the driver program. At any point of time when the spark application is running, the driver program will monitor the set of executors that run. Driver program in the spark architecture also schedules future tasks based on data placement by tracking the location of cached data. When driver programs main () method exits or when it call the stop () method of the Spark Context, it will terminate all the executors and release the resources from the cluster manager.

The structure of a Spark program at higher level is - RDD's are created from the input data and new RDD's are derived from the existing RDD's using different transformations, after which an action is performed on the data. In any spark program, the DAG operations are created by default and whenever the driver runs the Spark DAG will be converted into a physical execution plan.

##### Launching a Spark Program
spark-submit is the single script used to submit a spark program and launches the application on the cluster. There are multiple options through which spark-submit script can connect with different cluster managers and control on the number of resources the application gets. For few cluster managers, spark-submit can run the driver within the cluster like in YARN on worker node whilst for others it runs only on local machines.

 </details>

## Spark RDD
## Spark Applications
## Need For RDDs
## What are RDDs?
## Sources of RDDs
## Features of RDDs
## Creation of RDDs
## Operations Performed On RDDs

## Narrow Transformations
* Narrow Transformation is applied to single partition of parent RDD
* as the data rquired is available on single partition of parent RDD

Example operations of narrow transformation are :-
* map()
* filter()
* flatmap()
* partition()
* mappartitions()

## Wide Transformations
* Wide Transformation is applied to multiple partitions of an RDD
* as the data required is present across on multiple partitions of parent RDD

Example operations of wide transformation are :-
* reduceby()
* union()

## Actions

* collect() 
* count()
* date()
* first()

## RDDs Using Spark Pokemon Use-Case
[Pokemon Case Study](./PokemonCaseStudy.md)

## Spark DataFrame
* Distributed collection of data 
* Organized in collection of rows under named columns
* Filter, Group and Aggregate Opeariions are performed
* Used with Spark SQL
* Constructed from many sources
* Hive, Hbase, cassandra, RDD  

### What is a DataFrame?
* R, python, scala and Java

### Why Do We Need Dataframes?

### Features of DataFrames
* Immutability
* fault tolereent
* Distributed
* Lazy Evalution

### Sources Of DataFrames

![image](https://user-images.githubusercontent.com/4485129/120116791-66016a00-c1a7-11eb-831c-5f212d5579eb.png)

### Creation Of a simple DataFrame Example 1
```
scala> import org.apache.spark.sql.Row
import org.apache.spark.sql.Row

scala> val empData = Seq(Row("Alice", 24), Row("Bob", 26))
empData: Seq[org.apache.spark.sql.Row] = List([Alice,24], [Bob,26])

```
#### Define the Schema
```
scala> import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType};
import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType}

scala> val empSchema = List(StructField("name", StringType, true), StructField("age", IntegerType, true))
empSchema: List[org.apache.spark.sql.types.StructField] = List(StructField(name,StringType,true), StructField(age,IntegerType,true))

```

#### Create the DataFrame
```
scala> val empDataFrame = spark.createDataFrame(spark.sparkContext.parallelize(empData), StructType(empSchema))
empDataFrame: org.apache.spark.sql.DataFrame = [name: string, age: int]

```
#### Print the dataframe and the Schema

```
scala> empDataFrame.printSchema()
root
 |-- name: string (nullable = true)
 |-- age: integer (nullable = true)


scala> empDataFrame.show()
+-----+---+
| name|age|
+-----+---+
|Alice| 24|
|  Bob| 26|
+-----+---+
scala> 
```
### Creation Of a simple DataFrame Example 2
```
scala> val data = Seq(
     |  Row("James","","Smith","1991-04-01","M",3000), 
     |  Row("Michael","Rose","","2000-05-19","M",4000), 
     |  Row("Robert","","Williams","1978-09-05","M",4000), 
     |  Row("Maria","Anne","Jones","1967-12-01","F",4000), 
     |  Row("Jen","Mary","Brown","1980-02-17","F",-1) 
     |  )
data: Seq[org.apache.spark.sql.Row] = List([James,,Smith,1991-04-01,M,3000], [Michael,Rose,,2000-05-19,M,4000], [Robert,,Williams,1978-09-05,M,4000], [Maria,Anne,Jones,1967-12-01,F,4000], [Jen,Mary,Brown,1980-02-17,F,-1])

scala> 

scala> val columns = List(
     | StructField("firstname",StringType,true),
     | StructField("middlename",StringType,true),
     | StructField("lastname",StringType,true),
     | StructField("dob",StringType,true),
     | StructField("gender",StringType,true),
     | StructField("salary",IntegerType,true))
columns: List[org.apache.spark.sql.types.StructField] = List(StructField(firstname,StringType,true), StructField(middlename,StringType,true), StructField(lastname,StringType,true), StructField(dob,StringType,true), StructField(gender,StringType,true), StructField(salary,IntegerType,true))

scala> val empDataFrame = spark.createDataFrame(spark.sparkContext.parallelize(data), StructType(columns))
empDataFrame: org.apache.spark.sql.DataFrame = [firstname: string, middlename: string ... 4 more fields]

scala> empDataFrame.printSchema()
root
 |-- firstname: string (nullable = true)
 |-- middlename: string (nullable = true)
 |-- lastname: string (nullable = true)
 |-- dob: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- salary: integer (nullable = true)


scala> empDataFrame.show()
+---------+----------+--------+----------+------+------+
|firstname|middlename|lastname|       dob|gender|salary|
+---------+----------+--------+----------+------+------+
|    James|          |   Smith|1991-04-01|     M|  3000|
|  Michael|      Rose|        |2000-05-19|     M|  4000|
|   Robert|          |Williams|1978-09-05|     M|  4000|
|    Maria|      Anne|   Jones|1967-12-01|     F|  4000|
|      Jen|      Mary|   Brown|1980-02-17|     F|    -1|
+---------+----------+--------+----------+------+------+

```

### Data Frames Example
#### Foot Ball Data analysis
#### GOT Data analysis

## Spark SQL
## Why Spark SQL?
## Spark SQL Advantages Over Hive 
## Spark SQL Success Story 
## Spark SQL Features 
## Spark SQL Architecture
## Spark SQL Libraries
## Querying Using Spark SQL
## Adding Schema To RDDs
## Hive Tables
## Use Case: Stock Market Analysis with Spark SQL
## Spark Streaming 
## What is Streaming?
## Spark Streaming Overview
## Spark Streaming workflow
## Streaming Fundamentals
## DStream
## Input DStreams
## Transformations on DStreams
## DStreams Window
## Caching/Persistence
## Accumulators
## Broadcast Variables
## Checkpoints
## Use-Case Twitter Sentiment Analysis
## Spark MLlib
## MLlib Techniques
## Demo
## Use Case: Earthquake Detection Using Spark 
## Visualizing Result
## Spark GraphX
## Basics of Graph
## Types of Graph
## GraphX
## Property Graph
## Creating & Transforming Property Graph
## Graph Builder
## Vertex RDD
## Edge RDD
## Graph Operators
## GraphX Demo
## Graph Algorithms
## PageRank
## Connected Components
## Triangle Counting 
## Spark GraphX Demo
## MapReduce vs Spark
## Kafka with Spark Streaming
## Messaging System
## Kafka Components
## Kafka Cluster
## Demo
## Kafka  Spark Streaming Demo
## PySpark Tutorial
## PySpark Installation
## Spark Interview Questions

https://stackoverflow.com/questions/32621990/what-are-workers-executors-cores-in-spark-standalone-cluster
https://stackoverflow.com/questions/63914667/what-is-the-difference-between-driver-and-application-manager-in-spark

https://github.com/srinathkr07/IPL-Data-Analysis/blob/master/matches.csv
https://www.edureka.co/blog/rdd-using-spark/
