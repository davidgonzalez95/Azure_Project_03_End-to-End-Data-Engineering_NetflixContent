# Azure End to End Data Engineering Netflix Data streaming Project

## Table of Contents  

1. [Project Description](#project-description)
2. [Technical Components](#technical-components)
3. [Data Architecture](#data-architecture)
4. [Azure Data Factory (Ingestion)](#azure-data-factory)
   - [Objective](#objective-adf)
   - [Pipeline Architecture](#pipeline-architecture)
   - [1- PL_Extract_Data](#pl_extract_data)
5. [Azure Databricks](#azure-databricks)
   - [Unity Catalog](#unity-catalog-databricks)
   - [Ingestion](#ingestion-databricks)
   - [Transformations](#transformations-databricks)

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

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source.png" alt="image" width="550" height="auto"> 
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_0.png" alt="image" width="550" height="auto">    

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_1.png" alt="image" width="480" height="auto">    

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_source_inside_2.png" alt="image" width="500" height="auto">    

     **2- Creation of sink connection:**
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink.png" alt="image" width="500" height="auto">    
     
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink_inside_1.png" alt="image" width="700" height="auto">   

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_copy_sink_inside_2.png" alt="image" width="500" height="auto">   

  - **Creation of a parameter inside of the pipeline:**
    
     **1- Create a JSON file:** It is used a JSON to create dynamic parameters that automate the extraction and loading of data. The structure of the JSON is broken down below:
       - **folder_name:** Target folder.  
       - **file_name:** Target file name and format for source and sink.

         Then it is uploaded it into our Data Lake in the parameters folder. [Format of JSON](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Dynamic_Pipeline.json)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_parameter.png" alt="image" width="480" height="auto">     
     
  - **Creation of forEach Activity and put inside the Dynamic Copy:** (Extract the values from the parameter which uses the json script)
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_forEach.png" alt="image" width="480" height="auto">

  - **Creation of Validation Activity:** (Check if the title folder exist in 00-raw folder)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_validation_activity.png" alt="image" width="250" height="auto">
     
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Azure_Data_Factory/PL_Extract_Data_validation.png" alt="image" width="750" height="auto">

  - **PL_Extract_Data results:**

     **Bronze Folder:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/storage_bronze.png" alt="image" width="450" height="auto">

## Azure Databricks <a name="azure-databricks"></a>
### Unity Catalog - Objective: <a name="unity-catalog-databricks"></a>
<p align="justify">To ensure secure and efficient data governance, <b>Unity Catalog</b> is utilized for managing credentials and access controls across different data layers. Unity Catalog provides a centralized approach to defining permissions, enabling fine-grained access control for users, groups, and service principals. Through its integration with cloud identity providers, it allows organizations to establish secure authentication mechanisms and enforce role-based access (RBAC). Additionally, Unity Catalog simplifies credential management by enabling secure connections to storage accounts, ensuring that only authorized entities can read or write data while maintaining compliance with enterprise security policies.</p>

#### **Steps:**

  - **Creation of an access databricks:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks.png" alt="image" width="480" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_connector.png" alt="image" width="480" height="auto">

  - **Creation of a credential:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_credential.png" alt="image" width="480" height="auto">

  - **Creation of external tables:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_raw_external_table.png" alt="image" width="480" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_bronze_external_table.png" alt="image" width="480" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_silver_external_table.png" alt="image" width="480" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_gold_external_table.png" alt="image" width="480" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Unity_Catalog/Access_databricks_checkpoint_external_table.png" alt="image" width="480" height="auto">

### Ingestion - Objective: <a name="ingestion-databricks"></a>
<p align="justify">The <b>00-raw container</b> will be constantly loading new Netflix titles files. To achieve this, an <b>Incremental Data Loading using AutoLoader</b> will be implemented, creating a <b>checkpoint</b> to track which files have been loaded and which have not. The <b>checkpoint</b> is stored in a <b>dedicated container separate from the data layers</b> to ensure data consistency and avoid unintended deletions due to lifecycle policies. This setup guarantees reliable tracking of processed files without interfering with the raw, silver, or gold layers, <b>as recommended by</b> <a href="https://learn.microsoft.com/en-us/azure/databricks/ingestion/cloud-object-storage/auto-loader/production" target="_blank">Microsoft's best practices</a>.</p>

<p align="justify">After performing checks on the average size of the files to be loaded, a duration of 2 minutes has been set for the process. Subsequently, a workflow will be created along with its respective trigger to automate and manage the data loading process efficiently.</p>

#### Steps:

  - **It tests how much time is needed to process a file**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_01_test.png" alt="image" width="450" height="auto">
    
  - **Creation of a workflow**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_01.png" alt="image" width="450" height="auto">
    
  - **Creation of a trigger**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_01_schedule.png" alt="image" width="450" height="auto">
    
  - **Workflow Result:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_01_results.png" alt="image" width="600" height="auto">

<p align="justify">The following notebook involves <b>reading and writing data</b> in a <b>data stream</b> using Apache Spark, specifically to work with <b>CSV files</b> stored in an <b>Azure Data Lake Storage (ADLS)</b>.</p>

[01_Bronze_AutoLoader](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Databricks/01_Bronze_AutoLoader.ipynb)

  - **Storage result:**

     **Bronze Folder:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/storage_bronze_with_titles.png" alt="image" width="450" height="auto">

     **Checkpoint-state Folder:**
    
     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/storage_checkpoint-state.png" alt="image" width="450" height="auto">

### Transformations - Objective: <a name="transformations-databricks"></a>
The **silver layer** has been implemented in 2 independent workflows: 

**1-Workflow:** All files are loaded except for the title files, and the reference notebooks are as follows:

[02_01_Silver_LookUp](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Databricks/02_01_Silver_LookUp.ipynb)

[02_02_Silver_forEach](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Databricks/02_02_Silver_forEach.ipynb)

#### Steps:

  - **Creation of LookUp task by using** [02_01_Silver_LookUp](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Databricks/02_01_Silver_LookUp.ipynb)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_01_LookUp.png" alt="image" width="350" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_01_LookUp_Settings.png" alt="image" width="700" height="auto">
    
  - **Creation of forEach task  by using** [02_02_Silver_forEach](https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Codes/Databricks/02_02_Silver_forEach.ipynb)

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_02_ForEach.png" alt="image" width="350" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_02_ForEach_Settings_01.png" alt="image" width="700" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_02_ForEach_Settings_02.png" alt="image" width="700" height="auto">

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_02_ForEach_Loop.png" alt="image" width="700" height="auto">
  
  - **Workflow Result:**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/Databricks/Workflows/databricks_workflow_02_Silver_results.png" alt="image" width="600" height="auto">

  - **Storage results (Silver Folder):**

     <img src="https://github.com/davidgonzalez95/Azure_Project_03_End-to-End-Data-Engineering_NetflixDatastreaming/blob/main/Pictures/storage_silver.png" alt="image" width="450" height="auto">
