# Covid19 Project based on Europe Data End2End
This project mainly focus on the Europe Covid19 Data. 
The data has been taken from EuroStat and European Centre for Disease Prevention and Control (ECDC) wesbsites.

* The main objective of this project is to use raw data to perform **ETL** (Extract, Transform and Load) using  **Azure Data Factory**.

### Types of files and data used in the project are listed below:

File Name | Description of File
-------- | ---------
1.population_by_age.tsv.gz (EuroStat/Blob) | This is a zipped file that contains the population data of countries.
2.cases_deaths.csv (ECDC) | This csv file contains the weekly emerging Covid Cases and Deaths
3.hospital_admissions.csv (ECDC) | This csv file contains the Daily Hospital Admissions, Daily ICU admissions, Weekly Hospital Admissions per 100k, Weekly ICU Admissions per 100k.
4.lookups (Blob) | Other miscellaneous files like Calendar Lookup and Country lookup files.

### The below image represents the solution architechture for this project :
![image](https://user-images.githubusercontent.com/79434863/180002640-08b7bb82-116d-4960-a17a-301f7c240c43.png)

* **Linked Services** are craeted to connect to the Blob Storage, ADLS raw and processed containers, and SQL database
* **DataSets** are created with respect to source and target files.
* **Pipelines** are created to execute Copy Activities, Data Flows, and to Orchestrate pipelines

### Step 1: 
* *population_by_age.tsv.gz (zipped_file)* has been uploaded into the Azure Blob Storage manually.

3 pipelines are created
1. a **Copy Activity** that helps to Copy the Population Data from Blob storage to the ADLS raw container
2. a **Databricks Notbook** that helps to Copy the Population Data from the ADLS raw container to the ADLS processed container
3. a **Copy Activity** that helps to Copy the Population Data from the ADLS processed container to the SQL Database

![image](https://user-images.githubusercontent.com/79434863/180031742-cc2f0645-651b-4c78-9d4d-9a9660a30de3.png)


### Step 2: 
* The 2 source files - cases_and_deaths, hospial_admissions that are accessible from ECDC Website - https://opendata.ecdc.europa.eu 
* A parameterized linked service, dataset and pipeline are created to extract data from the ECDC Website and load to ADLS raw container
* parameters are passed from JSON file with the help of Lookup acivity and ForEach activity

![image](https://user-images.githubusercontent.com/79434863/180034148-285933e7-3e0e-427e-b92b-c1ab9eb82ee0.png)
![image](https://user-images.githubusercontent.com/79434863/180034330-8f84a0f6-378d-42d9-a6ba-5f170620520c.png)


### Step 3:
* The 2 source files - cases_and_deaths, hospial_admissions that are present in raw container are tarnsformed using Dataflows in ADF and loaded to ADLS processed container
* Various transformations like Lookup, Join, Filter, Sort, Conditional split, Aggregrate, Derived Column, Pivot, Surrogate Key are used basd on Bussiness logic

cases_and_deaths
![image](https://user-images.githubusercontent.com/79434863/180036961-ca7c484f-fb68-41f6-87fb-f47395e898fc.png)
hospial_admissions
![image](https://user-images.githubusercontent.com/79434863/180036991-b1271701-e738-4fb4-9395-daa3fda183d1.png)


### Step 4: 
* Finally the data from ADLS processed container is loaded into the SQL database using **Copy Activity**
* So that Data Analysts can query data directly from the database or Use it for PowerBi/Tableau or any other data visualization tool for analysis purpose.

cases_and_deaths
![image](https://user-images.githubusercontent.com/79434863/180038252-9b8e0a5f-e776-41df-bcd8-467624ef0915.png)
hospial_admissions
![image](https://user-images.githubusercontent.com/79434863/180038284-bc9b9b10-5d67-4342-abbe-5fd65d7d6f55.png)



This project has been taken inspiration from one of the course in Udemy: https://www.udemy.com/course/learn-azure-data-factory-from-scratch/
