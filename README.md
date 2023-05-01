## Data Integration Pipelines for NYC Payroll Data Analytics

### Project Overview
The City of New York would like to develope a Data Analytics platform on Azure Synapse Analytics to accomplich two primary objectives:
1. Analyze how th City's financial resources are allocated and how much of the City's budget is being devoted to overtime.

2. Make the data available to the interested public to show how the City's budget is being spent on salary and overtime pay for all municipal employees.

In this project as a Data Engineer I created high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operations. My Team also includes the city's Quality Assurance experts who will test the pipelines to find any errors and improve overall data quality.

The sources data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Aure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies

![image](https://user-images.githubusercontent.com/58150666/235501206-a2840698-cdd1-4b1f-b28a-6f31d405f02a.png)



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

** We are assuming the data will be stored in the `payroll_emp_md.csv` file in the blob storage. Also, change the `DATA_SOURCE` value, as applicable to you.**

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
