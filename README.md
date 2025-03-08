# News Data Analysis Project

## Overview
This project automates the process of fetching news data from NewsAPI, storing it in GCP, and loading it into Snowflake using Apache Airflow. The pipeline ensures data is collected, processed, and stored efficiently for further analysis.

## Architecture
1. **Fetch News Data**: Retrieve news data from NewsAPI using a GET request.
2. **Store Data in Pandas DataFrame**: Convert JSON data into a Pandas DataFrame.
3. **Save as Parquet File**: Store the DataFrame as a Parquet file in the Airflow local environment.
4. **Upload to GCP Bucket**: Transfer the Parquet file to a Google Cloud Platform (GCP) bucket.
5. **Snowflake Storage Integration**: Establish a connection between Snowflake and the GCP bucket and create a stage.
6. **Airflow DAG Execution**: Use Airflow to manage the entire process by defining tasks and dependencies.

## Setup Instructions
### 1. Fetch News Data and Store in GCP
- Define a **user-defined Python function** (`fetch_news_data()`) to:
  - Make a GET request to NewsAPI.
  - Convert JSON response to a Pandas DataFrame.
  - Save the DataFrame as a Parquet file.
  - Upload the Parquet file to a **GCP bucket**.

### 2. Create Snowflake Database & Storage Integration
- In Snowflake, create a **database** `news_api_data`.
- Establish a **storage integration** to allow Snowflake to access GCP.
- Create a **stage** in Snowflake pointing to the GCP bucket.

### 3. Airflow Integration
- Import the `fetch_news_data()` function into the Airflow DAG.
- Map it to a `PythonOperator` to execute within the workflow.

### 4. Create Table in Snowflake
- Define a **SQL query** to create a Snowflake table for storing news data.
- Use `SnowflakeOperator()` in Airflow to execute this query.
- Set up **Snowflake connection ID** in Airflow (`snowflake_conn_id`).
- Install `apache-airflow-providers-snowflake==5.8.1` and configure Snowflake connection details in Airflow.

### 5. Copy Data from GCP to Snowflake
- Use `SnowflakeOperator()` to copy data from **Snowflake stage** into the **Snowflake table**.
- Execute a **SQL query** within Airflow to transfer data.

### 6. Additional Data Processing in Snowflake
- Define two additional **SnowflakeOperator** tasks:
  1. **Summarize News**: Generate a summary of news articles and store it in `summary_news` table.
  2. **Store Author Details**: Extract author information and store it in `author` table.

### 7. Define Task Flow in Airflow
- Set up task execution order in Airflow DAG:
  ```
  fetch_news_data_task >> snowflake_create_table >> snowflake_copy
  snowflake_copy >> [news_summary_task, author_activity_task]
  ```
- The last two tasks (`news_summary_task` and `author_activity_task`) run in parallel.

## Benefits
- **Automated Data Pipeline**: Ensures seamless data fetching, processing, and storage.
- **Parallel Processing**: Optimizes workflow execution using Airflow DAGs.
- **Scalability**: Supports continuous ingestion and analysis of news data.
- **Integration with Snowflake**: Enables efficient querying and analytics.

## Conclusion
This pipeline automates the process of collecting, storing, and analyzing news data. By leveraging Airflow, GCP, and Snowflake, it ensures a robust and scalable data pipeline for news analysis.

