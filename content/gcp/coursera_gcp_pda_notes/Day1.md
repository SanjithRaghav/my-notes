# Day 1

### Structured Data
* Organized
* Rigid
* Quantitative

### Unstructured Data
* Disorganized
* File Based
* Stored in cloud Buckets

### Semi structured data
* Content is unstructured
* Tags or metadata is added to it

### Data Processing
- remove Duplicates
- filter irrelevant data
- Revover misising data
- consolidate incostent data

#### Objectives of Data Processing:
* Analysis 
* Reporting

#### Cloud Data Processing Services
- DataProc -> Data Science and ML service. It also supports a fully managed Hadoop and Spark deployment on clusters.(has a built in report system)
- DataFlow - Batch and stream processing tool
- DataPrep -  visually exploring, cleaning, and preparing structured and unstructured data.
- Data Fusion - fully managed ETL service. no code platform to manage data pipelines.

## Identity and access management

Controls access to cloud based applications. it helps to manage who can access what.

### Roles
set of permissions given to a user 

#### Basic Roles:

these are broad access given across all services, and are designed for simlicity.
- Owner
- Editor
- Viewer

#### Predefined roles:
these are roles defined by google for the access of specific services.

#### Custom Roles:
these are roles defined by users for specific needs.


## Gcloud cli commands
### Projects:
create a project:``` gcloud projects create PROJECTID --name ProjectName```

to set a project as default project:```gcloud config set project PROJECTID```

toggle projects: ```gcloud config set project NEW_PROJECT_ID```

List Projects: ```gcloud projects list```

delete projects:```gcloud projects delete PROJECTID```


### Buckets:
create a bucket : ```gcloud storage buckets create gs://NAME_OF_Bucket```

to create in a different location :
add ``` location=us-central-1```

upload files from local file system to gcloud: ```gcloud storage cp local_path gs://NAME_OF_Bucket```

### compute engine:
create n instance : 
```shell
gcloude compute instances create \
example \ 
--zone=us-central-1 \
--machine-type=n1-standard-1 \
--image-family=ubuntu-2004-lts \
--image-project=ubuntu-os-cloud \
--boot-disk-size=20GB
```

to  list instances
```gcloud comput instances list```

to ssh into an instance 
```gcloud compute ssh example --zone=us-central-a``` 

