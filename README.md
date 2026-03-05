# 🏭 SQL Data Warehouse Project

This project demonstrates the design and implementation of a **modern SQL-based Data Warehouse** ready to extract information for Data driven decision making.
The goal is to showcase practical skills and knowledge in **SQL, Data Warehouse Architecture, ETL Pipelines and Data Modeling** as a portfolio project.

---

# 📜 Project Overview

This project includes the full lifecycle and explanation on each step on how I built this Data Warehouse, including visual representations of the structures, as well as each query used and the results inside PGAdmin.

### 🏗 Data Architecture

Designing a scalable **Data Warehouse** using the **Medallion Architecture**:

![Architecture](Step_2-Designing_Data_Architecture/WareHouse%20Architecture.jpg)

1. Bronze Layer: Stores raw data as-is from the source systems. Data is ingested from CSV Files into PostgreSQL.

2. Silver Layer: Data cleansing, standardization, and normalization processes to prepare data for analysis.

3. Gold Layer: Business-ready data modeled into a star schema required for reporting and analytics.

### 🔄 ETL Pipelines

Building SQL based ETL processes to:

* Extract data from source systems
* Transform and clean the data
* Load structured data into the warehouse layers

### 🧱 Data Modeling

Designing **Fact and Dimension tables** to support analytical workloads and reporting.

---

# 🛠️ Technologies Used

| Tool           | Purpose                                      |
| -------------- | -------------------------------------------- |
| **PostgreSQL** | Data warehouse database                      |
| **pgAdmin**    | Database management and query development    |
| **Draw.io**    | Visualizations creation |
| **VSCode** | Code and documentation writing                     |
---

# 🔭 Project Steps

### 1️⃣ Requirements Analysis

Understanding the business requirements and defining analytical goals.

### 2️⃣ Designing the Data Architecture

Designing the Medallion architecture and defining data flows.

### 3️⃣ Data Warehouse Setup

Setting the naming convention and creating the schemas for the Data Warehouse.

### 4️⃣ Building the Data Warehouse

Developing ETL pipelines and building the Bronze, Silver, and Gold layers.
