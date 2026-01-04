# AWS Weather ETL Pipeline

Production-grade end-to-end ETL pipeline on AWS that ingests weather data hourly, stores raw data, processes it into analytics-ready Parquet format, and enables querying via Athena.

## Architecture
EventBridge → Lambda → S3 (Raw) → Glue Crawler → Glue ETL → S3 (Processed) → Athena

## Tech Stack
- AWS Lambda
- Amazon EventBridge
- Amazon S3
- AWS Glue (Crawler + Spark ETL)
- Amazon Athena
- AWS Budgets

## Features
- Hourly ingestion
- Secure secrets via SSM
- Partitioned Parquet data
- Cost controls enabled

## Author
Kumar Manik
