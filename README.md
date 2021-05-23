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

## Yahoo Use-Case
## Apache Spark Architecture
## RDD
## Spark Architecture
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
