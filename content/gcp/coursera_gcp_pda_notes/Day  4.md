## El , ELT and ETL

### EL -> Extract and load. 
* ==must be used only when the data is clean and correct.==
* Load it into bigquery's native storage, done using a rest api call.
* We can trigger it using a cloud function, cloud composer.

>cloud composer is a fully managed workflow orchestration tool, built on top of apache airflow and uses python programming to build the workflow.



### ELT -> Extract, Load and Transform. 
* ==experimental datasets where you are not sure which transformations are needed to make the data usable.==
* ==this can work only when the transformations can be expressed in SQL ==
* loaded into target and transformed on the fly whenever required.
* files from cloud storage is loaded into bigquery , transform the data on the fly using bigquery table views or new tables.

#### Purposes of data quality processing
* Validity
	* out of range
	* Data mismatch
	* empty fields
```sql
	Where
	Having
	Where field is Not Null
```
* Accuracy
	* Look up values against an objective datasets
	* do not exist
```sql
	in()
	use subquery or a join
```

* Completeness
	* Missing data
```sql
	nullif()
	ifnull()
	coalesce()
```
* consistency
	* duplicate records 
	* concurrency issues
* uniformity
	* same units of measurement
```sql
	format()
	cast()
```

#### Shortcomings
* if transformation cannot be fully expressed in sql.


### ETL -> Extract Transform Load 
* Transformed in an intermediate stage and then loaded into a data warehouse.
* use dataflow or dataproc  to build pipelines to write data from databases to bigquery.
* used when the raw data needs to be quality controlled , transformed or enriched before loading
* used when there are complex transformation which cannot be expressed in sql
#### how to choose a service?
* **dataproc**
	* managed service for batch, stream processing , querying and ml.
	* cost effective
	* integrated with data services in GCP
	* used when heavily invested in hadoop and spark.
	
* **data fusion**
	* visual pipeline builder, used by low-code or no-code users.(BA)
	
* **data flow**
	* based on apache beam
	* used for low-latency

Labels on datasets, tables and views help us to understand its lineage.
==should learn about data catalog==
	
