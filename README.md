# Azure Databricks ELT Pipeline to Delta Lake

This project documents a foundational **Extract, Load, Transform (ELT)** pipeline built on the Microsoft Azure data platform, utilizing **Azure Databricks** and **PySpark** to process semi-structured data and store the result in an optimized **Delta Lake** format.

---

## 🎯 Project Goal

The primary goal of this activity was to simulate a real-world Big Data processing workflow by:
1.  Establishing secure connectivity between the compute engine (Databricks) and cloud storage (ADLS Gen2).
2.  Implementing PySpark logic for data quality and feature engineering.
3.  Aggregating data to generate key business metrics.
4.  Persisting the final, clean, aggregated dataset into the Delta Lake format, preparing it for downstream analytical consumption (e.g., Power BI).

---

## 🛠️ Technology Stack

| Component | Role | Concept Mastered |
| :--- | :--- | :--- |
| **Cloud Platform** | Azure | Infrastructure provisioning and resource management. |
| **Data Lake** | ADLS Gen2 (Hierarchical Namespace) | Scalable, performant cloud storage optimized for Big Data analytics. |
| **Compute Engine** | Azure Databricks / Apache Spark | Distributed processing engine for running large-scale transformations. |
| **Language** | PySpark (Python) | Code used for implementing the ELT logic. |
| **Data Format** | **Delta Lake** | Open-source storage layer that brings ACID properties (Atomicity, Consistency, Isolation, Durability) and schema enforcement to the Data Lake. |

---

## ⚙️ Pipeline Execution Steps

The pipeline was executed within an Azure Databricks Notebook, attached to a provisioned Spark Cluster.

### 1. Extraction and Loading (E & L)

* **Authentication:** The Databricks Cluster was granted access to the ADLS Gen2 Storage Account using the **Access Key** via `spark.conf.set`.
* **Ingestion:** A large CSV/JSON file (simulating Big Data) was loaded directly from the ADLS Gen2 path (`abfss://`) into a Spark **DataFrame** (`df_raw`).

### 2. Transformation (T)

PySpark transformations were applied to the raw DataFrame (`df_raw`):

* **Filtering:** Rows with invalid data (`like_rate` being NULL or zero) were removed.
* **Feature Engineering:** A new metric, `engagement_score`, was calculated using existing columns (`like_rate * 100 + text_richness`).
* **Aggregation:** Data was grouped by **`platform`** and **`region`** to calculate crucial metrics like the **`avg_views_per_day`** and **`max_engagement_score`**.

### 3. Persistence into Delta Lake

* The final aggregated DataFrame (`df_aggregated`) was written to a specific path in ADLS Gen2 (`/curated-data/platform_summary`).
* The output format was explicitly set to **Delta Lake** (`.write.format("delta").mode("overwrite").save(path)`). This action creates the optimized Parquet data files and the transactional `_delta_log` directory.

---

## 📈 Next Steps (For Analytical Consumption)

The successfully created Delta Lake table is now available for direct querying and consumption by downstream analytical tools. The next phase of the project involves connecting **Power BI** to this Delta table and building advanced analytical dashboards using sophisticated **DAX measures** like `CALCULATE()` and Time Intelligence.
