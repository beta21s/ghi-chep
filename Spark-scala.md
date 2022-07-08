Setup ENV Scala run Spark using sbt

Edit file build.sbt and add to the end of the file and rebuid sbt
```
....
val sparkVersion = "3.2.1"
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion,
  "org.apache.spark" %% "spark-graphx" % sparkVersion
)
```

**Read file txt text**

```
val spark: SparkSession = SparkSession.builder()
  .master("local[*]")
  .appName("SparkByExamples.com")
  .getOrCreate()

val sc = spark.sparkContext
val rdd: RDD[String] = sc.textFile("/home/fit/Documents/selfjoin_30GB/file1")
val rdd2 = rdd.map(item => item.split(",")(0))
```

**Convert RDD to DataFrame**

```
import spark.implicits._
val df = rdd2.toDF()
df.show(10, false)
```

**Different RDD, DataFrame and Dataset in Spark  **

RDD is distributed collection of the data elements in Spark cluster and this not change.

DataFrame unlike RDD, it is like table in a relational database

Datasets it is an extension of DataFrame API that provides the function of the data in RDD.

**Spark scala wordcount**
```
val text = sc.textFile("mytextfile.txt") 
val rdd = text.flatMap(line => line.split(" "))
val rdd1 = rdd.map(word => (word,1)).reduceByKey(_+_)
val rdd1.collect.forearch(println) 
```

