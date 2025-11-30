HR Attrition & Headcount Data Engineering Pipeline
Automated ETL Pipeline using Airflow, Docker, GCP (BigQuery + GCS), Python & Terraform

This project automates the ingestion, transformation, storage, and visualization of HR Headcount (HC) and Attrition data for a large IT organization.
The goal is to ensure that management always has an up-to-date dashboard with insights on employee attrition, retention, and workforce distribution.

The pipeline orchestrates data extraction, ETL, cloud storage, BigQuery loading, and report refreshes using Airflow running on Docker.
A complete HR analytics dashboard is built in Looker Studio.

All implementation details come from the project documentation. 

DE shanistha (1)

📁 Repository Contents
File	Description
docker-compose.yaml	Airflow environment running on Docker.
main.tf	Terraform script used to create Google Cloud Storage bucket.
DE shanistha (1).pdf	Full project documentation with screenshots and workflow explanation. 

DE shanistha (1)


README.md	Project documentation (this file).
📌 Project Overview

A large IT company is experiencing rising employee attrition at a specific location.
Management requires an automated, reliable dashboard to monitor:

Headcount (HC)

Attrition trends

Reasons for attrition

Department-level breakdowns

HR provides CSV data regularly, and this project ensures the dashboard updates automatically without manual effort.
(As described on page 1 of the PDF. 

DE shanistha (1)

)

🏗️ Architecture Overview
🔥 Tools Used

Apache Airflow (Dockerized)

Python (ETL processing)

Google Cloud Storage (GCS)

Google BigQuery

Looker Studio

Terraform (for GCS bucket creation)

🛠️ End-to-End Workflow (Airflow DAG)

According to pages 1–3 of the PDF 

DE shanistha (1)

, the pipeline consists of:

▶️ Task 1 — Download CSV from Server (BashOperator)

Fetch HR CSV from an intranet file server

Rename dynamically (e.g., 20240201.csv)

▶️ Task 2 — ETL Processing (PythonOperator)

Convert categorical → numerical fields

Remove unnecessary columns

Create cleaned file:
employee_hr_data_YYYYMMDD.csv

▶️ Task 3 — Upload to GCS (PythonOperator)

Upload processed CSV to Google Cloud Storage bucket created using Terraform

▶️ Task 4 — Create BigQuery Table If Not Exists

Check BigQuery table

Create table on first run

▶️ Task 5 — Load Data from GCS → BigQuery Table

Append new data

Ensure dashboard always has fresh numbers

Airflow DAG screenshots show successful execution of all tasks (page 3).


DE shanistha (1)

☁️ Google Cloud Setup with Terraform

Based on pages 4–5 of the PDF, Terraform is used to create a GCS bucket: 

DE shanistha (1)

main.tf includes:

Provider configuration

Variables for bucket name, region, project ID

google_storage_bucket resource pointing to:

name          = var.bucket_name  
location      = var.bucket_location  
storage_class = var.storage_class  


Bucket successfully created (screenshot on page 5).


DE shanistha (1)

🗄️ BigQuery Integration

Airflow loads the processed CSV directly into BigQuery

Table is automatically created on first run

Data refresh confirmed in screenshots (pages 6–7)


DE shanistha (1)

📊 Looker Studio Dashboard

A complete HR dashboard was built on top of the BigQuery table.

Dashboards Include:

Active Headcount Overview (pie charts & bar charts)

Attrition Overview

Attrition Deep Dives (performance rating vs attrition count)

Screenshots on pages 8–9 show the visual reports.


DE shanistha (1)

🎯 Results & Business Impact
✔ Fully automated ETL pipeline

No manual steps. HR team simply uploads CSV → Dashboard updates automatically.

✔ Cloud-native & scalable

GCS + BigQuery ensures long-term storage and fast analytics.

✔ Improved HR decision-making

Management gets daily monitoring of:

Attrition by department

Reasons for leaving

High-risk groups

Workforce distribution

✔ Production-ready design

Airflow makes this workflow repeatable, reliable, and maintainable.

🚀 Future Enhancements

Potential improvements:

Add email alerts for high attrition

Deploy Airflow on Kubernetes

Add dbt for data modeling

Add SCD logic for tracking employee lifecycle

Integrate predictive modeling (attrition prediction)

⭐ Summary

This project demonstrates a full Data Engineering pipeline combining:

✔ Airflow orchestration
✔ Dockerized infrastructure
✔ Cloud storage & data warehousing
✔ Real-time dashboarding
✔ Fully automated HR analytics
