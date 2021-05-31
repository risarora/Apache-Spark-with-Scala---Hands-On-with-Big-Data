
**Topic**   | RDDs |	Dataframes|	Datasets
---|---|---|---
Data Representation	| RDD is a distributed collection of data elements without any schema.	| It is also the distributed collection organized into the named columns | It is an extension of Dataframes with more features like type-safety and object-oriented interface.
Optimization | No in-built optimization engine for RDDs. Developers need to write the optimized code themselves. | It uses a catalyst optimizer for optimization. | It also uses a catalyst optimizer for optimization purposes.
Projection of Schema | Here, we need to define the schema manually. | It will automatically find out the schema of the dataset. | It will also automatically find out the schema of the dataset by using the SQL Engine.
Aggregation Operation | RDD is slower than both Dataframes and Datasets to perform simple operations like grouping the data. | It provides an easy API to perform aggregation operations. It performs aggregation faster than both RDDs and Datasets. | Dataset is faster than RDDs but a bit slower than Dataframes.
