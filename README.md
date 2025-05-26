DS-2002 Final Project: Dimensional Data Lakehouse for Rental Transactions

Project Overview

This project builds a dimensional Data Lakehouse for analyzing rental transactions, modeled on the structure of a retail/rental business process. It integrates batch and real-time data from multiple sources using Databricks and PySpark, and follows the Medallion Architecture (Bronze → Silver → Gold).

Business Process

We model a rental scenario (e.g., movies, products, equipment) with interactions between customers, stores, and products. Transactions (orders) are processed over time and used to produce analytical summaries.

Data Sources

| Source Type | Description                            | Data Used                  |
|-------------|----------------------------------------|----------------------------|
| MySQL       | Relational data (AdventureWorks views) | Customers dimension        |
| MongoDB     | NoSQL data (Northwind collection)       | Stores dimension, fact orders |
| CSV         | Flat files (local or DBFS)             | Products & Date dimensions |
| JSON        | Streaming order data                   | Fact table (Bronze layer)  |

Dimensional Schema

- `dim_customers` – From **MySQL** (AdventureWorks)
- `dim_stores` – From **MongoDB** (Northwind)
- `dim_products` – From **CSV**
- `dim_date` – From **CSV** generated using provided SQL
- `fact_orders` – From **JSON** (streaming into Bronze/Silver/Gold)


Architecture & Execution

- **Medallion Architecture**:
  - **Bronze**: Streaming ingestion of raw JSON files using Spark AutoLoader
  - **Silver**: Join fact with dimensions
  - **Gold**: Aggregate summaries by store and product category

- **Batch Processing**:
  - Dimensions from MySQL, MongoDB, and CSV loaded once at project start

- **Streaming (Mini-batch)**:
  - Three JSON files simulate streaming intervals: `northwind_orders_01.json`, `02.json`, `03.json`

Sample Business Queries

- Total rentals per store
- Revenue by product category
- Rentals by customer segment over time

Technologies Used

- PySpark (Databricks Runtime)
- Spark Structured Streaming (AutoLoader)
- Azure MySQL
- MongoDB (Atlas/local)
- Databricks File System (local paths used in testing)

Notes

- The original midterm project used the **Sakila** database. For this final, we switched to **AdventureWorks + Northwind** for better support of streaming and integration use cases.
- File paths are currently local but could be migrated to DBFS or cloud buckets.
- One dimension originally planned for MongoDB (e.g., products) was instead loaded via CSV due to technical issues with Atlas connectivity during streaming setup. The fallback source still meets the requirement for using multiple data formats and supports full pipeline integration.
- The `dim_products` table was created from a local JSON file (`film_data.json`) rather than a CSV file.

Submitted by Sonia Sadani for DS-2002 – Data Project 2 (Final)
