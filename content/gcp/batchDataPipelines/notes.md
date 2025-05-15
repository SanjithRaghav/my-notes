cases when we store data in other options over bigquery.
1.  lower latency and higher throughput - dataflow to bigtable (since there is no overhead of query planning and execution).
2. already have invested in spark - dataflow to dataproc.
3. need for visual pipeline building - data fusion.


## Data proc
* use cloud storage instead of hdfs
* make sure that the cloud storage bucket is in the same region of the dataproc cluster.
* dont use many input files(10000), in that case combine the files into larger files.
* allocate enough virtual machines to the cluster.
* make sure that persistant disks are not limitting the throuput.

### use local hdfs when:
    1. jobs require lot of metadata operations.
    2. if we modify the  hdfs data frequently or rename the directories frequently.(cloud storage objects are immutable, and directories in gcs is just a naming convention and there aren't physical directories in storage, so renaming a file or a folder means copying entire data into another object).
    3. if we heavy IO
    4. IO workloads that are sensitive to latency. 

* use cloud storage as the initial and final source of data in a pipeline 
for example : if there are 5 spark jobs, first job reads the data from google cloud storage, and stores the intermediate output to hdfs. and the last job writes the output into gcs.
* Alocate atleast 100 gb for the primary persitant disks.
* we can increase or decrease the size of the local hdfs by changing the size of primary persistant disks of primary and secondary nodes.
* attach upto 8 ssds for each workeres and use these for hdfs if we are doing io intensive jobs and single digit ms latency.

* We can schedule a cluster deletion based on the idle time of a cluster. 
* main advantage of cloud clusters over onprem hadoop clusters is that they are ephemeral.required resources are active only when they are used.
* always try to use ephemeral clusters, that is spin up a cluster when required -> run the jobs -> store the output in cloud storage or other data sinks -> delete the cluster -> view logs in data log or cloud storage.
* if there is a need to use a persistant cluster, then follow these tips to reduce the cost:
    * create smallest clusters you can , using preemptable VMs based in time and budget.
    * scale the clusters to minimum number of workable nodes.
    

