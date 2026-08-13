# Decoding Vietnam's Climate: An End-to-End Data Warehouse & Machine Learning Project

Vietnam's climate is incredibly diverse and increasingly unpredictable. As data engineering students, we wanted to go beyond simple spreadsheets and build a robust, scalable architecture to process and analyze 12 years of meteorological data across 40 provinces. 

This repository contains our complete journey: from designing a centralized Data Warehouse and writing complex multidimensional queries, to building interactive dashboards and training machine learning models to forecast rainfall.

## The Challenge
Meteorological data is naturally noisy, highly seasonal, and spatially complex. Working with the raw dataset (over 181,000 daily records from 2009 to 2021) presented several challenges:
* **Fragmentation:** Data needed heavy cleaning, deduplication, and restructuring into a format optimized for fast analytical queries.
* **Multidimensionality:** Tracking climate changes requires slicing data by time (Year/Quarter/Month) and geography (Region/Subregion/Province) simultaneously.
* **Non-linear Patterns:** Rainfall is notoriously hard to predict using simple linear methods due to the complex interplay of humidity, pressure, and wind.

## Our Journey (The Technical Pipeline)

### 1. Building the Single Source of Truth (ETL & Data Warehousing)
Instead of querying flat files, we designed a **Star Schema** Data Warehouse in SQL Server. We built an automated ETL pipeline using **SSIS** to extract the raw CSVs, apply transformations (Derived Columns for time extraction, Sorting to remove duplicates), and load the clean data into a centralized `FACT_WEATHER` table surrounded by 4 dimensions.

### 2. Slicing and Dicing the Data (OLAP)
To answer complex business questions quickly, we constructed an OLAP Cube using **SSAS**. By defining hierarchies (e.g., Region -> Subregion -> Province) and writing 15 advanced **MDX queries**, we could easily track anomalies, such as which provinces experienced a decrease in rainy days between 2020 and 2021, or the dominant wind directions during peak monsoon seasons.

### 3. Visual Storytelling (Business Intelligence)
We connected our OLAP cube to **Power BI** and **Looker Studio** to bring the numbers to life. Our dashboards provide visual insights into:
* Regional rainfall distribution using Sankey diagrams (highlighting Central Vietnam as the peak rainfall region).
* The inverse correlation between temperature and humidity through scatter plots and heatmaps.
* Historical temperature trends spanning over a decade.

### 4. Predicting the Unpredictable (Machine Learning)
We didn't stop at historical reporting. We exported our processed data into Python to predict total monthly rainfall. After handling the right-skewed distribution and engineering time-lag features (`rain_lag12`), we tested several tree-based ensemble models. 

**Model Performance on Test Set:**

| Algorithm | R² Score | RMSE |
| :--- | :--- | :--- |
| Random Forest | 68.22% | 2,839.32 |
| LightGBM | 67.54% | 2,869.92 |
| **CatBoost (Winner)** | **69.91%** | **2,762.88** |

*CatBoost proved to be the most resilient model in capturing the non-linear, highly seasonal nature of Vietnam's rainfall.*

## Repository Structure
To make navigation easy, the project is organized into modular phases:
* `0_Dataset/`: The starting point (raw meteorological CSV data).
* `1_SSIS_ETL/`: Visual Studio solution containing the Data Flow tasks and connection managers.
* `2_SSAS_Cube/`: The OLAP cube design, hierarchical dimensions, and calculated measures.
* `3_MDX_and_Excel/`: A dedicated folder containing 15 advanced `.mdx` query scripts and `.xlsx` files demonstrating multidimensional analysis via PivotTables and PivotCharts.
* `4_Dashboards/`: Visual reports and dashboard files (`.pbix` files, Looker Studio references, and PDF exports).
* `5_Data_Mining/`: Jupyter notebooks detailing the EDA, feature engineering, and machine learning model training processes.

## Tech Stack
* **Storage & Processing:** SQL Server, SQL Server Integration Services (SSIS), SQL Server Analysis Services (SSAS).
* **Analytics & Visualization:** MDX, Power BI, Looker Studio.
* **Data Science:** Python, Pandas, Scikit-learn, LightGBM, CatBoost.

## The Team
* **Võ Hồ Trung Quân** - Data Engineer / Data Analyst
* **Trần Đình Trung Hiếu** - Data Engineer / Data Analyst

*This project was developed as a capstone for the Data Warehouse and OLAP course at the University of Information Technology (UIT).*
