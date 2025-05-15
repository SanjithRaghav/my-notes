# Day 2

## Compute 
* <mark>compute Engine</mark>:Infrastructure as a service.
* <mark>Google Kubernetes engine</mark>: it is used to deploy manage and scale containarized applications.
* <mark>App engine</mark>: Platform as a service
* <mark>Cloud Functions</mark> : Serverless execution environment.(Event driven execution)
* <mark>Cloud Run</mark>: serverless compute to deploy containarized applications.

## Storage
### unstructured Data: 
* data that is not stored in tabular format
* usually Stored in <mark>Cloud Storage</mark>(used to Store objects in buckets)
* now it can be stored in bigquery as well
#### Type of unstructured data storage based on access frequency
* Standard storage(hot data)
* nearline storage(once per month)
* coldline storage(once in 90 days)
* archival storage(once in a year)


### Structure data:
#### Transactional workloads
* used for online transactional processing systems.
* requires fast inserts and updates to build row based records.
* relatively standard queries that impact only a few rows.
* Sql
    * <mark>Cloud Sql</mark> (local/regional scalability)
    * <mark>Spanner</mark>(flobal scalability)
* No Sql:
    * <mark>Firestore</mark>(document based data store)
#### Analytical workloads
* requires entire table scans and aggregations.
* relatively complexe queries.
* Sql
    * <mark>Big Query</mark>(Data warehouse)
* Nosql
    * <mark>Big Tables</mark>

Data to AI workflow:
1. Ingestion and process 
2. Data Storage
3. Analytics
4. Machine learning

## Data Engineering for streaming data
### Pub Sub
it is a distributed messaging service.
* it offers atleast once delivery
* No provision is required 
* global by default
* APIs are open
* offers end to end encryption

Pipeline
1. data from upstream is ingested by pub sub,
2. it then read, stores and broadcasts the data to any subscribers of the data topic.
3. the data is then ingested and processed by a elastic streaming pipeline.
4. it is then fed to a visualizer to analyze the data.


### Data flow
- It creates a pipeline to process(ETL) both streaming and batch data.

- no operations required - maintanence , monitoring and scaling done by GCP
- serverless
#### it provides
- Graph optimization
- work scheduler
- autoscaler
- auto healing
- work rebalancing
- compute and storage

#### offers templates for
- Streaming
    - pubsub to Bigquery
    - pubsub to mongodb
    - pubsub to cloud storage
    - data stream to big query
- batch 
    - Bigquery to cloud storage
    - cloud storage to big query
    - big table to cloud storage
    - cloud spanner to cloud storage
- utility
    - Bulk compression of Cloud Storage files
    - File format conversion
    - file store bulk deletion


#### Few questions to be considered during the pipeline design phase.
* will the pipeline code be compatible for both batch and stream processing or will it need to be refactored
* will the pipeline code sdk has all the transformations, mid flight aggregations and windowing
* will the code sdk be able to handle late data

> mid flight aggregation : process of aggregating or transforming data while it is being processed in real-time, without waiting for the entire dataset to be collected 

> late data : it refers to data that arrives after a window of time has closed or after the expected processing time

### Apache beam
* used to build and execute data processing pipelines.
* It supports execution engines like apache spark and data flow. 
* provides inbuilt pipeline templates.
