## Data Integration Pipelines for NYC Payroll Data Analytics

### Project Overview
The City of New York would like to develope a Data Analytics platform on Azure Synapse Analytics to accomplich two primary objectives:
1. Analyze how th City's financial resources are allocated and how much of the City's budget is being devoted to overtime.

2. Make the data available to the interested public to show how the City's budget is being spent on salary and overtime pay for all municipal employees.

In this project as a Data Engineer I created high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operations. My Team also includes the city's Quality Assurance experts who will test the pipelines to find any errors and improve overall data quality.

The sources data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Aure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies

![image](https://user-images.githubusercontent.com/58150666/235501206-a2840698-cdd1-4b1f-b28a-6f31d405f02a.png)









## **Step 4:** Create Data Flows

1. **In Azure Data Factory, create the data flow to load 2021 Payroll Data to SQL DB transaction table (in the future NYC will load all the transaction data into this table).**

    * Create a new data flow
    * Select the dataset for the 2021 payroll file as the source
    * Select the sink dataset as the payroll table on SQL DB
    * Make sure to reassign any missing source to target mappings
  
  2. **Create Pipeline to load 2021 Payroll data into transaction table in the SQL DB**
    * Create the new pipeline
    * Select the data flow to load the 2021 file into SQLDB
    * Trigger the pipeline
    * Monitor the pipeline
    * Take a screenshot of the Azure Data Factory screen pipeline run after it has finished 
    * Make sure the data is successfully loaded into the SQL DB table

3. **Create data flows to laod the data from the data lake files into the Synapse Analytics data tables**
    * Create the data flows for laoding Employee, Title, and Agency files into corresponding SQL pool tables on Synapse Analytics
    * For each Employee, Title, and Agency file data flow, sink the data into each target Synapse table

4. **Create a data flow to load 2021 data from SQL DB to Syanpse Analytics**
5.  **Create pipelines for Employee, Title, Agency, and year 2021 Payroll transaction data to Synapse Analytics containting the data flows.**
    * Select the dirstaging folder in the data lake storage for staging
    * Optionally you can also create one master pipeline to invoke all the Data Flows
    * Validate and publish the pipelines

6. **Trigger and monitor the Pipelines**
    * Take a screenshot of each pipeline run after it has finished, or one after your master pipeline run has finished.
    
    In total, you should have 6 pipelines or on master pipeline
