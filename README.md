# Vietnam Weather Data Warehouse & Rainfall Forecasting

A data engineering and analytics team project that builds an end-to-end pipeline for Vietnam's weather data to support climate research. It addresses the challenges of fragmented meteorological data by turning raw historical records into multidimensional dashboards and rainfall forecasting models.

## Overview

The project proposes a comprehensive data warehouse and machine learning workflow for meteorological analysis. Instead of analyzing flat CSV files manually, the team extracted historical weather data, designed an automated ETL pipeline, built an OLAP cube, visualized regional climate trends, and demonstrated how machine learning can predict rainfall patterns.

The core analytical problem centers on the difficulty of tracking and predicting climate variations, which can cause:
* lack of visibility into regional temperature and humidity correlations;
* difficulties in analyzing long-term wind and weather state patterns;
* challenges in forecasting heavy rainfall and extreme weather events;
* weak coordination in preparing data for scalable machine learning models.

## Objectives

* Analyze historical weather data across 40 provinces in Vietnam.
* Build an automated ETL pipeline to clean and load data into a Star Schema.
* Develop an OLAP cube and MDX queries for multidimensional climate analysis.
* Build and compare machine-learning models to forecast total rainfall.
* Demonstrate weather trends through interactive Power BI and Looker Studio dashboards.

## Scope

* **Analytical scope:** Temperature, Rainfall, Wind, Humidity, and Weather Status.
* **Data scope:** 181,980 daily weather records from 2009-2021.
* **Technical scope:** SSIS-based ETL, SSAS cubes, Python forecasting notebooks, and BI dashboard design.

## Analytical workflow

Raw weather data -> ETL pipeline (SSIS) -> Star Schema Data Warehouse
                                        -> OLAP Cube (SSAS) -> MDX Queries -> BI Dashboards
                                        -> Feature Engineering -> Model comparison -> Rainfall Forecast

The project compares standard OLAP multidimensional analysis with predictive machine learning. The BI prototype focuses on providing visual decision support for climate monitoring, while the ML models attempt to capture complex, non-linear weather patterns.

## Data and results

* 181,980 weather records with 21 attributes.
* Baselines: Random Forest, LightGBM.
* Main model: CatBoost.
* Best reported model:

| Model | R² | RMSE | MAPE |
| :--- | :--- | :--- | :--- |
| CatBoost | 69.91% | 2,762.88 | 59.76% |

*Metrics are team experiment results on the project's prepared dataset.*

## BI prototype

Dashboard results support:
* monitoring average rainfall and temperature across different regions;
* analyzing the inverse correlation between temperature and humidity;
* tracking the frequency of specific wind directions and weather states;
* visual comparisons using Sankey diagrams, Heatmaps, and Time Series.

Main demo features include cross-filtering by province/year, geographical rainfall distribution, and macro-trend analysis from 2009 to 2021.

## Repository guide

* `1_SSIS_ETL/`: ETL packages and data flow tasks.
* `2_SSAS_Cube/`: Cube design, hierarchies, and MDX queries.
* `3_Dashboards/`: Power BI (`.pbix`) and Looker Studio reports.
* `4_Data_Mining/`: EDA, forecasting, and model evaluation notebooks.
* `Dataset/`: Input-data note.

## Tech stack

SQL Server, SSIS, SSAS, Power BI, Looker Studio, Python, pandas, LightGBM, Random Forest, CatBoost.

## Team

* Võ Hồ Trung Quân - Data Engineer / Data Analyst
* Trần Đình Trung Hiếu - Data Engineer / Data Analyst

*Built for the Data Warehouse and OLAP course.*

## Future improvements

* Automate the data pipeline to fetch real-time weather APIs instead of static CSVs.
* Extend forecasting to include extreme weather event classification (e.g., storms).
* Explore additional deep learning models for more complex spatio-temporal weather patterns.
* 
