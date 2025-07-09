## Overview

This project implements a complete data pipeline as described in the [Rearc Data Quest assignment](https://github.com/rearc-data/quest).   
It covers data ingestion, storage, analytics, and automation using AWS services and Python.

---

## Contents

- **Part 1:** BLS Data Sync to S3 (`part1_bls_to_s3.py`)
- **Part 2:** DataUSA API Fetch to S3 (`part2_datausa_to_s3.py`)
- **Part 3:** Data Analytics (Databricks notebook, PySpark) (`part3_analytics.html` and/or `part3_analytics.ipynb`)
- **Part 4:** Infrastructure as Code (AWS CDK) and Data Pipeline Automation (`cdk/` folder)
- **Sample Data:** `sample_data/population.json`
- **This README**
- All files can be found here : [Link](https://drive.google.com/drive/folders/134x4619Yu0VpE_kexBRqv0e0kJEQZeSN?usp=sharing)

---

## Part 1: AWS S3 & Sourcing Datasets

- **Script:** `part1_bls_to_s3.py` [Link-to-file](https://drive.google.com/file/d/1dshWpz4swT2RZXfEPFEazp7E2SXO-1dl/view?usp=drive_link)
- **What it does:**  
  Downloads all files from the [BLS time series directory](https://download.bls.gov/pub/time.series/pr/), and syncs them to an S3 bucket, handling new/removed files automatically.
- **S3 Link:**  
  https://bls-dataset-bucket.s3.eu-north-1.amazonaws.com/pr.data.0.Current
- **Note:**  
  The script uses a custom User-Agent header to comply with BLS data access policies.

![[Pasted image 20250710005606.png]]

---

## Part 2: APIs

- **Script:** `part2_datausa_to_s3.py` [Link-to-file](https://drive.google.com/file/d/1FIi3aXBvn6Mxb7QIPDi0l-EO7BazSvA2/view?usp=drive_link)
- **What it does:**  
  Fetches US population data from the DataUSA API and saves the result as a JSON file in S3.
- **Note:**  
  If the API is unavailable, the script uploads demo data for continuity.

---

## Part 3: Data Analytics

- **Notebook:** `part3_analytics.html` [Link-to-file](https://drive.google.com/file/d/1JSPAte6Fs-lau7ydF9ih1BYlzBMoRPSx/view?usp=drive_link) (with output) and/or `part3_analytics.ipynb` [Link-to-file](https://drive.google.com/file/d/1IPRmd2Um7zSpTI6GsMNPiWm9TSPBQsbo/view?usp=drive_link)
- **What it does:**  
  - Loads the BLS CSV and DataUSA JSON as PySpark DataFrames.
  - Calculates:
    - Mean and standard deviation of US population (2013–2018)
    - Best year (max sum of value) for each `series_id`
    - Joins BLS and population data for `series_id = PRS30006032` and `period = Q01`
  - Shows all code, queries, and results.
- **How to view:**  
  Open the HTML file in your browser to see all code and output.  

---

## Part 4: Infrastructure as Code & Data Pipeline

- **Code:** `cdk/` folder (see structure below) [Link-to-folder](https://drive.google.com/drive/folders/17QzHbd7aIIwB2EgyxVJNZMMlhw1xeqQR?usp=drive_link)
- **What it does:**  
  - Deploys a data pipeline using AWS CDK (Python):
    - S3 bucket for data
    - SQS queue for notifications
    - Lambda for ingest (scheduled daily, runs Part 1 & 2)
    - Lambda for analytics (triggered by SQS, runs Part 3 logic and logs results)
  - All analytics results are logged to CloudWatch Logs (no .ipynb required for this part).
- **How to deploy:**
  1. Install dependencies: `pip install -r requirements.txt`
  2. Bootstrap your AWS environment: `cdk bootstrap`
  3. Deploy: `cdk deploy`
- **How to test:**
  - Trigger the Ingest Lambda manually or wait for the schedule.
  - Check S3 for new files.
  - Check SQS for messages.
  - Check CloudWatch Logs for analytics output.

---

## Folder Structure

```
submission/
│
├── part1_bls_to_s3.py
├── part2_datausa_to_s3.py
├── part3_analytics.html
├── part3_analytics.ipynb
├── README.md
│
├── cdk/
│   ├── app.py
│   ├── cdk.json
│   ├── requirements.txt
│   ├── rearc_data_pipeline/
│   │   ├── __init__.py
│   │   └── rearc_data_pipeline_stack.py
│   └── lambda/
│       ├── ingest/
│       │   ├── ingest.py
│       │   └── requirements.txt
│       └── analytics/
│           ├── analytics.py
│           └── requirements.txt
│
└── sample_data/
    └── population.json
```

---

## Notes

- **Demo data** is used for DataUSA if the API is unavailable.
- **All code is commented and ready to run.**
- **Contact:** kalokhe.pushpak28@gmail.com

---

**Thank you for reviewing my submission!**  
Let me know if you need access to the S3 bucket or have any questions.

---
