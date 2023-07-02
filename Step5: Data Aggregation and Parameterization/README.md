## **Step 5:** Data Aggregation and Parameterization

In this step, you'll extract the 2021 year data and historical data, merge, aggregate and store it in Synpase Analytics. The aggregation will be on Agency Name, Fiscal Year and TotalPaid.

1. **Create a Summary table in Synapse with the following SQL script and create a dataset named table_synapse_nycpayroll_summary**

```
undefined CREATE TABLE [dbo].[NYC_Payroll_Summary]( [FiscalYear] [int] NULL,
[AgencyName] [varchar](50) NULL, [TotalPaid] [float] NULL )
```

2. **Create a new dataset for the Azure Data Lake Gen2 folder that contains the historical files**
   * Select dirhistoryfiles in the data lake as the source

3. **Create new data flow and name it Dataflow Aggregate Data**
   * Create a data flow level parameter for Fiscal Year
   * Add first Source for table_sqldb_nyc_payroll_data table
   * Add second Source for the Azure Data Lake history folder

4. **Create a new Union activity in the data flow and Union with history files**
5. **Add a Filter activity after Union**
   * In Expression Builder, enter
     ```
     toInteger(FiscalYear) >= $dataflow_param_fiscalyear
     ```
6. **Derive a new TotalPaid column**
   * In Expression Builder, enter ``` RegularGrossPaid + TotalOTPaid+TotalOtherPay ```
7. **Add an Aggregate activity to the data flow next to the TotalPaid activity**
   * Under Group By, Select AgencyName and Fiscal Year
8. **Add a Sink activity to the Data Flow**
   * Select the dataset to target (sink) the data into the Synpase Analytics Payroll Summary table.
   * In Settings, select Truncate Table
    
9. **Create a new Pipeline and add the Aggregate data flow**
    * Create a new Global Parameter (This will be the Parameter at the global level that will be passed on the data flow
    * In Parameters, select Pipeline Expression
    * Choose the parameter created at the Pipeline level
10. **Validate, Publish and Trigger the pipeline. Enter the desired value for te paramter**
11. **Monitor the Pipeline run and take a screenshot of the finished pipeline run.**
