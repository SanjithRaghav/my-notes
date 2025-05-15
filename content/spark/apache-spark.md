# Apache Spark

## Data LakeHouse:
* One plataform that combines the best features of both 
    * <mark>data warehouse </mark>(Which is used to store structured data and prominently used for analytical purpose)  
    * <mark>data Lake</mark>(is used for storing unstructured data, used prominently by data scientists to train ml models in it).

## File Formats:
### Column Based:
* Data is stored as columns.Egs:Parquet, Delta
* it provides other special features like statistics, compression,Meta data and push down predicate 
>push down predicate is a query optimization technique that enhances query performance by filtering data as early as possible in the execution pipeline
### Row Based:
* Data is stored as rows, egs: csv

## Databricks commands
run any python notebook -> ` %run location/of/the/notebook`

to specify a language for a cell in a notebook -> `%python,%scala,%sql,%md,%sh`

to run a filesystem search -> `%fs ls /databricks-datasets`

to see what is there in a file -> `%fs head /readme.md`
to show all the commands available in fs -> `%fs `

`%fs`  is same as using `dbutils.fs.`


## Apache Spark Core
#### Transformations : 
 * Manipulations done to Dataframes that returns a new dataframe
 * transformations are lazy, It will not start until an action statement is called.
#### Actions :
* It is a method that triggers Spark job.
```python
df.count()
df.show()
df.collect()
display(df)
```
#### Creating a Spark Context
``` 
spark=sparkSession.builder.appName("app").master(local[*]).getOrCreate()
```
## Data Reading and Writing
### Loading data:
**reading a paruet file:**
```python
#reading parquet files
spark.read.parquet("location")
```

**reading a csv file**
```python
#reading a csv file with schema(fastest)
schema=StructType(StructField("Name",StringType(),True))
#new way of defining types: 
ddlschema="name string,age int,date timestamp"
spark.read.schema(schema).option("sep","\t").option("header",True).csv("file location")

#We can also implicitly infer Schema (slower,because it has to read the data to inferSchema)
spark.read.option("inferSchema",True).option("header",True).csv("file location")

```
>StructType is used to define schema in pyspark, it is a collection of Field objects. StructField(name,datatypeFunc,isNullable).


**reading a json file**
```python
spark.read.option("inferSchema",True).json("location")
```

### Writing Data:
> writing data into file storage will create a file for each partition, while reading it again, we can specify the folder location instead of the specific file.

**writing data into a file**
```python
df.write.option("compression","snappy").mode("overwrite").parquet("location")
or 
df.write.format("csv").mode("overwrite").save("location")
##different modes -> append,overwrite  , fileformats-> csv,parquet etc
```
**writing data into a sql tables**
```python
df.write.mode("overwrite").saveAsTable("TableName")
##different modes -> append,overwrite  , fileformats-> csv,parquet,delta etc
```


## DataFrame and Columns

there are three ways of referring to column:
```python
df.devices
df["devices"]
col("devices")

#to access the child column in a struct
df.devices.os
df["devices"]["os"]
col("devices.os")
```

***Functions to remember***:
* select
* selectExpr
* alias
* cast
* drop
    * `dropDuplicates([cols..])`
    * `dropna(how="any"),dropna(how="all"),dropna(subset=[cols..])`
* isNotNull/isNull
* distinct
* filter or where
* limit
* union
* unionByName 
    * `df.unionByName(df2,allowMisssingColumn=True)`
* sort
    * `df.sort(col("Name").asc(),col("someOtherCol").desc()) `
    * rand() -> returns a random column with independent and identically distruted samples uniformly distributed(0,1) ```df.orderBy(rand())```
* join
    * ```df.join(df2,condition or colname,"typeOfJoin")```
    condition or name of the column if both the tables has same columns



## Aggregations
>Always Aggregations occur in two stages one is local aggregation then global aggregation as it is a wide transformation.

|  Method | Description |
| --------| ------------|
| approx_count_distinct | functionality is same as the name suggests, used when the data is huge, it is exponentialy faster than the normal count distinct |
| collect_list | returns a list for each group by key |
| collect_set | returns a list of distinct values |
| sum Distinct | returns sum of distinct values in a column |
| corr | returns pearsons correlation coefficient for two columns.

## Datetime functions

converting unix time to normal timestamp
```python
df.withcolumn("ts",(col("unix")/1e6).cast("timestamp"))
```

### Functions:
**DateFormat**
```python
#date_format(col,"dd-MM-yyyy,hh:mm:ss:SSSS")
time=nyc.select(F.date_format(F.col("tpep_pickup_datetime"),"yyyy-MM-dd").alias("timestamp_Format"))
```
**DateDiff**
```python
#date_diff(end, start) -> in days
df.withColumn("diff_in_days", datediff(col("current_date"), col("date")))

#months_between(end,start)
df.withColumn("monthsInbetween", months_between(col("current_date"), col("date")))

#date difference in seconds -> We use the unix_timestamp() function in Spark SQL to convert Date/Datetime into seconds and then calculate the difference between dates in terms of seconds.
#unix_timestamp(timestamp, TimestampFormat)

withColumn("seconds_between", unix_timestamp(col("current_date")) - unix_timestamp(col("date").cast("Date")))


```
**DateAdd**
```python
date_add(start: ColumnOrName, days: Union[ColumnOrName, int])
```


## String Functions
* split
```python
df.split(col,delimiter,lengthOfResult)
df.select(split(df.s, '[ABC]', 2).alias('s')).collect()
# Output: [Row(s=['one', 'twoBthreeC'])]

df.select(split(df.s, '[ABC]', 2).alias('s').getItem(1)).collect()
# Output: 'twoBthreeC'
```
* regexp_extract
* regexp_replace
* ltrim(removes leading space in the given string) 
* lower
* concat

## Complex DataTypes
**Array** ->  A collection of values having same datatype ```ArrayType(StringType())```

**Map** -> Set of key-value pairs 
```MapType(StringType(), StringType())```


**Struct** -> Ordered,Fixed collection of columns of any datatype.
```StructType(StructField("name",StringType(),True),StructField("age",IntegerType(),True)...)```

### Collection Functions
| Method | Description |
|--------|-------------|
|array_contains | returns null if the array is empty, true if it contains the value , and false otherwise |
| explode | Converts each value in the array or map into a row (Maps will be Exploded as key and value pairs resulting <mark> two columns </mark> ) ```explode("FruitColors").alias("Fruit", "Color")```
|element_at | returns the element at the given index for an array <mark>(based on index 1 and not 0)</mark> or returns the value for the given key if it is a map |

### Functions for Handling Null

```python
df.na.drop() #drops any rows containing null value
df.na.fill({"colName":value,"colName":value})

```

## User Defiened functions
* with Python , Inter Process Communication passes each row from JVM to python objects, then executes the UDF, 
* and again sent back to the JVM through inter process communication 
* this overhead makes python UDFs slower than Scala UDFs
* there are three ways of writing UDFs in python -> vanilla UDF, decorator UDF and ***vectorized pandas***(best way bcos faster serialization and block transfer instead of row by row)
```python
def say_hello(name:str)->str:
    return f'Hello {name}'
my_udf=udf(lambda name:say_hello(name),StringType())
display df.withColumn("greeting",my_udf("name"))
```
```python
@udf(returnType=Stringtype())
def say_hello(name:str)->str:
    return f'Hello {name}'
display df.withColumn("greeting",say_hello("name"))
```

```python
import pandas as pd
from pyspark.sql.functions import pandas_udf

@pandas_udf(returnType=StringType())
def udf(email:pd.Series) -> pd.Series:
    return email.str[0]
display df.select(udf("email"))

```

## Spark architecture and performance

### Transformations
Narrow:
* data is not exchanged between partitions, all the execution is done inside the partition.
* filter,select

Wide:
* data is shuffled between partitions,
* aggregations, groupby, sort

### Shuffle
Sort phase
* Spark sorts the data in partition in order to group the data that has the same key.
* it partitions the sorted into a set of partitions, this number of partitions depends on the number of reducer tasks.
* then the partitions are serialized and written in the disk.
* serialized data are sent to the reducers tasks over network.

Reducer phase
* Spark deserialized the data in each partition and coverts it back to original format.
* It groups the data that has same keys
* performs aggregation on each group and outputs the final result.

#### Shuffle optimizations

* Configure the number of map and reduce tasks based on the size of your data and the resources available in your cluster.
* Use the right data serialization format to reduce the size of data written to disk during shuffle.
* Avoid shuffling large datasets across the network, as this can lead to network congestion and slow performance.
* Use efficient data structures for your keys and values, such as primitive types or compact binary formats, to reduce memory usage and improve performance.
* Monitor the shuffle metrics to identify any performance bottlenecks and optimize accordingly.


### Spark Execution Plans:

#### 1.Unresolved Logical Plan:
* first plan created by spark.

#### 2.Resolved Logical Plan:
* Spark creates a resolved logical plan by checking if all the columns are present in the table , casts the columns into required datatype for execution.

#### 3. Optimized logical plan:
* Spark optimizes the logical plan with its catalyst optimizer.it does optimizations like changing the order of transformations(filtering before join is better than join - filter ,order of joining tables) 

#### 4. Physical Plan:
* Physical plan is made and optimized solely based on the execution strategy. it chooses join strategy, data skipping , predicate push down.

#### 5. Whole Stage Code generation:
* Finally cost based optimization is done on the physical plan, which is then converted to java byte code and sent to the executors.


### Adaptive Query Execution
* It optimizes thenumber of partitions and improves performace, 
* it has a little bit of overhead.

### Predicate Pushdown
* Predicate push down is when the data source actively limits the data fed to the spark reader via select/filter/where.
* cast functions cannot be pushed down So use it wisely.

### Caching
* dont cache the data unless you are sure that the data will be reused multiple times.
* Manually evict the cache when not needed. ```upersist()```

### Number of partitions:
* default shuffle partition is 200
* Initial number of partitions are determined by the numer cores in the cluster.
* to get number of partitions - ```df.rdd.getNumPartitions()```

#### Coalsece
* It reduces the number of partitions.
* Narrow Transformation 
* No shuffling , retains the sort order
* ``` Coalsesce(int)```

#### Repartition
* used to increase or decrease the number of partitions.
* wide transformation.
* evenly distributed partition size.
* ```repartition(int,col)```
