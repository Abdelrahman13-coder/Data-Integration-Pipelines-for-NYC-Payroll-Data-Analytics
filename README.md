## Data Integration Pipelines for NYC Payroll Data Analytics

### Project Overview
The City of New York would like to develope a Data Analytics platform on Azure Synapse Analytics to accomplich two primary objectives:
1. Analyze how th City's financial resources are allocated and how much of the City's budget is being devoted to overtime.

2. Make the data available to the interested public to show how the City's budget is being spent on salary and overtime pay for all municipal employees.

In this project as a Data Engineer I created high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operations. My Team also includes the city's Quality Assurance experts who will test the pipelines to find any errors and improve overall data quality.

The sources data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Aure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies

![image](https://user-images.githubusercontent.com/58150666/235501206-a2840698-cdd1-4b1f-b28a-6f31d405f02a.png)



## **Step 1:** Prepare the Data Infrastructure
Setup Data and Resources in Azure

**1. Creaet the data lake and upload data**


2. **Create an Azure Data Factory Resource**
3. **Create a SQL Database to store the current year of the payroll data**

    * Add Client IP address to the SQL DB firewall

    * Create a table called **NYC_Payroll_Data** in **db_nycpayroll** in the Azure Query Editor with the SQL Script:

``` tsql
CREATE TABLE [dbo].[NYC_Payroll_Data](
    [FiscalYear] [int] NULL,
    [PayrollNumber] [int] NULL,
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL,
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL,
    [AgencyStartDate] [date] NULL,
    [WorkLocationBorough] [varchar](50) NULL,
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL,
    [LeaveStatusasofJune30] [varchar](50) NULL,
    [BaseSalary] [float] NULL,
    [PayBasis] [varchar](50) NULL,
    [RegularHours] [float] NULL,
    [RegularGrossPaid] [float] NULL,
    [OTHours] [float] NULL,
    [TotalOTPaid] [float] NULL,
    [TotalOtherPay] [float] NULL
) 
GO
```

5. **Create master data tables and payroll transaction tables**
* Define the file format, if not already, for reading/saving the data from/to a comma delimited file i blob storage

``` tsql
-- Use the same file format as used for creating the External Tables during the LOAD step.
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'SynapseDelimitedTextFormat') 
    CREATE EXTERNAL FILE FORMAT [SynapseDelimitedTextFormat] 
    WITH ( FORMAT_TYPE = DELIMITEDTEXT ,
           FORMAT_OPTIONS (
             FIELD_TERMINATOR = ',',
             USE_TYPE_DEFAULT = FALSE
            ))
GO
```

* Define the data source to persist the results.
``` tsql
-- Storage path where the result set will persist
IF NOT EXISTS (SELECT * FROM sys.external_data_sources WHERE name = 'mydlsfs20230413_mydls20230413_dfs_core_windows_net') 
    CREATE EXTERNAL DATA SOURCE [mydlsfs20230413_mydls20230413_dfs_core_windows_net] 
    WITH (
        LOCATION = 'abfss://mydlsfs20230413@mydls20230413.dfs.core.windows.net' 
    )
GO
```

**Use the blob storage account name as applicable to you.** In the snippet above, `mydls20230413` is the Data Lake Gen 2 storage name, and `mydlsfs20230413` si the name fo the file system (container).

* Create Employee Master Data table:
``` tsql
CREATE EXTERNAL TABLE [dbo].[NYC_Payroll_EMP_MD](
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL
)
WITH (
		LOCATION = 'payroll_emp_md.csv',
      DATA_SOURCE = [mydlsfs20230413_mydls20230413_dfs_core_windows_net],
      FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO
```

**We are assuming the data will be stored in the `payroll_emp_md.csv` file in the blob storage. Also, change the `DATA_SOURCE` value, as applicable to you.**

* Create Job Title Table:
``` tsql
CREATE TABLE [dbo].[NYC_Payroll_TITLE_MD](
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL
)
WITH (
		LOCATION = 'payroll_title_md.csv',
      DATA_SOURCE = [mydlsfs20230413_mydls20230413_dfs_core_windows_net],
      FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO
```

* Create Agency Master table:
``` tsql
CREATE TABLE [dbo].[NYC_Payroll_AGENCY_MD](
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL
)
WITH (
		LOCATION = 'payroll_agency_md.csv',
      DATA_SOURCE = [mydlsfs20230413_mydls20230413_dfs_core_windows_net],
      FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO
```

* Create Payroll transaction data table:
``` tsql
CREATE TABLE [dbo].[NYC_Payroll_Data](
    [FiscalYear] [int] NULL,
    [PayrollNumber] [int] NULL,
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL,
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL,
    [AgencyStartDate] [date] NULL,
    [WorkLocationBorough] [varchar](50) NULL,
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL,
    [LeaveStatusasofJune30] [varchar](50) NULL,
    [BaseSalary] [float] NULL,
    [PayBasis] [varchar](50) NULL,
    [RegularHours] [float] NULL,
    [RegularGrossPaid] [float] NULL,
    [OTHours] [float] NULL,
    [TotalOTPaid] [float] NULL,
    [TotalOtherPay] [float] NULL
) 
WITH (
		LOCATION = 'payroll_data.csv',
      DATA_SOURCE = [mydlsfs20230413_mydls20230413_dfs_core_windows_net],
      FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO
```


## **Step 2:** Create Linked Services

1. **Create a Linked Service for Azure Data Lake
In Azure Data Factory, create a linked service to the data lake that contains the data files**
    * From the data stores, select Azure Data Lake Gen2
    * Test the connection
    
2. **Create a Linked Service to SQL Database that has the current (2021) data**
    * If you get a connection error, remember to add the IP address to the firewall settings in SQL DB in the Azure Portal
    
3. **Create a Linked Service for Synapse Analytics**
    * Create the linked service to the SQL pool.


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
