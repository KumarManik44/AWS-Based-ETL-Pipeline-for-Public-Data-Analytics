# AWS Weather ETL Pipeline 🌦️

An end-to-end **serverless data engineering project** that ingests real-time weather data, processes it, and makes it queryable using AWS-native analytics tools.

This project demonstrates a **production-style ETL pipeline** using AWS Lambda, S3, Glue, Athena, EventBridge, and cost controls via AWS Budgets.

---

## 🏗️ Architecture Overview

**Flow:**

1. **EventBridge (Scheduler)** triggers ingestion every hour  
2. **AWS Lambda** fetches weather data from OpenWeatherMap API  
3. Raw JSON data is stored securely in **Amazon S3 (Raw Zone)**  
4. **AWS Glue Crawler** catalogs raw data  
5. **AWS Glue ETL Job** transforms raw JSON → analytics-ready Parquet  
6. Processed data is written to **S3 (Processed Zone)** partitioned by city & date  
7. **Amazon Athena** enables SQL queries on processed data  
8. **AWS Budgets** monitors and caps monthly spend

---

## 🧱 Tech Stack

- **Compute**: AWS Lambda, AWS Glue
- **Storage**: Amazon S3 (Raw & Processed zones)
- **Orchestration**: Amazon EventBridge
- **Catalog**: AWS Glue Data Catalog
- **Query Engine**: Amazon Athena
- **Security**: IAM, SSM Parameter Store (SecureString)
- **Cost Control**: AWS Budgets
- **Format**: Parquet (columnar, analytics-optimized)

---

## 📂 S3 Data Layout

### Raw Zone
`
s3://aws-weather-data-lake-kumar/raw/weather/
└── ingestion_date=YYYY-MM-DD/
└── weather_YYYYMMDD_HHMMSS.json
`


### Processed Zone
`
s3://aws-weather-data-lake-kumar/processed/weather/
└── city=Bangalore/
└── ingestion_date=YYYY-MM-DD/
└── part-0000.snappy.parquet
`

---

## 📊 Schema (Processed Data)

| Column           | Type      |
|------------------|-----------|
| event_time       | timestamp |
| temperature_c    | double    |
| humidity         | int       |
| pressure         | int       |
| weather_main     | string    |
| wind_speed       | double    |
| city             | string (partition) |
| ingestion_date   | date (partition) |

---

## 🧪 Sample Athena Query

```sql
SELECT
  city,
  date(event_time) AS day,
  avg(temperature_c) AS avg_temp
FROM weather_data.processed_weather
GROUP BY city, date(event_time)
ORDER BY day DESC;
```

## 🔐 Security & Best Practices

- API keys stored in SSM Parameter Store (SecureString)
- IAM roles scoped to least privilege
- Parquet + partitioning for low Athena scan cost
- No hardcoded secrets
- Serverless. No always-on infrastructure

## 💸 Cost Awareness
- Event-driven. No idle compute
- Budget alert configured (monthly threshold)
- Athena scans only partitioned data
- Glue jobs run on-demand

## 🚀 What This Project Demonstrates

- Real-world AWS ETL pipeline design
- Serverless data engineering
- JSON → Parquet transformations
- Schema evolution handling
- Partition-aware analytics
- Production-grade IAM + cost controls

## 📌 Possible Extensions

- Add QuickSight dashboards
- Add data quality checks
- Add alerts on extreme weather
- Stream ingestion using Kinesis
- CI/CD via Terraform or CDK

## 🧠 Author

Built as a hands-on learning project to deeply understand AWS Data Engineering workflows, not just theory.

If you’re learning data engineering or cloud analytics. This pipeline mirrors real production patterns.

⭐ If this helped you, consider starring the repo.
