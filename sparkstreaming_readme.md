# Spark Streaming Setup Guide

## Introduction
This guide aims to provide a detailed walkthrough for setting up Spark Streaming in an AWS EC2 environment as part of a data engineering pipeline.

## Pre-requisites
- An AWS EC2 instance
- AWS S3 Bucket
- PySpark
- Python script `pyspark_ttcstreaming.py`
- JSON schema file `bus_status_schema.json`

## Installation Steps

### Step 1: Install Apache Spark on EC2
SSH into your AWS EC2 instance and follow these commands to install Apache Spark:
```
$ sudo apt-get update
$ sudo apt-get install default-jdk
$ wget https://downloads.apache.org/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz
$ tar xvf spark-*
$ sudo mv spark-3.1.2-bin-hadoop3.2 /usr/local/spark
```

### Step 2: Upload JSON Schema to S3
Before running the PySpark script, upload the `bus_status_schema.json` to your AWS S3 bucket.

### Step 3: Run PySpark Script
### Run PySpark Script
Navigate to the directory where PySpark is installed (spark-3.3.3-bin-hadoop3/bin). Then execute the following command:

```
./spark-submit --master local[*] --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.3,org.apache.hudi:hudi-spark3-bundle_2.12:0.12.3 --conf "spark.serializer=org.apache.spark.serializer.KryoSerializer" /home/ec2-user/final_project/pyspark_ttcstreaming.py
'''

## Troubleshooting
- Ensure that you've uploaded `bus_status_schema.json` to the S3 bucket.
- Make sure all necessary Python packages are installed.

## Further Information
For additional details, consult [Apache Spark's official documentation](https://spark.apache.org/docs/latest/streaming-programming-guide.html).

