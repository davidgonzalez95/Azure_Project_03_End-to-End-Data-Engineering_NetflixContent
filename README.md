# Azure End to End Data Engineering Netflix Data streaming Project

## Table of Contents  

1. [Project Description](#project-description)
2. [Technical Components](#technical-components)
3. [Data Architecture](#data-architecture)
4. [Azure Data Factory (Ingestion)](#azure-data-factory)
   - [Objective](#objective-adf)
   - [Pipeline Architecture](#pipeline-architecture)
   - [1- PL_Extract_Data](#pl_extract_data)
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

<p align="justify">This project focuses on building a comprehensive, <b>end-to-end Azure Data Engineering solution</b> that seamlessly <b>integrates streaming and batch data ingestion, transformation, and analytics</b>. It follows the principles of the <b>Medallion Architecture</b>, ensuring a <b>structured and scalable approach</b> to data processing. By implementing this architecture, the solution facilitates an efficient and organized data flow, transitioning from raw ingestion to progressively refined and enriched datasets. These optimized datasets will ultimately support <b>real-time and batch analytics</b>, enabling stakeholders to derive meaningful insights and make <b>data-driven decisions with confidence</b>.
</p>

## Technical Components <a name="technical-components"></a>
 
 - **GitHub:** Data source.
 - **Azure Data Lake:** Centralized storage for transformed data.
 - **Azure Data Factory (ADF):** Data ingestion.
 - **Databricks with Delta Live Tables:** Scalable data transformation with real-time processing and automated batch/streaming management.
 - **Azure Synapse Analytics:** Data warehouse.
 - **Power BI:** Reporting.

## Data Architecture <a name="data-architecture"></a>

<img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Architecture_Project_03.png" alt="image" width="550" height="auto">

## Azure Data Factory (Ingestion) <a name="azure-data-factory"></a>
### Objective <a name="objective-adf"></a>

<p align="justify">The purpose of this section in the README is to explain the pipeline implemented in Azure Data Factory, detailing their structure and functionality for data <b>ingestion</b>. The main goal is to highlight how the architecture is designed to be efficient.</p>

#### **1- PL_Extract_Data:** <a name="pl_extract_data"></a>

<p align="justify">Extracting all Netflix files on GitHub except for the titles file already stored in the 00-raw Container, the extraction is done using a <b>dynamic copy parameter to extract the URL path and destination of the file</b> inside a forEach activity that reads the corresponding URL and loads the data via a parameter. Before that, a validation of the existence of the title file in the 00-raw container is carried out.</p>

<img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_architecture.png" alt="image" width="500" height="auto">

##### **Steps:**
  - **Creation a Dynamic Copy Activity:**
    
     **1- Creation of source connection:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source.png" alt="image" width="500" height="auto"> 
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_0.png" alt="image" width="500" height="auto">    

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_1.png" alt="image" width="350" height="auto">    

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_2.png" alt="image" width="500" height="auto">    

     **2- Creation of sink connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink.png" alt="image" width="500" height="auto">    
     
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink_inside_1.png" alt="image" width="500" height="auto">   

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink_inside_2.png" alt="image" width="500" height="auto">   

  - **Creation of a parameter inside of the pipeline:**
    
     **1- Create a JSON file:** It is used a JSON to create dynamic parameters that automate the extraction and loading of data. The structure of the JSON is broken down below:
       - **folder_name:** Target folder in the ADLS Gen2 bronze layer.  
       - **file_name:** Target file name and format for source and sink.

         Then it is uploaded it into our Data Lake in the parameters folder. [Format of JSON](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Dynamic_Pipeline.json)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_parameter.png" alt="image" width="480" height="auto">     
     
  - **Creation of forEach Activity and put inside the Dynamic Copy:** (Extract the values from the parameter which uses the json script)
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_forEach.png" alt="image" width="480" height="auto">

  - **Creation of Validation Activity:** (Check if the title folder exist in 00-raw folder)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_validation_activity.png" alt="image" width="480" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_validation.png" alt="image" width="480" height="auto">

  - **PL_Extract_Data results:**

     **Bronze Folder:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/storage_bronze.png" alt="image" width="500" height="auto">
