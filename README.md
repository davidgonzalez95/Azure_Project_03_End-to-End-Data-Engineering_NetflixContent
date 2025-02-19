# Azure End to End Data Engineering Netflix Content Project

## Table of Contents  

1. [Project Description](#project-description)
2. [Technical Components](#technical-components)
3. [Data Architecture](#data-architecture)
4. [Azure Data Factory (Ingestion and Orchestration)](#azure-data-factory)
   - [Objective](#objective-adf)
   - [Pipeline Architecture](#pipeline-architecture)
   - [1- PL_Extract_Raw_Data](#pl_extract_raw_data)
   - [2- PL_Trans_Load](#pl_trans_load)
5. [Azure Databricks (Transformation)](#azure-databricks)
   - [Objective](#objective-databricks)
   - [Considerations](#considerations)
   - [Development and Production Notebooks Overview](#development-and-production)
   - [Development Notebook](#development-notebook)
   - [Production Notebook](#production-notebook)
6. [Azure Synapse Analytics (Serving)](#azure-synapse-analytics)
   - [Objective](#objective-synapse)
   - [Steps](#steps-synapse)
7. [Power BI (Visualization)](#power-bi)
   - [Objective](#objective-powerbi)
   - [Steps](#steps-powerbi)
   - [Visualizations](#visualizations)

## Project Description

<p align="justify">This project focuses on building a comprehensive, <b>end-to-end Azure Data Engineering solution</b> that seamlessly integrates data ingestion, transformation, and analytics. It is designed to follow the principles of the <b>Medallion Architecture</b>, ensuring a structured and scalable approach to data processing. By implementing this architecture, the solution facilitates an efficient and organized data flow, transitioning from raw ingestion to progressively refined and enriched datasets. These optimized datasets will ultimately support advanced analytics and reporting, enabling stakeholders to derive meaningful insights and make data-driven decisions with confidence. 
</p>

## Technical Components <a name="technical-components"></a>
 
 - **GitHub:** Data source.
 - **Azure Data Lake:** Centralized storage for transformed data.
 - **Azure Data Factory (ADF):** Data integration and orchestration platform.
 - **Databricks:** Data transformation.
 - **Azure Synapse Analytics:** Data analysis.
 - **Power BI:** Visualization and reporting.

## Data Architecture <a name="data-architecture"></a>

<img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixContent/blob/main/Pictures/Architecture_Project_03.png" alt="image" width="550" height="auto">
