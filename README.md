# AWS-ETL-Pipeline-Project

This project demonstrates a fully automated ETL (Extract–Transform–Load) data pipeline built using AWS serverless services.
It ingests raw JSON order data, transforms it into optimized Parquet format, and makes it available for SQL-based analytics using Amazon Athena.

The goal is to simulate a real-world data processing workflow for analytics and reporting while following AWS best practices.


# Architecture
| ----------- |
| <img width="7960" height="2170" alt="AWS Serverless ETL Pipeline drawio" src="https://github.com/user-attachments/assets/e9307cd5-17b1-46ed-940a-5a7ba4037e5f" /> |


# AWS Setup
Create an AWS account and log into the Management Console.

**Create S3 buckets and folders:**

- orders_json_incoming/ → raw JSON files
- orders_parquet_datalake/ → processed Parquet data

**Create IAM roles:**

- Lambda → access to S3 and Glue
- Glue Crawler → access to S3

**Verify Python and Jupyter Notebook locally for testing.**

| S3 folders structure |
| ----------- |
|<img width="1711" height="322" alt="S3 folders structure" src="https://github.com/user-attachments/assets/ed7b9a69-f27b-4ee5-8174-e99d9e290761" />|

| IAM role policy configuration |
| ----------- |
| <img width="2064" height="497" alt="IAM role policy configuration" src="https://github.com/user-attachments/assets/0f7a91ce-805c-4890-8cab-7fea20ae9946" /> |

# Data Preparation

| Raw JSON |
| ----------- |
| <img width="1308" height="842" alt="1  Raw json file" src="https://github.com/user-attachments/assets/b3da0463-d35b-4b1f-a3a9-a520692b07d1" /> |


| Read and Flatten JSON in Pandas|
| ----------- |
| <img width="1540" height="1250" alt="2  Flatten Data" src="https://github.com/user-attachments/assets/8b423042-2514-46ed-ad19-e77cb2b6a49d" />|


# AWS Lambda — ETL Automation

Function: ETL_pipeline
Trigger: S3 incoming/ (event: ObjectCreated)

**Lambda Tasks:**

  1. Read JSON from S3

  2. Flatten nested data

  3. Convert DataFrame to Parquet

  4. Save result to /parquet/ with timestamp

  5. Trigger Glue Crawler automatically

| Lambda function code window |
| ----------- |
| <img width="1423" height="1247" alt="Lambda function code window" src="https://github.com/user-attachments/assets/e026aee2-e542-4145-9d95-cded96dd6c49" /> |
  
| S3 event trigger configuration |
| ----------- |
| <img width="2085" height="811" alt="S3 event trigger configuration" src="https://github.com/user-attachments/assets/337248bb-3f6e-4a3b-a5ec-0ec2c23758cf" /> |

# AWS Glue Crawler

**Steps:**
  1. Create a new Glue crawler with source → parquet/.

  2. Assign an IAM role with access to S3.

  3. Verify schema after the first run (column names, data types).

  4. Automate crawler execution after new data upload.

| Glue Crawler configuration |
| ----------- |
| <img width="2208" height="1018" alt="Glue Crawler configuration" src="https://github.com/user-attachments/assets/7f671548-40c3-4d2e-be2c-b6163feafe19" /> |

| Glue Table Schema – Columns & Data Types |
| ----------- |
| <img width="2223" height="1134" alt="Glue Table Schema – Columns   Data Types" src="https://github.com/user-attachments/assets/5da86f29-ea9b-496e-885c-e3b4c8d1262e" /> |

| Glue Crawler Run – Success log |
| ----------- |
| <img width="2264" height="703" alt="Glue Crawler Run – Success log" src="https://github.com/user-attachments/assets/23b08765-1831-49db-9001-7327ddd3ba9b" /> |

# AWS Amazon Athena
**Steps:**

1. Create a database.

2. Use the table created by Glue.

3. Run SQL queries to analyze the Parquet data.

| Athena console query + results |
| ----------- |
| <img width="2146" height="1127" alt="Athena console query + results" src="https://github.com/user-attachments/assets/071d1af7-1311-4572-8952-652306ab2ad2" /> |

## Query examples

| Total sales by customer |
| ----------- |
| <img width="2111" height="1153" alt="Query 1 Total sales by customer" src="https://github.com/user-attachments/assets/b48bf4df-6f02-45eb-854a-46d447353244" />|


| Top-selling products |
| ----------- |
| <img width="2146" height="1014" alt="Query 2 Top-selling products" src="https://github.com/user-attachments/assets/871705dc-2c1d-4b45-b796-216606641ead" /> |


| Daily revenue trends |
| ----------- |
| <img width="2118" height="1152" alt="Query 3 Daily revenue trends" src="https://github.com/user-attachments/assets/49ada089-96ad-4f10-aa7a-0ee3a52e6dde" />|

# Automation & Monitoring

**Monitoring:**

- CloudWatch Logs for Lambda execution and Glue runs.

- Validate successful triggers and Parquet generation.

| CloudWatch Logs showing Lambda success |
| ----------- |
| <img width="2531" height="433" alt="CloudWatch Logs showing Lambda success" src="https://github.com/user-attachments/assets/5240465f-8af7-434e-9f5a-c4d8e47bc708" /> |

| Automatic Сrawler Execution |
| ----------- |
| <img width="2512" height="741" alt="Automatic Сrawler Execution" src="https://github.com/user-attachments/assets/66c44780-bd3a-40e1-9d0e-b41f6074de39" /> |


# Conclusion and Results

## Project Summary

This AWS ETL project demonstrates a fully automated order processing pipeline:

Upload JSON → Lambda ETL → Parquet → Glue Data Catalog → Athena queries

Serverless architecture: no manual intervention required after setup
End-to-end automation: Lambda triggers on file upload, Glue Crawler updates schema, Athena queries ready immediately

## Key Achievements

**Data Automation**

- Processed 100+ JSON orders in a single batch automatically

- Converted data into Parquet format, reducing storage size by ~30%

- Schema maintained automatically in Glue, no manual updates required

- Queries run instantly in Athena once Parquet files are uploaded
  
## Skills Demonstrated

- AWS Lambda, S3, Glue, Athena

- Python + Pandas for data transformation

- Serverless ETL design and automation

- Cloud monitoring via CloudWatch

## Results / Insights

- Total sales per customer calculated in under 2 seconds per 1000 orders

- Top-selling products identified for inventory planning

- Daily revenue trends available instantly for analytics

- Pipeline efficiency:
  -  Average Lambda execution time: ~5 seconds per JSON file
  -  Automatic Glue Crawler update within 30 seconds of Parquet upload
  -  Athena queries return results <1 second for 1000+ rows

