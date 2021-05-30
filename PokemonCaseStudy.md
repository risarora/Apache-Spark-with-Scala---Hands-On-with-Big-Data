![image](https://user-images.githubusercontent.com/4485129/119651723-eb50eb80-be42-11eb-8bb7-ff1fc1f17c08.png)

```

scala> val pokemonData=sc.textFile("./pokemon.csv");
pokemonData: org.apache.spark.rdd.RDD[String] = ./pokemon.csv MapPartitionsRDD[19] at textFile at <console>:24

scala> pokemonData.take(10).foreach(println);
#,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary
1,Bulbasaur,Grass,Poison,318,45,49,49,65,65,45,1,False
2,Ivysaur,Grass,Poison,405,60,62,63,80,80,60,1,False
3,Venusaur,Grass,Poison,525,80,82,83,100,100,80,1,False
3,VenusaurMega Venusaur,Grass,Poison,625,80,100,123,122,120,80,1,False
4,Charmander,Fire,,309,39,52,43,60,50,65,1,False
5,Charmeleon,Fire,,405,58,64,58,80,65,80,1,False
6,Charizard,Fire,Flying,534,78,84,78,109,85,100,1,False
6,CharizardMega Charizard X,Fire,Dragon,634,78,130,111,130,85,100,1,False
6,CharizardMega Charizard Y,Fire,Flying,634,78,104,78,159,115,100,1,False

scala> val head=pokemonData.first()
head: String = #,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary

scala> print(head)
#,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary
scala> 




```


### Find Number of Water and fire type Pokemon

```
scala> val WaterRDD = pokemonData.filter(line => line.contains("Water"))
WaterRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[22] at filter at <console>:25

scala> WaterRDD.take(10).foreach(println)
7,Squirtle,Water,,314,44,48,65,50,64,43,1,False
8,Wartortle,Water,,405,59,63,80,65,80,58,1,False
9,Blastoise,Water,,530,79,83,100,85,105,78,1,False
9,BlastoiseMega Blastoise,Water,,630,79,103,120,135,115,78,1,False
54,Psyduck,Water,,320,50,52,48,65,50,55,1,False
55,Golduck,Water,,500,80,82,78,95,80,85,1,False
60,Poliwag,Water,,300,40,50,40,40,40,90,1,False
61,Poliwhirl,Water,,385,65,65,65,50,50,90,1,False
62,Poliwrath,Water,Fighting,510,90,95,95,70,90,70,1,False
72,Tentacool,Water,Poison,335,40,40,35,50,100,70,1,False

scala> val FireRDD = pokemonData.filter(line => line.contains("Fire"))
FireRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[23] at filter at <console>:25

scala> FireRDD.take(10).foreach(println)
4,Charmander,Fire,,309,39,52,43,60,50,65,1,False
5,Charmeleon,Fire,,405,58,64,58,80,65,80,1,False
6,Charizard,Fire,Flying,534,78,84,78,109,85,100,1,False
6,CharizardMega Charizard X,Fire,Dragon,634,78,130,111,130,85,100,1,False
6,CharizardMega Charizard Y,Fire,Flying,634,78,104,78,159,115,100,1,False
37,Vulpix,Fire,,299,38,41,40,50,65,65,1,False
38,Ninetales,Fire,,505,73,76,75,81,100,100,1,False
58,Growlithe,Fire,,350,55,70,45,70,50,60,1,False
59,Arcanine,Fire,,555,90,110,80,100,80,95,1,False
77,Ponyta,Fire,,410,50,85,55,65,65,90,1,False

scala> WaterRDD.count()
res29: Long = 126

scala> FireRDD.count()
res30: Long = 64

```

#### Remove header
```
scala> val pokemonData=sc.textFile("./pokemon.csv");
pokemonData: org.apache.spark.rdd.RDD[String] = ./pokemon.csv MapPartitionsRDD[25] at textFile at <console>:24

scala> val header = pokemonData.first()
header: String = #,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary

scala> val NoHeader=pokemonData.filter(row => row != header)
NoHeader: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[26] at filter at <console>:27

scala> NoHeader.take(4).foreach(println)
1,Bulbasaur,Grass,Poison,318,45,49,49,65,65,45,1,False
2,Ivysaur,Grass,Poison,405,60,62,63,80,80,60,1,False
3,Venusaur,Grass,Poison,525,80,82,83,100,100,80,1,False
3,VenusaurMega Venusaur,Grass,Poison,625,80,100,123,122,120,80,1,False
4,Charmander,Fire,,309,39,52,43,60,50,65,1,False
5,Charmeleon,Fire,,405,58,64,58,80,65,80,1,False
6,Charizard,Fire,Flying,534,78,84,78,109,85,100,1,False
6,CharizardMega Charizard X,Fire,Dragon,634,78,130,111,130,85,100,1,False
6,CharizardMega Charizard Y,Fire,Flying,634,78,104,78,159,115,100,1,False
7,Squirtle,Water,,314,44,48,65,50,64,43,1,False

```


### Filter defence points of all Pokemon

```
                                                    ^
scala> val defenceList= NoHeader.map{x => x.split(',')}.map{ x=> (x(6).toDouble)}
defenceList: org.apache.spark.rdd.RDD[Double] = MapPartitionsRDD[32] at map at <console>:25

scala> defenceList.take(4).foreach(println)
49.0
62.0
82.0
100.0

scala> print(defenceList.max())
190.0
scala> 

```
