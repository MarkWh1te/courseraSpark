# Reduction Operations & Distributed Key-value pairs

##  Reduction Operations

1. Reduction Operations,generally
    1. fold,reduce,aggregate
    2.  exmaple
```scala
case class Taco(kind:String,price:Double)
val tacoOrder = 
  List(
    Taco("c",2.21)
    Taco("b",6.15)
    Taco("a",1.23)
    Taco("s",3.28)
    Taco("t",7.89)
    )
val cost = tacoOrder.foldLeft(0.0)((sum,dtaco)=>sum+taco.price)
```
2.  Parallel Reduction Operations
    1. foldleft is not parallelizeble
    def foldLeft[B](z:B)(f:(B,A)=>B):B
    2.  fold  is parallelizable
    def fold (z:A)(f:(A,A)+>A):A 
    3. aggregate 
    def aggregate[B](z:=>B)(seqop:(B,A)=>B,combop(B,B)=>B):B
3. Reduction Operations on RDDs
|scala collection|spark|
|fold |fold|
-----------
|foldleft/foldright|----|
|reduce |reduce|
|aggregate|aggregate|
```scala
case calss wikipediapage(
  title:String,
  timestamp:string,
  text:String
  )
```
### Distributed Key-value Pairs(Pair RDDs)

1. In single-node Scala, Key-value pairs can be thought of as maps. 
    1. example(JSON)
    2. Pair RDDS
    3. Pari RDDS has special methodsd
        * def groupByKey()
        * def reduceByKey()
        * def join
    4. create a pair RDD
        val rdd= RDD[Wikipediapage] =  ....
        val pariRDd = rdd.map(page=>(page.title,page.txt))

### Some interesting Pair RDDs operations

1. Transformations(lazy)
    * groupByKey
```scala
def groupBy[K](f:A=>K):Map[K,Traversable[A]]
def groupByKey():RDD[(K,Iterable[V])]

groupedRDD.eventsRDD.groupByKey().collect().foreach(println)
```
    * reduceByKey
      combination of groupByKey and reduce-ing but more efficient
```scala
def reduceByKey(func:(V,V)=>V):Rdd[(K,V)]
```
    * mapValues
      thought of as a short-hand for:
```scala
rdd.map{case(x,y):(x,func(y)}
```  
    * keys
      return an RDD with th e keys of each tuple
```scala
val numUniqueVisits = visits.keys.distinct().count()
```  
    * join
    * leftOuterJoin/rightOuterJoin
2. Action(eagerness)
    * countByKey

### Join
1.  not works in Pair RDDs.
2.  two kinds of joins:
    1. Inner joins(join)
    2. Outer joins(leftOuterJoin,rigrightOuterJoin)
3. example
```scala
val abos  = sc.parallzelize(as)//RDD[(Int,(String,Abonnement))]
val locations = sc.parallzelize(ls)//RDD[(Int,String)]

def join[W](other:RDD[(K,W)]):RDD[(K,(V,W))]

val trackedCustomer = abos.join(locations) //RDD[(Int,((String,Abonnement),String))]
trackedCustomer.collect().foreach(println)
def leftOuterJoin[W](other:RDD[(K,W)]):RDD[(K,(V,Option[W]))]
def leftOuterJoin[W](other:RDD[(K,W)]):RDD[(K,(V,Option[W]))]
def rightOuterJoin[W](other:RDD[(K,W)]):RDD[(K,(Option[W],V))]
val abosWithOptionalLocations = abos.leftOuterJoin(locations)
val customerwithlocationDataand optionalAbos = abos.rightOuterJoin(llocations)
```
  
