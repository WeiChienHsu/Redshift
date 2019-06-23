# Redshift



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

---

### Data Distribution: EVEN Example

```sql
CREATE TABLE loft_deep_dive (
  aid INT -- audience_id
  , loc CHAR(3) -- location
  , dt DATE --date
) DISTSTYLE EVEN
```

```sql
INSERT INTO loft_deep_dive VALUES
(1, 'SFO', '2016-09-01'),
(1, 'SFO', '2016-09-01'),
(1, 'SFO', '2016-09-01'),
(1, 'SFO', '2016-09-01');
```