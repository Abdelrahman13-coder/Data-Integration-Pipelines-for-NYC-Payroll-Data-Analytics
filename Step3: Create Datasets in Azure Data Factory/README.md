## **Step 3:** Create Datasets in Azure Data Factory 

1. **Create the datasets for the 2021 Payroll file on Azure Data Lake Gen2**
    * Select DelimitedText
    * Set the path to the nucpayroll_2021.csv in the Data Lake
    * Preview the data to make sure it si correctly parsed
    
2. **Repeat the same process to create datasets for the reest of the data files in the Data Lake**
    * EmpMaster.csv
    * TitleMaster.csv
    * AgencyMaster.csv
    * Remember to publish all the datasets
    
3. **Create the dataset for transaction data table that should contain current (2021) data in SQL DB**

4. **Create the datasets for destination (target) tables in Synapse Analytics**
    * dataset for NYC_Payroll_EMP_MD
    * for NYC_Payroll_TITLE_MD
    * for NYC_Payroll_AGENCY_MD
    * for NYC_Payroll_Data
