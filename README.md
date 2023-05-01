## Data Integration Pipelines for NYC Payroll Data Analytics

### Project Overview
The City of New York would like to develope a Data Analytics platform on Azure Synapse Analytics to accomplich two primary objectives:
1. Analyze how th City's financial resources are allocated and how much of the City's budget is being devoted to overtime.

2. Make the data available to the interested public to show how the City's budget is being spent on salary and overtime pay for all municipal employees.

In this project as a Data Engineer I created high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operations. My Team also includes the city's Quality Assurance experts who will test the pipelines to find any errors and improve overall data quality.

The sources data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Aure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies

![image](https://user-images.githubusercontent.com/58150666/235501206-a2840698-cdd1-4b1f-b28a-6f31d405f02a.png)

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
