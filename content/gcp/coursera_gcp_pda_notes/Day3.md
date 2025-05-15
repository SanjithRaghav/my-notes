# Day 3
## Big Query
* Serverless
* pay as u go model or there is a fixed rate model that provides certain amount of resources.
* Storage(11000 peta bytes) + anlytics
* data encrypotion by default
* built in machine learning( run ml models using sql).

### upstream
- DataFlow(ETL)
- internal data
- external data
    - cloud storage
    - spanner
    - cloud sql
- multi-cloud data
- public datasets
### downstream 
#### BI tools
* Looker
* Looker Studio
* tableu
* google sheets

#### AI/ML tools
* Auto ML
* Vertext AI

### Execution
* interactive query(Executed as needed)
* Batch queries (queries start when resources are available).

### creating a ml model with big query:
```sql
create or replace model modelname
options (
    model_type='linear_regression',
    max_iteration=5,
) as
```