# Redshift

- [Redshift](#Redshift)
- [Intro Redshift](#Intro-Redshift)
  - [Easy to setup, deploy, and manage](#Easy-to-setup-deploy-and-manage)
  - [Query your data lake](#Query-your-data-lake)
- [Loading Data](#Loading-Data)
  - [COPY from Amazon DynamoDB](#COPY-from-Amazon-DynamoDB)
  - [Loading Data from an Amazon DynamoDB Table](#Loading-Data-from-an-Amazon-DynamoDB-Table)
  - [Loading Data from Amazon S3](#Loading-Data-from-Amazon-S3)
    - [Step 1: Launch a Cluster](#Step-1-Launch-a-Cluster)
    - [Step 2: Download the Data Files](#Step-2-Download-the-Data-Files)
    - [Step 3: Upload the Files to an Amazon S3 Bucket](#Step-3-Upload-the-Files-to-an-Amazon-S3-Bucket)
      - [Upload the files to an Amazon S3 bucket:](#Upload-the-files-to-an-Amazon-S3-bucket)
      - [User Credentials](#User-Credentials)
    - [Step 4: Create the Sample Tables](#Step-4-Create-the-Sample-Tables)
    - [Step 5: Run the COPY Commands](#Step-5-Run-the-COPY-Commands)
    - [MANIFEST](#MANIFEST)
    - [Step 6: Vacuum and Analyze the Database](#Step-6-Vacuum-and-Analyze-the-Database)
    - [Step 7: Clean Up Your Resources](#Step-7-Clean-Up-Your-Resources)
  - [Tutorial: Tuning Table Design](#Tutorial-Tuning-Table-Design)
- [](#)
- [Resources](#Resources)

# Intro Redshift

## Easy to setup, deploy, and manage

`Automated provisioning`: Amazon Redshift is simple to set up and operate. You can deploy a new data warehouse with just a few clicks in the AWS console, and Redshift automatically provisions the infrastructure for you. Most administrative tasks are automated, such as backups and replication, so you can focus on your data, not the administration. When you want control, Redshift provides options to help you make adjustments tuned to your specific workloads.

`Automated backups`: Amazon Redshift automatically and continuously backs up your data to `Amazon S3`. Redshift can asynchronously `replicate your snapshots to S3` in another region for disaster recovery. You can use any system or user snapshot to restore your cluster using the AWS Management Console or the Redshift APIs. Your cluster is available as soon as the system metadata has been restored, and you can start running queries while user data is spooled down in the background.

`Flexible querying`: Amazon Redshift gives you the flexibility to execute queries within the `console` or `connect SQL client tools`, `libraries`, or `Business Intelligence tools` you love. Query Editor on the AWS console provides a powerful interface for executing SQL queries on Redshift clusters and viewing the query results and query execution plan (for queries executed on compute nodes) adjacent to your queries.

`Integrated with third-party tools:` Enhance Amazon Redshift by working with industry-leading tools and experts for loading, transforming and visualizing data.

- Load and transform your data with `Data Integration Partners`
- Analyze data and share insights across your organization with `Business Intelligence Partners`
- Architect and implement your analytics platform with `System Integration and Consulting Partners`
- Query, explore and model your data using tools and utilities from `Query and Data Modeling Partners`

---

## Query your data lake

`Amazon S3 data lake`: Amazon Redshift is the only data warehouse that extends your queries to your Amazon S3 data lake without loading data. You can query open file formats you already use, such as Avro, CSV, Grok, JSON, ORC, Parquet, and more, directly in S3. This gives you the flexibility to **store highly structured, frequently accessed data** on Redshift local disks, keep exabytes of structured and unstructured data in S3, and query seamlessly across both to provide unique insights that you would not be able to obtain by querying independent datasets.

`AWS analytics ecosystem`: Amazon Redshift is natively integrated with the AWS analytics ecosystem. 

- `AWS Glue` can extract, transform, and load (ETL) data into Redshift. 
- `Amazon Kinesis Data Firehose` is the easiest way to capture, transform, and load streaming data into Redshift for near real-time analytics.
- You can use `Amazon QuickSight` to create reports, visualizations, and dashboards.
- `AWS Data Pipeline` AWS Data Pipeline is a web service that helps you reliably process and move data between different AWS compute and storage services, as well as on-premises data sources, at specified intervals. With AWS Data Pipeline, you can regularly access your data where it’s stored, transform and process it at scale, and efficiently transfer the results to AWS services such as Amazon S3, Amazon RDS, Amazon DynamoDB, and Amazon EMR.

***

# Loading Data

## COPY from Amazon DynamoDB

https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-source-dynamodb.html


***

## Loading Data from an Amazon DynamoDB Table

https://docs.aws.amazon.com/redshift/latest/dg/t_Loading-data-from-dynamodb.html

You can use the COPY command to load a table with data from a single Amazon DynamoDB table.

- The COPY command matches attribute names in the items retrieved from the DynamoDB table to column names in an existing Amazon Redshift table by using the following rules:

- Amazon Redshift table columns are case-insensitively matched to Amazon DynamoDB item attributes. If an item in the DynamoDB table contains multiple attributes that differ only in case, such as Price and PRICE, the COPY command will fail.

- Amazon Redshift table columns that do not match an attribute in the Amazon DynamoDB table are loaded as either NULL or empty, depending on the value specified with the EMPTYASNULL option in the COPY command.

- Amazon DynamoDB attributes that do not match a column in the Amazon Redshift table are discarded. Attributes are read before they are matched, and so even discarded attributes consume part of that table's provisioned throughput.

- Only Amazon DynamoDB attributes with scalar `STRING` and `NUMBER` data types are supported. 

- **The Amazon DynamoDB BINARY and SET data types are not supported**. If a COPY command tries to load an attribute with an unsupported data type, the command will fail. If the attribute does not match an Amazon Redshift table column, COPY does not attempt to load it, and it does not raise an error.

***

## Loading Data from Amazon S3

[Tutorals](https://docs.aws.amazon.com/redshift/latest/dg/tutorial-loading-data.html)

You can add data to your Amazon Redshift tables either by using an INSERT command or by using a COPY command. At the scale and speed of an Amazon Redshift data warehouse, the COPY command is many times faster and more efficient than INSERT commands.

---

### Step 1: Launch a Cluster

Follow the Getting Started steps to connect to your cluster from a SQL client and test a connection. You do not need to complete the remaining Getting Started steps to create tables, upload data, and try example queries.

<br>

### Step 2: Download the Data Files

In this step, you will download a set of sample data files to your computer. In the next step, you will upload the files to an Amazon S3 bucket.

```
customer-fw-manifest
customer-fw.tbl-000
customer-fw.tbl-000.bak
customer-fw.tbl-001
customer-fw.tbl-002
customer-fw.tbl-003
customer-fw.tbl-004
customer-fw.tbl-005
customer-fw.tbl-006
customer-fw.tbl-007
customer-fw.tbl.log
dwdate-tab.tbl-000
dwdate-tab.tbl-001
dwdate-tab.tbl-002
dwdate-tab.tbl-003
dwdate-tab.tbl-004
dwdate-tab.tbl-005
dwdate-tab.tbl-006
dwdate-tab.tbl-007
part-csv.tbl-000
part-csv.tbl-001
part-csv.tbl-002
part-csv.tbl-003
part-csv.tbl-004
part-csv.tbl-005
part-csv.tbl-006
part-csv.tbl-007
```

<br>

### Step 3: Upload the Files to an Amazon S3 Bucket

#### Upload the files to an Amazon S3 bucket:

1. Create a bucket in Amazon S3.

2. Create a folder.

3. Upload the data files to the new Amazon S3 bucket.

#### User Credentials

The Amazon Redshift COPY command must have access to read the file objects in the Amazon S3 bucket. If you use the same user credentials to create the Amazon S3 bucket and to run the Amazon Redshift COPY command, the COPY command will have all necessary permissions.

<br>

### Step 4: Create the Sample Tables

For this tutorial, you will use a set of five tables based on the Star Schema Benchmark (SSB) schema. The following diagram shows the SSB data model.

<br>

### Step 5: Run the COPY Commands

You will run COPY commands to load each of the tables in the SSB schema. The COPY command examples demonstrate loading from different file formats, using several COPY command options, and troubleshooting load errors.

### MANIFEST

When you COPY from Amazon S3 using a key prefix, there is a risk that you will load unwanted tables. 

For example, the 's3://mybucket/load/ folder contains eight data files that share the key prefix customer-fw.tbl: customer-fw.tbl0000, customer-fw.tbl0001, and so on. 

However, the same folder also contains the extraneous files customer-fw.tbl.log and customer-fw.tbl-0001.bak.

To ensure that you load all of the correct files, and only the correct files, use a manifest file. 

The manifest is a text file in JSON format that explicitly lists the unique object key for each source file to be loaded. The file objects can be in different folders or different buckets, but they must be in the same region. For more information, see MANIFEST.


<br>

### Step 6: Vacuum and Analyze the Database

Whenever you add, delete, or modify a significant number of rows, you should run a VACUUM command and then an ANALYZE command. A vacuum recovers the space from deleted rows and restores the sort order. The ANALYZE command updates the statistics metadata, which enables the query optimizer to generate more accurate query plans. 

<br>

### Step 7: Clean Up Your Resources

Your cluster continues to accrue charges as long as it is running. When you have completed this tutorial, you should return your environment to the previous state by following the steps in Step 5: Revoke Access and Delete Your Sample Cluster in the Amazon Redshift Getting Started.


***

## Tutorial: Tuning Table Design

https://docs.aws.amazon.com/redshift/latest/dg/tutorial-tuning-tables.html

***

# 

***

# Resources

[Offical AWS Docs](https://docs.aws.amazon.com/)