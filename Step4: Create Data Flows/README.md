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

4. **Create data flows to laod the data from the data lake files into the Synapse Analytics data tables**
    * Create the data flows for laoding Employee, Title, and Agency files into corresponding SQL pool tables on Synapse Analytics
    * For each Employee, Title, and Agency file data flow, sink the data into each target Synapse table

5. **Create a data flow to load 2021 data from SQL DB to Syanpse Analytics**

6. **Create pipelines for Employee, Title, Agency, and year 2021 Payroll transaction data to Synapse Analytics containting the data flows.**
   
    * Select the dirstaging folder in the data lake storage for staging
    * Optionally you can also create one master pipeline to invoke all the Data Flows
    * Validate and publish the pipelines

7. **Trigger and monitor the Pipelines**
    * Take a screenshot of each pipeline run after it has finished, or one after your master pipeline run has finished.
    
    In total, you should have 6 pipelines or on master pipeline

[Please create a master key in the database or open the master key in the session before performing this operation.](https://knowledge.udacity.com/questions/903084#:~:text=Got%20it%20resolved%20by,DECRYPTION%20BY%20PASSWORD%20%3D%20%27%3Cpassword%3E%27)

