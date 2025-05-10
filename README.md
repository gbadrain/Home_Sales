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
![Screenshot 2025-05-10 at 10 19 13 AM](https://github.com/user-attachments/assets/6f98a1e4-32ca-4192-bdbc-5e9bebd63386)

* **Homes with 3 beds, 3 baths per build year**
  
![Screenshot 2025-05-10 at 10 19 46 AM](https://github.com/user-attachments/assets/b22112a9-68cf-4ec9-9728-54c1f958385b)

* **Homes with 3 beds, 3 baths, 2 floors, ≥ 2000 sqft per build year**
  
![Screenshot 2025-05-10 at 10 21 40 AM](https://github.com/user-attachments/assets/20381bd2-8aa1-4ee1-b1b5-c8f02a4c7a6a)


* **Average price per view rating (≥ $350,000)**

![Screenshot 2025-05-10 at 11 25 09 AM](https://github.com/user-attachments/assets/12de8977-53b9-471a-8b03-4c3b8f584785)
![Screenshot 2025-05-10 at 11 25 30 AM](https://github.com/user-attachments/assets/9b25f3b4-bde1-44e1-8d43-5eec98a754be)


### 3. Performance Optimization
* **Caching the temporary table**
  ```python
  spark.sql("CACHE TABLE home_sales")
  ```
  ![Screenshot 2025-05-10 at 11 29 28 AM](https://github.com/user-attachments/assets/c31a01fb-99b4-4231-bbdb-151b8707157f)
  ![Screenshot 2025-05-10 at 11 30 02 AM](https://github.com/user-attachments/assets/5288a711-14ca-42b1-afb7-dd53f85dc352)

* **Partitioning parquet data & verifying performance improvements**

### 4. Uncaching & Validation
* **Uncache table:**
  ```python
  spark.sql("UNCACHE TABLE home_sales")
  ```
  ![Screenshot 2025-05-10 at 11 46 42 AM](https://github.com/user-attachments/assets/15027e44-1269-4eba-aa81-4a65ddbfbdcd)
  ![Screenshot 2025-05-10 at 11 47 21 AM](https://github.com/user-attachments/assets/83ce0538-e08c-40f4-b51f-0d3f6d391ec1)




  **Analysis**
The query logic remains consistent, successfully filtering views where avgPrice >= 350000.

Caching vs. Non-Caching Comparison:

Prior cached execution: 0.67 seconds

Latest execution: 0.82 seconds

While slightly slower, it's still in the optimized range compared to earlier non-cached runs.

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

*Recommended Book: High Performance Spark*  
Apache Spark Performance Tuning  
Best practices to optimizing query performance  
Parquet and Partitioning Best Practices  


## Acknowledgments
Special thanks to the **University of Oregon Data Analytics Boot Camp** for providing the structured learning path for **PySpark analysis** and **SQL query optimization**.

## Contact

* **Name**: Gurpreet Singh Badrain
* **Role**: Market Research Analyst & Aspiring Data Analyst
* **GitHub**: https://github.com/gbadrain
* **LinkedIn**: http://linkedin.com/in/gurpreet-badrain-b258a0219
* **Email**: gbadrain@gmail.com

