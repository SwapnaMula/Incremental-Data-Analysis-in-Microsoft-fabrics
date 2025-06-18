# Incremental Data Analysis in Microsoft Fabric

## Project Overview:

This project focuses on implementing an incremental data loading mechanism using Microsoft Fabric with a Lakehouse architecture. The goal is to simulate real-time sales data ingestion from an Oracle database, process it using PySpark in notebooks, and present the transformed insights through a Power BI dashboard. The solution leverages the medallion architecture (Bronze-Silver-Gold) and includes automation, deduplication, and reporting.

## Business Requirements:

The business aims to analyze large-scale sales data with daily growth and ensure data is updated incrementally without full reloads. Key requirements include:

* **Efficient Incremental Loading** – Load only newly added rows daily to improve performance and scalability.
* **Clean and Structured Storage** – Apply medallion architecture for data refinement.
* **Real-Time Insights** – Use Power BI to reflect the latest sales data and trigger alerts based on thresholds.
* **Data Quality Handling** – Deduplicate records and handle null values before reporting.

## Solution Overview:

To address these requirements, the solution consists of:

## Data Ingestion:

* Generated synthetic sales data in Python, simulating **5,000 new records daily**.
* Inserted this data into an **Oracle database** using credentials.
* Created a **Microsoft Fabric Lakehouse** to serve as the data storage layer.
* Built a pipeline using **Copy Data activity** to move **raw data** from Oracle DB to **Lakehouse Files (Bronze layer)** as CSV files.

## Incremental Load Pipeline:

* Developed a second pipeline to handle **incremental data** loading.
* Used a **timestamp filter and parameterized queries** to fetch only the latest 5,000 records.
* Saved incremental data as a separate file in the **Bronze layer**.

## Data Transformation:

**Bronze to Silver (Notebook 1):**

* Used **PySpark notebooks** to read both full and incremental data files from Lakehouse.
* Merged the datasets and **removed duplicates** to maintain historical integrity.
* Output merged data (e.g., 1,00,000 base + 5,000 new = 1,05,000 records) to **Silver layer Lakehouse** Files as a single CSV.

**Silver to Gold (Notebook 2):**

* Loaded cleaned data from Silver layer.
* Performed transformations:
    * **Changed column data types**.
    * **Removed null values**.
    * **Added necessary derived columns**.
* Created **fact and dimension tables**:
    * Dimensions: **dim_product, dim_region, dim_date, dim_customer**
    * Fact: **fact_sales**
* Saved output both to **Lakehouse Tables** and **Lakehouse Files**, customizing filenames like **dim_customer.csv, dim_region.csv,** etc., to replace the default **part-0000** style.

## Data Storage and Reporting:

* Connected the **Lakehouse Tables** to **Power BI** using **SQL Analytics Endpoint**.
* Developed interactive reports visualizing:
      * Sales by region, gender, and category
      * Time-based sales trends
* Published reports to Power BI Service.

## Automation and Alerts:

* **Scheduled pipelines** to run daily for continuous data ingestion and transformation.
* Set up **Power BI data-driven alerts** to send email notifications when key metrics exceed defined thresholds (e.g., daily sales target exceeded).

## Technology Stack:

* **Microsoft Fabric** – Unified platform for data ingestion, transformation, and storage.
* **Oracle Database** – Source system for sales data.
* **Lakehouse** – Stores data using the medallion architecture (Bronze/Silver/Gold).
* **PySpark Notebooks** – For transformations and business logic implementation.
* **Power BI** – Final reporting and visualization layer.
* **Power BI Alerts** – To notify users about critical data thresholds.

## Setup Instructions:

**Step 1: Environment Setup**

* Create a **Lakehouse** with Bronze, Silver, and Gold folders.
* Configure **Oracle DB connectivity** and ensure data is accessible.

**Step 2: Data Ingestion**

* Develop a Copy Data pipeline to load:
      * **Full data** into Bronze.
      * **Incremental data** using timestamp-based query into Bronze.

**Step 3: Transformation**

* Use two PySpark notebooks:
     * First to merge raw and incremental data and write to Silver.
     * Second to clean Silver data and structure it into fact/dim tables in Gold.

**Step 4: Reporting**

* Connect Power BI to the SQL endpoint of the Lakehouse.
* Build and publish a report using visuals like bar charts, filters, and slicers.

**Step 5: Automation and Alerts**

* Automate pipeline runs daily.
* Set up data alerts in Power BI for key KPIs.

![image](https://github.com/user-attachments/assets/2a116de2-739a-4eb7-ba3e-5329812706ba)

## Conclusion:

This project demonstrates an efficient incremental loading framework within Microsoft Fabric using Lakehouse architecture. It showcases best practices for handling big data with automation, quality checks, and real-time business insights via Power BI—making it a scalable and production-ready data engineering solution.
