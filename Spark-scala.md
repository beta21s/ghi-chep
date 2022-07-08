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



**Convert RDD to DataFrame**

```
import spark.implicits._
val df = rdd2.toDF()
df.show(10, false)
```
