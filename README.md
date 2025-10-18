# AWS-ETL-Pipeline-Project

This project demonstrates a fully automated ETL (Extract–Transform–Load) data pipeline built using AWS serverless services.
It ingests raw JSON order data, transforms it into optimized Parquet format, and makes it available for SQL-based analytics using Amazon Athena.

The goal is to simulate a real-world data processing workflow for analytics and reporting while following AWS best practices.

---

## Architecture
| ----------- |
|  |




## AWS Setup
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

## Data Preparation

| Raw JSON |
| ----------- |
| <img width="1308" height="842" alt="1  Raw json file" src="https://github.com/user-attachments/assets/b3da0463-d35b-4b1f-a3a9-a520692b07d1" /> |


| Read and Flatten JSON in Pandas|
| ----------- |
| <img width="1540" height="1250" alt="2  Flatten Data" src="https://github.com/user-attachments/assets/8b423042-2514-46ed-ad19-e77cb2b6a49d" />|


## AWS Lambda — ETL Automation

Function: ETL_pipeline
Trigger: S3 incoming/ (event: ObjectCreated)

**Lambda Tasks:**

  1.Read JSON from S3

  2.Flatten nested data

  3.Convert DataFrame to Parquet

  4.Save result to /parquet/ with timestamp

  5.Trigger Glue Crawler automatically

| Lambda function code window |
| ----------- |
| <img width="1423" height="1247" alt="Lambda function code window" src="https://github.com/user-attachments/assets/e026aee2-e542-4145-9d95-cded96dd6c49" /> |
  
| S3 event trigger configuration |
| ----------- |
| <img width="2085" height="811" alt="S3 event trigger configuration" src="https://github.com/user-attachments/assets/337248bb-3f6e-4a3b-a5ec-0ec2c23758cf" /> |

## AWS Glue Crawler

