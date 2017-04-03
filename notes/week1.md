# Basic of Spark's RDD ## RDDS
1. Basic APIs 
```scala
  abstract class RDD[T]{
    def map[U](f:T=>U):RDD[U] =
    def flatMap[U]
    def filter
    def reduce
    def fold
    def aggregate
  }
```
2. example:

count encyclopedia
```scala
val encyclopedia:RDD[string]
val result = encyclopedia.filter(paged=>page.contains("EPFL")).count()
```
count hello world
```scala
  val rdd = spark.textFile("hdfs://...")
  val count = rdd.flatMap(line=>line.split("")).map(word=>(word,1)).reduceByKey(_+_)
```

3. how to create an RDD

    1. transforming an existing RDD
  
    2. from a sparkcontext(or sparksession) object
  
        1. parallzelize : convert a local Scala collection to an RDD
        2. textFile : read a text file from HDFS or a local file system and return an RDD of string
    
## Transformations and Actions

1.  Transformations and Actions
    1.  Transformers: Return new collection as results (not single values) examples: map filter flatMa groupBy

    2. Accessors; Return single values as results (not collections)
    examples: reduce fold aggregate
    
2. Transformations: return new RDDS as results(lazy)
    Actions: Compute as results based on RDD,and either returned or saved to an external storage system (e.g., HDFS)(eagerness)
3.  example
```scala
val largeList:List[String]=....
val wrodsRdd = sc.parallzelize(largeList)
val lengthRdd = wordsRdd.map(_.length)
```
  Nothing
```scala
val totalChars = lengthRdd.reduce(_+_)
```
  now the thing happened







L  