## Create rdd with external data 
```
$ head ipl.csv
id,season,city,date,team1,team2,toss_winner,toss_decision,result,dl_applied,winner,win_by_runs,win_by_wickets,player_of_match,venue,umpire1,umpire2,umpire3
1,2017,Hyderabad,2017-04-05,Sunrisers Hyderabad,Royal Challengers Bangalore,Royal Challengers Bangalore,field,normal,0,Sunrisers Hyderabad,35,0,Yuvraj Singh,"Rajiv Gandhi International Stadium, Uppal",AY Dandekar,NJ Llong,
2,2017,Pune,2017-04-06,Mumbai Indians,Rising Pune Supergiant,Rising Pune Supergiant,field,normal,0,Rising Pune Supergiant,0,7,SPD Smith,Maharashtra Cricket Association Stadium,A Nand Kishore,S Ravi,
3,2017,Rajkot,2017-04-07,Gujarat Lions,Kolkata Knight Riders,Kolkata Knight Riders,field,normal,0,Kolkata Knight Riders,0,10,CA Lynn,Saurashtra Cricket Association Stadium,Nitin Menon,CK Nandan,
4,2017,Indore,2017-04-08,Rising Pune Supergiant,Kings XI Punjab,Kings XI Punjab,field,normal,0,Kings XI Punjab,0,6,GJ Maxwell,Holkar Cricket Stadium,AK Chaudhary,C Shamshuddin,
5,2017,Bangalore,2017-04-08,Royal Challengers Bangalore,Delhi Daredevils,Royal Challengers Bangalore,bat,normal,0,Royal Challengers Bangalore,15,0,KM Jadhav,M Chinnaswamy Stadium,,,
6,2017,Hyderabad,2017-04-09,Gujarat Lions,Sunrisers Hyderabad,Sunrisers Hyderabad,field,normal,0,Sunrisers Hyderabad,0,9,Rashid Khan,"Rajiv Gandhi International Stadium, Uppal",A Deshmukh,NJ Llong,
7,2017,Mumbai,2017-04-09,Kolkata Knight Riders,Mumbai Indians,Mumbai Indians,field,normal,0,Mumbai Indians,0,4,N Rana,Wankhede Stadium,Nitin Menon,CK Nandan,
8,2017,Indore,2017-04-10,Royal Challengers Bangalore,Kings XI Punjab,Royal Challengers Bangalore,bat,normal,0,Kings XI Punjab,0,8,AR Patel,Holkar Cricket Stadium,AK Chaudhary,C Shamshuddin,
9,2017,Pune,2017-04-11,Delhi Daredevils,Rising Pune Supergiant,Rising Pune Supergiant,field,normal,0,Delhi Daredevils,97,0,SV Samson,Maharashtra Cricket Association Stadium,AY Dandekar,S Ravi,
$ 


scala> val iplfile = sc.textFile("ipl.csv")
iplfile: org.apache.spark.rdd.RDD[String] = ipl.csv MapPartitionsRDD[8] at textFile at <console>:24
```

### Per City match Counter 

```
scala> val city = iplfile.map(_.split(",")(2))
city: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[10] at map at <console>:25

scala> city.collect.foreach(println)
city
Hyderabad
Pune

scala> 

scala> val cityCount = city.map(cityCount => (cityCount,1))
cityCount: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[11] at map at <console>:25

scala> 

scala> val cityMatchCounter=cityCount.reduceByKey((x,y)=> x+y).map(tup=>(tup._2,tup._1))sortByKey(false)
cityMatchCounter: org.apache.spark.rdd.RDD[(Int, String)] = ShuffledRDD[16] at sortByKey at <console>:25

scala> 


scala> cityMatchCounter.take(10).foreach(println)
(101,Mumbai)
(77,Kolkata)
(74,Delhi)
(66,Bangalore)
(64,Hyderabad)
(57,Chennai)
(47,Jaipur)
(46,Chandigarh)
(38,Pune)
(15,Durban)

scala> 

```

## Filter out all matched excluding Hyderabad 

```
scala> var flatRDD = iplfile.flatMap(line=>line.split("Hyderabad"))
flatRDD: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[17] at flatMap at <console>:25

scala> 


scala> flatRDD.take(10).foreach(println)
id,season,city,date,team1,team2,toss_winner,toss_decision,result,dl_applied,winner,win_by_runs,win_by_wickets,player_of_match,venue,umpire1,umpire2,umpire3
1,2017,
,2017-04-05,Sunrisers 
,Royal Challengers Bangalore,Royal Challengers Bangalore,field,normal,0,Sunrisers 
,35,0,Yuvraj Singh,"Rajiv Gandhi International Stadium, Uppal",AY Dandekar,NJ Llong,
2,2017,Pune,2017-04-06,Mumbai Indians,Rising Pune Supergiant,Rising Pune Supergiant,field,normal,0,Rising Pune Supergiant,0,7,SPD Smith,Maharashtra Cricket Association Stadium,A Nand Kishore,S Ravi,
3,2017,Rajkot,2017-04-07,Gujarat Lions,Kolkata Knight Riders,Kolkata Knight Riders,field,normal,0,Kolkata Knight Riders,0,10,CA Lynn,Saurashtra Cricket Association Stadium,Nitin Menon,CK Nandan,
4,2017,Indore,2017-04-08,Rising Pune Supergiant,Kings XI Punjab,Kings XI Punjab,field,normal,0,Kings XI Punjab,0,6,GJ Maxwell,Holkar Cricket Stadium,AK Chaudhary,C Shamshuddin,
5,2017,Bangalore,2017-04-08,Royal Challengers Bangalore,Delhi Daredevils,Royal Challengers Bangalore,bat,normal,0,Royal Challengers Bangalore,15,0,KM Jadhav,M Chinnaswamy Stadium,,,
6,2017,
scala> 


scala> var fi17 = iplfile.filter(line=>line.contains("2017"))
fi17: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[18] at filter at <console>:25

var fi17 = iplfile.filter(line=>line.contains("2017"))

fi17.collect.take(10).foreach(println)
```

## Union Transformatiion Man of the Match Count 
```

scala> var manoftheMatch=iplfile.map(_.split(",")(13))
manoftheMatch: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[20] at map at <console>:25

scala> manoftheMatch.collect.take(10).foreach(println)
player_of_match
Yuvraj Singh
SPD Smith
CA Lynn
GJ Maxwell
KM Jadhav
Rashid Khan
N Rana
AR Patel
SV Samson

scala> 
scala> var manoftheMatchCount=manoftheMatch.map(WINcount => (WINcount,1))
manoftheMatchCount: org.apache.spark.rdd.RDD[(String, Int)] = MapPartitionsRDD[21] at map at <console>:25

scala> var ManOTH=manoftheMatchCount.reduceByKey((x,y)=>x+y).map(tup=>(tup._2,tup._1))
ManOTH: org.apache.spark.rdd.RDD[(Int, String)] = MapPartitionsRDD[23] at map at <console>:25

scala> ManOTH.take(10).foreach(println)
(5,Yuvraj Singh)
(1,KK Cooper)
(2,GH Vihari)
(20,AB de Villiers)
(1,SW Billings)
(2,MM Sharma)
(1,R McLaren)
(2,Iqbal Abdulla)
(3,WP Saha)
(1,A Singh)

scala> 



# get the man of the match count per player in soted function

scala> var ManOTHSorted=manoftheMatchCount.reduceByKey((x,y)=>x+y).map(tup=>(tup._2,tup._1))sortByKey(false)
ManOTHSorted: org.apache.spark.rdd.RDD[(Int, String)] = ShuffledRDD[28] at sortByKey at <console>:25

scala> ManOTHSorted.take(10).foreach(println)
(21,CH Gayle)
(20,AB de Villiers)
(17,DA Warner)
(17,MS Dhoni)
(17,RG Sharma)
(16,YK Pathan)
(15,SR Watson)
(14,SK Raina)
(13,G Gambhir)
(12,MEK Hussey)


```

