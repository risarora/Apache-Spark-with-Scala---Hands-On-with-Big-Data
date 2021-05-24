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

In-Memory, i.e. data inside RDD is stored in memory as much (size) and long (time) as possible.

Immutable or Read-Only, i.e. it does not change once created and can only be transformed using transformations to new RDDs.

Lazy evaluated, i.e. the data inside RDD is not available or transformed until an action is executed that triggers the execution.

Cacheable, i.e. you can hold all the data in a persistent "storage" like memory (default and the most preferred) or disk (the least preferred due to access speed).

Parallel, i.e. process data in parallel.

Typed, i.e. values in a RDD have types, e.g. RDD[Long] or RDD[(Int, String)].

Partitioned, i.e. the data inside a RDD is partitioned (split into partitions) and then distributed across nodes in a cluster (one partition per JVM that may or may not correspond to a single node).

## Spark Architecture
![image](https://user-images.githubusercontent.com/4485129/119299413-8055b800-bc7c-11eb-93ba-8497aaa887f3.png)

## Demo

## Spark RDD
## Spark Applications
## Need For RDDs
## What are RDDs?
## Sources of RDDs
## Features of RDDs
## Creation of RDDs
## Operations Performed On RDDs
## Narrow Transformations
## Wide Transformations
## Actions
## RDDs Using Spark Pokemon Use-Case
## Spark DataFrame
## What is a DataFrame?
## Why Do We Need Dataframes?
## Features of DataFrames
## Sources Of DataFrames
## Creation Of DataFrame
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
