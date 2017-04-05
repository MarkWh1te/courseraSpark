# Basic of Spark's RDD 
## RDDS
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

  find error log
```scala
val lastYearLogs:RDD[strings] = ...
val numdecDecErrorLogs = lastYearLogs.filter(lg=>lg.contains("2016-12")&&lg.contains("error")).count()
val firstLogsWithErrors = lastYearLogs.filter(_.contains("ERROR")).take(10)
```

4. Transformations on Two RDDS
  * union
  * intersection
  * subtract
  * cartesian 
  
5. other usefull RDD actions
  * takeSample
  * takeOrdered
  * saveAsTextFile
  * saveAsSequenceFile

## Evaluation in Spark

1. why spark good for data science
    1. in-memory computation 
    2. Transformations(defferred/lazy) and  actions(eagerness)

    3. most Data Science problem involve iteration
    
    4. iteration in Hadoop
    mapreduce => write into HDFS => mapreduce=> wrie into HDFS
    
    5. iteration in spark
      read => some iteration
      avoid all IO on disk
      
2. Logistic Regression
```scala
val points = sc.textFile(...).map(parsePoint)
var w = Vector.zeros(d)
for (i<-1 to numIterations){
    val gradient = points.map{
      p =>
      (1/(1+exp(-p.y * w.dot(p.x))))-1)* p.y*p.y
    }.reduce(_+_)
}
```  
3. cache() and persistence
```scala
val lastYearLog:RDD[String]=...
val logsWithErrors = lastYearLog.filer(_.contains("ERROR")).persist()
val firstLogsWithErrors = logsWithErrors.take(10)
val numErrors = logsWithErrors.count()
```
    * LR
 ```scala
val points = sc.textFile(...).map(parsePoint).persist()
var w = Vector.zeros(d)
for (i<-1 to numIterations){
  val gradient = points.map{
        p=> (1/(1+exp(-p.y*w.dot(p.x)))-1)*p.y*p.y 
  }.reduce(_+_)
  w -= alpha*gradient
}
```
    cache() memory level
    persist() storage level
    MEMORY_AND_DISK_SER  
    
4. Restating the benefits of laziness for large-scale Data

## Cluster Toplogy Matters!

1. example
```scala
case class Person(name:String,age:Int)
val people:RDD[Person]=
val first10 = people.take(10)
```
2. How spark jobs are executed
    * Driver program(Spark Context)
    * Cluster manager(yearn,mesos)
    * work Node(executer)
    * foreach is execute on work node (because it's side effect)




















































L  