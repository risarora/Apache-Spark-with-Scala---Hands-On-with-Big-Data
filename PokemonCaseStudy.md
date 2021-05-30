![image](https://user-images.githubusercontent.com/4485129/119651723-eb50eb80-be42-11eb-8bb7-ff1fc1f17c08.png)

```

scala> val pokemonData=sc.textFile("./pokemon.csv");
pokemonData: org.apache.spark.rdd.RDD[String] = ./pokemon.csv MapPartitionsRDD[1] at textFile at <console>:24

scala> pokemonData.take(10).foreach(println);
Identifier,Name,Dark Threshold,Type,Secondary Type,Evolutions
1,bulbasaur,0.61604,grass,poison,ivysaur
2,ivysaur,0.462721,grass,poison,venasaur
3,venusaur,0.5451,grass,poison,
4,charmander,0.724984,fire,,charmeleon
5,charmeleon,0.491232,fire,,charizard
6,charizard,0.601881,fire,flying,
7,squirtle,0.708672,water,,wartortle
8,wartortle,0.748101,water,,blastoise
9,blastoise,0.64938,water,,

scala> val head=pokemonData.first()
head: String = Identifier,Name,Dark Threshold,Type,Secondary Type,Evolutions

scala> print(head)
Identifier,Name,Dark Threshold,Type,Secondary Type,Evolutions
scala> 



```


### Find Number of Water and fire type Pokemon

```
                                         ^

scala> val WaterRDD = pokemonData.filter(line => line.contains("water"))
WaterRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[4] at filter at <console>:25

scala> WaterRDD.collect().foreach(println)
7,squirtle,0.708672,water,,wartortle
8,wartortle,0.748101,water,,blastoise
9,blastoise,0.64938,water,,
54,psyduck,0.798752,water,,golduck



scala> val FireRDD = pokemonData.filter(line => line.contains("fire"))
FireRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[5] at filter at <console>:25

scala> FireRDD.collect().foreach(println)
4,charmander,0.724984,fire,,charmeleon
5,charmeleon,0.491232,fire,,charizard
6,charizard,0.601881,fire,flying,
37,vulpix,0.555764,fire,,ninetales

scala> WaterRDD.count()
res8: Long = 117

scala> FireRDD.count()
res9: Long = 55


```
