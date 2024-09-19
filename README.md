# Azure-Data-Pipeline-for-Ingestion-and-Transformation




# Data Ingestion and Transformation Pipeline using Azure Data Factory

## Overview

This project demonstrates how to build a data pipeline using **Azure Data Factory (ADF)** for ingesting and transforming data from multiple sources into **Azure Data Lake Storage (ADLS)**. The ingested data is cleaned, transformed, and stored in a structured format, ready for analytics and reporting purposes. The pipeline incorporates error handling, monitoring, data validation, and automated scheduling for seamless operation.

---
## Business Problem

In today’s data-driven world, organizations often face challenges with:
- **Manual Data Handling**: Managing data from multiple sources manually can be time-consuming and error-prone.
- **Data Integration**: Combining data from different sources and formats into a unified system for analysis.
- **Data Transformation**: Cleansing and preparing data for analytics involves complex transformations.
- **Monitoring and Error Handling**: Ensuring the pipeline runs smoothly, with effective error management and recovery mechanisms.

This project addresses these issues by automating the entire data pipeline process, ensuring data is ingested, transformed, and stored efficiently and accurately.

## Goal

The primary goals of this project are:
1. **Data Ingestion**: Create a pipeline to pull data from various sources, such as CSV files in Azure Blob Storage.
2. **Data Transformation**: Implement transformations to cleanse and standardize the data, perform aggregations, and enrich the dataset.
3. **Data Storage**: Load the transformed data into Azure Data Lake Storage for further analysis and reporting.
4. **Automation**: Schedule the pipeline to run at specified intervals, with built-in error handling and retry policies.
5. **Monitoring and Alerts**: Monitor pipeline execution, track performance, and configure notifications for any issues.

----

## Solution Design

### Data Source Configuration

Connections are established to the data sources, which include:

- **Source 1**: A CSV file (`customer.csv`) containing customer details, stored in Azure Blob Storage.
- **Source 2**: A CSV file (`sales.csv`) containing sales details, stored in Azure Blob Storage.

These sources are configured in Azure Data Factory as **datasets**, and the pipeline will use these connections for ingesting data into the **Azure Data Lake Storage**.

---

### Pipeline Creation

A pipeline in Azure Data Factory orchestrates the entire data ingestion and transformation process. It pulls data from the source, transforms it, and loads it into the destination.

---

### Data Ingestion

The data ingestion process is managed through a series of activities that extract data from the configured sources:

- **Ingestion from Source 1**: The customer details from `customer.csv`.
- **Ingestion from Source 2**: The sales details from `sales.csv`.

The ingested data is loaded into the **Azure Data Lake Storage (ADLS)**, where it undergoes further transformation.

---

### Data Transformation

The transformation step includes cleansing and restructuring the ingested data to meet reporting needs:

1. **Customer Data Transformation**:
   - Removing duplicate records based on `CustomerID`.
   - Standardizing customer names by capitalizing the first letter of each name and converting the rest to lowercase.
   - Cleansing email addresses by removing leading or trailing spaces.

2. **Sales Data Transformation**:
   - Aggregating sales data to calculate total sales per product and region.
   - Calculating additional metrics such as average sales price and maximum quantity sold.

3. **Data Mapping & Joining**:
   - Mapping customer names to create a `FullName` field.
   - Joining customer and sales data using `CustomerID` to enrich the sales data.

4. **Filtering**:
   - Filtering out sales with quantities less than 10 and prices below $10.

5. **Date Manipulation**:
   - Extracting the year from `OrderDate` and adding it as a separate column.

---

### Data Storage

The transformed data is stored in **Azure Data Lake Storage** in a structured format, ready for analytics and reporting purposes. The storage configuration ensures easy access for downstream systems like **Azure Synapse Analytics** or **Power BI**.

---

### Data Orchestration

The pipeline activities are sequenced to ensure the correct order of execution. The pipeline includes dependencies to manage the flow from ingestion to transformation.

---

### Scheduling and Monitoring

- **Scheduling**: The pipeline is scheduled to run daily at **2 AM UTC** to ingest and transform the latest data.
- **Monitoring**: Azure Data Factory’s monitoring tools provide detailed tracking of pipeline execution, including run status, execution time, errors, and warnings. This ensures smooth operation and quick resolution of issues.

---

### Error Handling and Retry

The pipeline includes error handling mechanisms to manage potential failures:

- **Retry Policies**: For transient failures like network issues, retry policies are configured to automatically attempt the failed activities again.
- **Failure Notifications**: In case of repeated failures, notifications are sent to the data engineering team for manual intervention.

---

### Data Quality and Validation

Data quality checks are integrated to ensure data accuracy and integrity:

- **Validation Rules**: Checks are performed for missing or invalid values, data type consistency, and referential integrity.
- **Duplicate Handling**: Duplicates are removed based on a combination of key fields like `OrderID` and `CustomerID`.

---

### Notifications and Alerts

Alerts are configured to notify the data engineering team of any pipeline failures, data quality issues, or critical exceptions. Email notifications ensure that immediate action can be taken when required.

---

## Sample Data and Transformations

### Data Sources:
1. **Customer Data (customer.csv)**:
   - Columns: `CustomerID`, `FirstName`, `LastName`, `Email`, `Address`, `City`, `Country`
   
2. **Sales Data (sales.csv)**:
   - Columns: `OrderID`, `CustomerID`, `ProductID`, `Quantity`, `Price`, `OrderDate`, `Region`

### Sample Transformations:

1. **Customer Data Transformations**:
   - Remove duplicates based on `CustomerID`.
   - Create `FullName` by concatenating `FirstName` and `LastName`.
   - Clean email addresses by removing spaces and standardizing to lowercase.

2. **Sales Data Transformations**:
   - Group sales by `ProductID` and `Region` to calculate total sales.
   - Filter out sales with quantity less than 10 or price less than $10.
   - Calculate additional metrics such as average and maximum values.

---

## Tools Used

- **Azure Data Factory (ADF)**: To orchestrate the data pipeline.
- **Azure Data Lake Storage (ADLS)**: For storing the ingested and transformed data.
- **Azure Blob Storage**: To store the source data files (CSV files).
- **Azure SQL Database**: Optional for performing advanced data transformations (if needed).
- **Azure Functions**: Optional for custom data transformation logic.
- **SMTP/Email Service**: To send notifications and alerts.
- **Azure Monitor**: For pipeline execution monitoring and alerts.

---

## Conclusion

This data pipeline built using Azure Data Factory provides a scalable, automated solution for ingesting and transforming data from various sources into Azure Data Lake Storage. It ensures data quality, error handling, and monitoring, making it a reliable and efficient system for data-driven decision-making and reporting. The project can be easily extended to support additional data sources, more complex transformations, and new reporting requirements.

