## Components of data engineering ecosystem

* Data Sources
* Data Sinks - Data Lakes and data warehouses.
* Data pipelines - processing these data
* Orchestration workflows - monitoring pipelines, triggering pipleines etc.

## ETL options:
### EL:
* when there isn't transformations required, data can ba directly loaded into warehouses.
* there is also an option of querying external tables stored elsewhere using bigquery.

### ELT: 
* used when the amount transformations needed isn't very high, and the transformation will not greatly reduce the amount of data.
* we can use bigquery sql to create new tables with transformed data.


## ETL: 
* used when the transformation is essential, and the data size is reduced significantly after transformation.
* transforming earlier can reduce the network bandwidth
* involves a data integration process, the data transformed in an intermidary service (dataflow, dataproc) and then loaded to data warehouse.

## Cloud Storage:

### properties:
* Persistence
* Availability
    * it is a global service, it can be accessed from any location in globe. 
    * Note: I can also be stored in a single region if required.
* Strong Consistency

* Durabilty
    * multiple replicas will be created to improve fault tolerance.
* High throughput and moderate latency.
    * replicas will be created in multiple regions in case of multiregion bucket and multiple zones in case of single region bucket.
    * requesters are served data from the nearest region or zone. this improves latency.


### buckets and objects:
* Cloud Storage essentially is an object storage and other properties and built around it.
* it consists of buckets and objects.
* buckets - are collection of objects, it has a unique global name , no other bucket can have the same name around the globe.
* objects - contains the actual data . also has metadata which is used for encryption, access control, compression etc.
* every bucket is linked to a region or multi region, choosing the region where the data transformation services are done can reduce the network charges.

### Management features:
* retention policy
    * set duration for retention.
* versioning 
* lifecycle management 
    * move to different types of storage-standard,nearline, coldline,archive. based on usage.


### Controlling access
* access control can be provided using wither IAM or Access control list,
* IAM is used accross all services in GCP, and can be used to provide access rules on bucket level, all object inside that bucket will have same access rules.
    * there are poject level(ability to set IAM bucket roles, modify or delete buckets) and bucket level roles(ability to set ACL, modify or delete objects and buckets).
* ACL can be used to provide access rules for buckets as well as objects.


## Building a data warehouse
* key difference between a data warehouse and a data lake is that data warehouse has a schema and it consolidate data from different resources, wheras data lakes doesn't have a fixed schema , and it stores all kind of data.

* Big query slots- fundamental units of big query's computational capacity.
* each slot has RAM, CPU and Network.
* Big query will automatically allocate the slots based on each stage.
* big query writes the result of a query in an intermediate table, and the query editor pull the result from that intermediate table. If we pass the same query again, it pulls the result from the cache and we are not charged for that query. 
* we can use the query validator to estimate the cost of the query execution.
* the cost of a query always goes to the active project where the query is executed.
* we can provide access control at the dataset,table,view and column level.
* need to learn labs with Access policies ibn bigquery. and bigquery data transfer service.

* to query the metadata of a bigquery dataset or a table - [github link of all metadata sql commands](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/information_schema.md)

* we denormalized data before storing it in bigquery. denormalizing makes it easier to process data parllelly and column based even if it is larger in size.

* instead of joins , take advantages of nested, and repeating queries. 

* keep the dimensions table under 10gb normalized, unless the table rarely goes through update and delete operations.

* denormalize the dimension tables if it is larger than 10gb, unless data manipulations or costs outweigh the benefits of optimal queries.

* we can create partitions while creating a table,
in three ways
    * time_partitioning_type=DAY -> partition data based on ingestion time.
    * time_partitioning_field=<date_field> -> partitioning data based on a datatime field
    * range_partitioning = <integer_field> -> partitioning data based on an integer field.
* bigquery supports clustering for both partitioned and nonpartitioned tables.



