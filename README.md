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
