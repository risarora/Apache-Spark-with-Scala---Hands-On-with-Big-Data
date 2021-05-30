```
import org.apache.spark.sql.types._
import org.apache.spark.storage.StorageLevel 
import scala.io.Source 
import scala.collection.mutable.HashMap 
import java.io.File 
import org.apache.spark.sql.Row 
import org.apache.spark.sql.types._ 
import scala.collection.mutable.ListBuffer 
import org.apache.spark.util.IntParam
import org.apache.spark.rdd.RDD 
import org.apache.spark.SparkContext 
import org.apache.spark.SparkContext._ 
import org.apache.spark.SparkConf 
import org.apache.spark.sql.SQLContext 
import org.apache.spark.rdd._ 

val sqlContext = new org.apache.spark.sql.SQLContext(sc)

import sqlContext.implicits._
import sqlContext._
```

```
scala> val schema = StructType(Array(
     | StructField("ID", IntegerType, true),
     | StructField("Name", StringType, true),
     | StructField("Age", IntegerType, true),
     | StructField("Nationality", StringType, true),
     | StructField("Potential", IntegerType, true),
     | StructField("Club", StringType, true),
     | StructField("Value", StringType, true),
     | StructField("Preferred Foot", StringType, true),
     | StructField("International Reputation", IntegerType, true),
     | StructField("Skill Moves", IntegerType, true),
     | StructField("Position", StringType, true),
     | StructField("Jersey Number", IntegerType, true),
     | StructField("Crossing", IntegerType, true),
     | StructField("Finishing", IntegerType, true),
     | StructField("HeadingAccuracy", IntegerType, true),
     | StructField("ShortPassing", IntegerType, true),
     | StructField("Volleys", IntegerType, true),
     | StructField("Dribbling", IntegerType, true),
     | StructField("Curve", IntegerType, true),
     | StructField("FKAccuracy", IntegerType, true),
     | StructField("LongPassing", IntegerType, true),
     | StructField("BallControl", IntegerType, true),
     | StructField("Acceleration", IntegerType, true),
     | StructField("SprintSpeed", IntegerType, true),
     | StructField("Agility", IntegerType, true),
     | StructField("Balance", IntegerType, true),
     | StructField("ShotPower", IntegerType, true),
     | StructField("Jumping", IntegerType, true),
     | StructField("Stamina", IntegerType,true))
     | )
schema: org.apache.spark.sql.types.StructType = StructType(StructField(ID,IntegerType,true), StructField(Name,StringType,true), StructField(Age,IntegerType,true), StructField(Nationality,StringType,true), StructField(Potential,IntegerType,true), StructField(Club,StringType,true), StructField(Value,StringType,true), StructField(Preferred Foot,StringType,true), StructField(International Reputation,IntegerType,true), StructField(Skill Moves,IntegerType,true), StructField(Position,StringType,true), StructField(Jersey Number,IntegerType,true), StructField(Crossing,IntegerType,true), StructField(Finishing,IntegerType,true), StructField(HeadingAccuracy,IntegerType,true), StructField(ShortPassing,IntegerType,true), StructField(Volleys,IntegerType,true), StructField(Dri...

scala> 
```
```
scala> val FIFAdf = spark.read.format("csv").option("header", true").load("data.csv")
<console>:1: error: ')' expected but string literal found.
       val FIFAdf = spark.read.format("csv").option("header", true").load("data.csv")
                                                                  ^
<console>:1: error: unclosed string literal
       val FIFAdf = spark.read.format("csv").option("header", true").load("data.csv")
                                                                                    ^

scala> 
```

```
scala> val FIFAdf = spark.read.format("csv").option("header", true).load("Footballer.csv")
FIFAdf: org.apache.spark.sql.DataFrame = [_c0: string, ID: string ... 87 more fields]

scala> FIFAdf.printSchema()
root
 |-- _c0: string (nullable = true)
 |-- ID: string (nullable = true)
 |-- Name: string (nullable = true)
 |-- Age: string (nullable = true)
 |-- Photo: string (nullable = true)
 |-- Nationality: string (nullable = true)
 |-- Flag: string (nullable = true)
 |-- Overall: string (nullable = true)
 |-- Potential: string (nullable = true)
 |-- Club: string (nullable = true)
 |-- Club Logo: string (nullable = true)
 |-- Value: string (nullable = true)
 |-- Wage: string (nullable = true)
 |-- Special: string (nullable = true)
 |-- Preferred Foot: string (nullable = true)
 |-- International Reputation: string (nullable = true)
 |-- Weak Foot: string (nullable = true)
 |-- Skill Moves: string (nullable = true)
 |-- Work Rate: string (nullable = true)
 |-- Body Type: string (nullable = true)
 |-- Real Face: string (nullable = true)
 |-- Position: string (nullable = true)
 |-- Jersey Number: string (nullable = true)
 |-- Joined: string (nullable = true)
 |-- Loaned From: string (nullable = true)
 |-- Contract Valid Until: string (nullable = true)
 |-- Height: string (nullable = true)
 |-- Weight: string (nullable = true)
 |-- LS: string (nullable = true)
 |-- ST: string (nullable = true)
 |-- RS: string (nullable = true)
 |-- LW: string (nullable = true)
 |-- LF: string (nullable = true)
 |-- CF: string (nullable = true)
 |-- RF: string (nullable = true)
 |-- RW: string (nullable = true)
 |-- LAM: string (nullable = true)
 |-- CAM: string (nullable = true)
 |-- RAM: string (nullable = true)
 |-- LM: string (nullable = true)
 |-- LCM: string (nullable = true)
 |-- CM: string (nullable = true)
 |-- RCM: string (nullable = true)
 |-- RM: string (nullable = true)
 |-- LWB: string (nullable = true)
 |-- LDM: string (nullable = true)
 |-- CDM: string (nullable = true)
 |-- RDM: string (nullable = true)
 |-- RWB: string (nullable = true)
 |-- LB: string (nullable = true)
 |-- LCB: string (nullable = true)
 |-- CB: string (nullable = true)
 |-- RCB: string (nullable = true)
 |-- RB: string (nullable = true)
 |-- Crossing: string (nullable = true)
 |-- Finishing: string (nullable = true)
 |-- HeadingAccuracy: string (nullable = true)
 |-- ShortPassing: string (nullable = true)
 |-- Volleys: string (nullable = true)
 |-- Dribbling: string (nullable = true)
 |-- Curve: string (nullable = true)
 |-- FKAccuracy: string (nullable = true)
 |-- LongPassing: string (nullable = true)
 |-- BallControl: string (nullable = true)
 |-- Acceleration: string (nullable = true)
 |-- SprintSpeed: string (nullable = true)
 |-- Agility: string (nullable = true)
 |-- Reactions: string (nullable = true)
 |-- Balance: string (nullable = true)
 |-- ShotPower: string (nullable = true)
 |-- Jumping: string (nullable = true)
 |-- Stamina: string (nullable = true)
 |-- Strength: string (nullable = true)
 |-- LongShots: string (nullable = true)
 |-- Aggression: string (nullable = true)
 |-- Interceptions: string (nullable = true)
 |-- Positioning: string (nullable = true)
 |-- Vision: string (nullable = true)
 |-- Penalties: string (nullable = true)
 |-- Composure: string (nullable = true)
 |-- Marking: string (nullable = true)
 |-- StandingTackle: string (nullable = true)
 |-- SlidingTackle: string (nullable = true)
 |-- GKDiving: string (nullable = true)
 |-- GKHandling: string (nullable = true)
 |-- GKKicking: string (nullable = true)
 |-- GKPositioning: string (nullable = true)
 |-- GKReflexes: string (nullable = true)
 |-- Release Clause: string (nullable = true)

```
### Count
```

scala> FIFAdf.count()
res16: Long = 18207
```
### Columns/Schema Name
```
scala> FIFAdf.columns.take(10).foreach(println)
_c0
ID
Name
Age
Photo
Nationality
Flag
Overall
Potential
Club
```
### Summary 
If you wish to look at the summary of a particular column in a DataFrame, we can apply to describe command. 
This command will give us the statistical summary of a particular selected column if nothing is specified, 
then it provides the statistical information of the DataFrame.

```
scala> FIFAdf.describe("Value").show
+-------+-----+
|summary|Value|
+-------+-----+
|  count|18207|
|   mean| null|
| stddev| null|
|    min|   €0|
|    max|  €9M|
+-------+-----+


scala> FIFAdf.describe("ShotPower").show
+-------+------------------+
|summary|         ShotPower|
+-------+------------------+
|  count|             18159|
|   mean| 55.46004735943609|
| stddev|17.237957966796195|
|    min|                10|
|    max|                95|
+-------+------------------+

```
### We shall find out the Nationality of a particular player by using the select command.

```
scala> FIFAdf.select("Name","Nationality").show
+-----------------+-----------+
|             Name|Nationality|
+-----------------+-----------+
|         L. Messi|  Argentina|
|Cristiano Ronaldo|   Portugal|
|        Neymar Jr|     Brazil|
|           De Gea|      Spain|
|     K. De Bruyne|    Belgium|
|        E. Hazard|    Belgium|
|        L. Modrić|    Croatia|
|        L. Suárez|    Uruguay|
|     Sergio Ramos|      Spain|
|         J. Oblak|   Slovenia|
|   R. Lewandowski|     Poland|
|         T. Kroos|    Germany|
|         D. Godín|    Uruguay|
|      David Silva|      Spain|
|         N. Kanté|     France|
|        P. Dybala|  Argentina|
|          H. Kane|    England|
|     A. Griezmann|     France|
|    M. ter Stegen|    Germany|
|      T. Courtois|    Belgium|
+-----------------+-----------+
only showing top 20 rows



scala> FIFAdf.select("Name","Club").distinct.show()
+--------------+--------------------+
|          Name|                Club|
+--------------+--------------------+
|   A. Di María| Paris Saint-Germain|
|       T. Horn|          1. FC Köln|
|    M. Dembélé|   Tottenham Hotspur|
|       Rafinha|        FC Barcelona|
|        Marlos|    Shakhtar Donetsk|
|         Bruma|          RB Leipzig|
|      S. Kalou|          Hertha BSC|
|    Diogo Jota|Wolverhampton Wan...|
|   W. Weghorst|       VfL Wolfsburg|
|   N. Füllkrug|         Hannover 96|
|     G. Donsah|             Bologna|
|        Sandro|       Real Sociedad|
|  M. Fernández|         Club Necaxa|
|      O. Şahan|         Trabzonspor|
|        J. Ibe|         Bournemouth|
|       C. Löwe|   Huddersfield Town|
|       B. Wood|         Hannover 96|
|José Rodríguez|     Fortuna Sittard|
|     M. Linnes|      Galatasaray SK|
|   O. Zahustel|        Sparta Praha|
+--------------+--------------------+
only showing top 20 rows

```
### View All clubs in Fifa 2019
```

scala> FIFAdf.select("Club").distinct.show()
+--------------------+
|                Club|
+--------------------+
|             Palermo|
|          Göztepe SK|
|CD Everton de Viñ...|
|     Shonan Bellmare|
|         Yeovil Town|
|          Sagan Tosu|
|  1. FC Union Berlin|
|               Carpi|
|           Puebla FC|
|  Argentinos Juniors|
|     SC Paderborn 07|
|       Karlsruher SC|
|     Cheltenham Town|
|         SC Freiburg|
|San Lorenzo de Al...|
|  SpVgg Unterhaching|
|Universidad Católica|
|         GFC Ajaccio|
|           FC Luzern|
|                 AIK|
+--------------------+
only showing top 20 rows

```

### Select and Filter 
```
scala> FIFAdf.select("Name","Age").filter(" Age < 30 ").show
+---------------+---+
|           Name|Age|
+---------------+---+
|      Neymar Jr| 26|
|         De Gea| 27|
|   K. De Bruyne| 27|
|      E. Hazard| 27|
|       J. Oblak| 25|
| R. Lewandowski| 29|
|       T. Kroos| 28|
|       N. Kanté| 27|
|      P. Dybala| 24|
|        H. Kane| 24|
|   A. Griezmann| 27|
|  M. ter Stegen| 26|
|    T. Courtois| 26|
|Sergio Busquets| 29|
|      K. Mbappé| 19|
|       M. Salah| 26|
|       Casemiro| 26|
|   J. Rodríguez| 26|
|     L. Insigne| 27|
|           Isco| 26|
+---------------+---+
only showing top 20 rows


scala> 
```
