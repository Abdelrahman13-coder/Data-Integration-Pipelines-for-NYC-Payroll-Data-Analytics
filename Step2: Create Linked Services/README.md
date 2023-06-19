## **Step 2:** Create Linked Services

1. **Create a Linked Service for Azure Data Lake
In Azure Data Factory, create a linked service to the data lake that contains the data files**
    * From the data stores, select Azure Data Lake Gen2
    * Test the connection
    
2. **Create a Linked Service to SQL Database that has the current (2021) data**
    * If you get a connection error, remember to add the IP address to the firewall settings in SQL DB in the Azure Portal
    
3. **Create a Linked Service for Synapse Analytics**
    * Create the linked service to the SQL pool.
