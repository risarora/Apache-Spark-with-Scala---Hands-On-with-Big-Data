![image](https://user-images.githubusercontent.com/4485129/119651723-eb50eb80-be42-11eb-8bb7-ff1fc1f17c08.png)

```

scala> val pokemonData=sc.textFile("./pokemon.csv");
pokemonData: org.apache.spark.rdd.RDD[String] = ./pokemon.csv MapPartitionsRDD[19] at textFile at <console>:24

scala> pokemonData.take(4).foreach(println);
#,Name,Type 1,Type 2,Total,HP,Attack,Defense,Sp. Atk,Sp. Def,Speed,Generation,Legendary
1,Bulbasaur,Grass,Poison,318,45,49,49,65,65,45,1,False
2,Ivysaur,Grass,Poison,405,60,62,63,80,80,60,1,False
3,Venusaur,Grass,Poison,525,80,82,83,100,100,80,1,False

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

scala> WaterRDD.take(4).foreach(println)
7,Squirtle,Water,,314,44,48,65,50,64,43,1,False
8,Wartortle,Water,,405,59,63,80,65,80,58,1,False
9,Blastoise,Water,,530,79,83,100,85,105,78,1,False
9,BlastoiseMega Blastoise,Water,,630,79,103,120,135,115,78,1,False

scala> val FireRDD = pokemonData.filter(line => line.contains("Fire"))
FireRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[23] at filter at <console>:25

scala> FireRDD.take(4).foreach(println)
4,Charmander,Fire,,309,39,52,43,60,50,65,1,False
5,Charmeleon,Fire,,405,58,64,58,80,65,80,1,False
6,Charizard,Fire,Flying,534,78,84,78,109,85,100,1,False
6,CharizardMega Charizard X,Fire,Dragon,634,78,130,111,130,85,100,1,False

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

```


### Filter defence points of all Pokemon

```
                                                    ^
scala> val defenceList= NoHeader.map{x => x.split(',')}.map{ x=> (x(7).toDouble)}
defenceList: org.apache.spark.rdd.RDD[Double] = MapPartitionsRDD[40] at map at <console>:25

scala> defenceList.take(4).foreach(println)
49.0
63.0
83.0
123.0

scala> println(defenceList.max())
230.0

scala> 

```

### Filter Pokemon Names with Max Defence
```
scala> val defenceWithPokemonName= NoHeader.map{x => x.split(',')}.map{ x=> (x(7).toDouble,x(1))}
defenceWithPokemonName: org.apache.spark.rdd.RDD[(Double, String)] = MapPartitionsRDD[48] at map at <console>:25

scala> defenceWithPokemonName.take(4).foreach(println)
(49.0,Bulbasaur)
(63.0,Ivysaur)
(83.0,Venusaur)
(123.0,VenusaurMega Venusaur)

scala> val MaxDefencePokemon = defenceWithPokemonName.groupByKey.takeOrdered(1)(Ordering[Double].reverse.on(_._1))
MaxDefencePokemon: Array[(Double, Iterable[String])] = Array((230.0,CompactBuffer(SteelixMega Steelix, Shuckle, AggronMega Aggron)))

scala> MaxDefencePokemon.take(4).foreach(println)
(230.0,CompactBuffer(SteelixMega Steelix, Shuckle, AggronMega Aggron))

scala> 

```

#### get Pokemons with Minimum Defence
```
scala> val defenceList= NoHeader.map{x => x.split(',')}.map{ x=> (x(7).toDouble)}
defenceList: org.apache.spark.rdd.RDD[Double] = MapPartitionsRDD[69] at map at <console>:25

scala> var minDefenceList= defenceList.distinct.sortBy(x=> x.toDouble,true,1)
minDefenceList: org.apache.spark.rdd.RDD[Double] = MapPartitionsRDD[75] at sortBy at <console>:25

scala> minDefenceList.take(4).foreach(println)
5.0
10.0
15.0
20.0

scala> 
```

### Get  names  of Pokemins with Min Defence 
```
scala> val defenceWithPokemonName= NoHeader.map{x => x.split(',')}.map{ x=> (x(7).toDouble,x(1))}
defenceWithPokemonName: org.apache.spark.rdd.RDD[(Double, String)] = MapPartitionsRDD[81] at map at <console>:25

scala> defenceWithPokemonName.take(4).foreach(println)
(49.0,Bulbasaur)
(63.0,Ivysaur)
(83.0,Venusaur)
(123.0,VenusaurMega Venusaur)

scala> val MinDefencePokemon = defenceWithPokemonName.groupByKey.takeOrdered(1)(Ordering[Double].on(_._1))
MinDefencePokemon: Array[(Double, Iterable[String])] = Array((5.0,CompactBuffer(Chansey, Happiny)))

scala> MinDefencePokemon.take(4).foreach(println)
(5.0,CompactBuffer(Chansey, Happiny))

scala> 

```
