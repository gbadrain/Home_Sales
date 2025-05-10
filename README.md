# Home Sales Analysis using SparkSQL

## Overview
This repository contains a Spark-based data analysis project focused on home sales data. Using **PySpark**, the project evaluates trends in **home pricing, views, and other key metrics** by leveraging SQL queries, caching strategies, and partitioning for performance improvements.

* **Goal:** Extract insights from home sales data using SparkSQL.
* **Approach:** Use temporary tables, SQL queries, caching mechanisms, and data partitioning for efficient computations.
* **Dataset:** `home_sales_revised.csv` from an **AWS S3 bucket**.

## Results & Implementation

### 1. Data Loading & Preparation
* **Target Dataset:** `home_sales_revised.csv`
* **Spark SQL Temporary Table:** Created for structured analysis
  ```python
  home_sales_df.createOrReplaceTempView("home_sales")
  ```
* **Partitioning Used:** Data partitioned by `date_built` for efficient querying.

### 2. Key Queries

#### Average Home Prices
* **Four-bedroom homes per year:**
  ```sql
  SELECT year(date_sold) AS year_sold, ROUND(AVG(price), 2) AS avg_price
  FROM home_sales
  WHERE bedrooms = 4
  GROUP BY year_sold
  ORDER BY year_sold;
  ```
* **Homes with 3 beds, 3 baths per build year**
* **Homes with 3 beds, 3 baths, 2 floors, ≥ 2000 sqft per build year**
* **Average price per view rating (≥ $350,000)**

### 3. Performance Optimization
* **Caching the temporary table**
  ```python
  spark.sql("CACHE TABLE home_sales")
  ```
* **Partitioning parquet data & verifying performance improvements**

### 4. Uncaching & Validation
* **Uncache table:**
  ```python
  spark.sql("UNCACHE TABLE home_sales")
  ```
* **Check cache status:**
  ```python
  spark.catalog.isCached("home_sales")
  ```

## Project Files & Structure
```
HOME_SALES/
├── .ipynb_checkpoints/
├── .vscode/
├── home_sales/
│   ├── date_built=2010/
│   │   ├── .part-00000-76e4...  # Parquet file
│   │   ├── .part-00001-76e4...  # Parquet file
│   ├── date_built=2011/
│   ├── date_built=2012/
│   ├── date_built=2013/
│   ├── date_built=2014/
│   ├── date_built=2015/
│   ├── date_built=2016/
│   ├── date_built=2017/
│   ├── _SUCCESS
│   ├── ._SUCCESS.crc
├── Home_Sales_Colab.ipynb
├── Home_Sales.ipynb
├── README.md
```

## Getting Started

### Prerequisites
* **Python 3.8+**
* **PySpark**
* **AWS S3 access (for dataset retrieval)**

### Installation
```bash
# Clone the repository
git clone https://github.com/gbadrain/Home_Sales.git

# Install PySpark
pip install pyspark
```

### Usage
* Open `Home_Sales.ipynb` in **Jupyter Notebook** or **Databricks**.
* Run SQL queries to extract insights.
* Compare runtimes of **cached vs. uncached** operations.

## Sources of Help
* **PySpark Documentation**
* **Microsoft Copilot** for troubleshooting
* **UO Boot Camp resources** on **Big Data Resources**
Recommended Book: High Performance Spark
Apache Spark Performance Tuning
Best practices to to optimizing query performance 
Parquet and Partitioning Best Practices

## Acknowledgments
Special thanks to the **University of Oregon Data Analytics Boot Camp** for providing the structured learning path for **PySpark analysis** and **SQL query optimization**.

## Contact
**Gurpreet Singh Badrain**
* **Role:** Math Teacher & Aspiring Data Analyst
* **GitHub:** gbadrain
* **LinkedIn:** Gurpreet Badrain
* **Email:** gbadrain@gmail.com
