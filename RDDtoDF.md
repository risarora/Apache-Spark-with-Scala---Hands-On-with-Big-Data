```

scala> var fligh2011 =sc.textFile("./2011-summary.csv")
fligh2011: org.apache.spark.rdd.RDD[String] = ./2011-summary.csv MapPartitionsRDD[1] at textFile at <console>:24

scala> fligh2011.take(10).foreach(println)
DEST_COUNTRY_NAME,ORIGIN_COUNTRY_NAME,count
United States,Saint Martin,2
United States,Guinea,2
United States,Croatia,1
United States,Romania,3
United States,Ireland,268
Egypt,United States,13
United States,India,76
United States,Singapore,24
United States,Grenada,59

scala> var fligh2011HEAD=fligh2011.first()
fligh2011HEAD: String = DEST_COUNTRY_NAME,ORIGIN_COUNTRY_NAME,count

scala> var fligh2011minusHead = fligh2011.filter( x => x != fligh2011HEAD)
fligh2011minusHead: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[2] at filter at <console>:27

scala> 

scala> val sqlContext = new org.apache.spark.sql.SQLContext(sc)
warning: there was one deprecation warning (since 2.0.0); for details, enable `:setting -deprecation' or `:replay -deprecation'
sqlContext: org.apache.spark.sql.SQLContext = org.apache.spark.sql.SQLContext@1edeebc9

scala> 

scala> import sqlContext.implicits._
import sqlContext.implicits._

scala> import sqlContext._
import sqlContext._

scala> 

scala> var fligh2011df = fligh2011minusHead.toDF(fligh2011HEAD)
fligh2011df: org.apache.spark.sql.DataFrame = [DEST_COUNTRY_NAME,ORIGIN_COUNTRY_NAME,count: string]

scala> fligh2011df.take(10).foreach(println)
[United States,Saint Martin,2]
[United States,Guinea,2]
[United States,Croatia,1]
[United States,Romania,3]
[United States,Ireland,268]
[Egypt,United States,13]
[United States,India,76]
[United States,Singapore,24]
[United States,Grenada,59]
[Costa Rica,United States,494]

scala> fligh2011df.printSchema()
root
 |-- DEST_COUNTRY_NAME,ORIGIN_COUNTRY_NAME,count: string (nullable = true)


scala> import org.apache.spark.sql.types.StringType
import org.apache.spark.sql.types.StringType

scala> import org.apache.spark.sql.types.IntegerType
import org.apache.spark.sql.types.IntegerType

scala> import org.apache.spark.sql.types.StructType
import org.apache.spark.sql.types.StructType

scala> 

scala> val schema = (new StructType()
     |         .add("DEST_COUNTRY_NAME",StringType,true)
     |         .add("ORIGIN_COUNTRY_NAME",StringType,true)
     |         .add("count",IntegerType,true))
schema: org.apache.spark.sql.types.StructType = StructType(StructField(DEST_COUNTRY_NAME,StringType,true), StructField(ORIGIN_COUNTRY_NAME,StringType,true), StructField(count,IntegerType,true))

scala> 

scala> val flightData2011 = (spark
     |     .read
     |     .option("header", "true")
     |     .schema(schema)
     |     .csv("./2011-summary.csv"))
flightData2011: org.apache.spark.sql.DataFrame = [DEST_COUNTRY_NAME: string, ORIGIN_COUNTRY_NAME: string ... 1 more field]

scala> 

scala> flightData2011.sort("count").explain()
== Physical Plan ==
*(1) Sort [count#11 ASC NULLS FIRST], true, 0
+- Exchange rangepartitioning(count#11 ASC NULLS FIRST, 200), ENSURE_REQUIREMENTS, [id=#26]
   +- FileScan csv [DEST_COUNTRY_NAME#9,ORIGIN_COUNTRY_NAME#10,count#11] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex[file:/Users/rarora17/Desktop/project/airline/2011-summary.csv], PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,ORIGIN_COUNTRY_NAME:string,count:int>



scala> flightData2011.sort("count").take(10).foreach(println)
[United States,Malaysia,1]
[Libya,United States,1]
[The Gambia,United States,1]
[United States,Algeria,1]
[United States,Georgia,1]
[United States,Brunei,1]
[Malta,United States,1]
[Belarus,United States,1]
[United States,Cyprus,1]
[United States,Lithuania,1]

scala> 

scala> spark.conf.set("spark.sql.shuffle.partitions", "5")

scala> val dataFrameWay = flightData2011.groupBy('DEST_COUNTRY_NAME).count()
dataFrameWay: org.apache.spark.sql.DataFrame = [DEST_COUNTRY_NAME: string, count: bigint]

scala> dataFrameWay.take(10).foreach(println)
[Bolivia,1]
[Turks and Caicos Islands,1]
[Pakistan,1]
[Marshall Islands,1]
[Suriname,1]
[Panama,1]
[New Zealand,1]
[Ireland,1]
[Malaysia,1]
[Belarus,1]

scala> 

scala> (flightData2011.groupBy("DEST_COUNTRY_NAME").count()
     |   .filter($"count" >= 2)
     |   .show())
+-----------------+-----+
|DEST_COUNTRY_NAME|count|
+-----------------+-----+
|    United States|  127|
+-----------------+-----+


scala> 

scala> (flightData2011.groupBy("DEST_COUNTRY_NAME").count().take(10).foreach(println))
[Bolivia,1]
[Turks and Caicos Islands,1]
[Pakistan,1]
[Marshall Islands,1]
[Suriname,1]
[Panama,1]
[New Zealand,1]
[Ireland,1]
[Malaysia,1]
[Belarus,1]

scala> 

```

#### References
* https://stackoverflow.com/questions/29383578/how-to-convert-rdd-object-to-dataframe-in-spark
