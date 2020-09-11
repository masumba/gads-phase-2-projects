# Associate Cloud Engineer: Learning Phase 1 Main Track Channel (2020)
## Course: Course: Essential Google Cloud Infrastructure: Core Services
## Module: Resource Management
### LAB: Examining Billing data with BigQuery

### Objectives

In this lab, you learn how to perform the following tasks:

* Sign in to BigQuery from the Cloud Console

* Create a dataset

* Create a table

* Import data from a billing CSV file stored in a bucket

* Run complex queries on a larger dataset

### Step:1 Create a dataset

    bq --location=US mk -d \
    --default_table_expiration 86400000 \
    imported_billing_data
    
## Step:2 Create a table and import
    
    bq mk \
    -t \
    --expiration 3600 \
    --description "This is my table" \
    --label organization:development \
    imported_billing_data.imported_billing_data \
    qtr:STRING,sales:FLOAT,year:STRING
    
    bq load \
    --source_format=CSV \
    --skip_leading_rows=1 \
    imported_billing_data.imported_billing_data \
    gs://cloud-training/archinfra/export-billing-example.csv