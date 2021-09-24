# Load-data-from-on-premise-to-azure-dwh

## Create SQL Server VM
- Create SQL Server VM
  - Image: Free SQL Server License, SQL Server 2017 Developer on Windows Server 2017
  - Size: DS1_v2
  - Allow RDP access and SQL Server Ports
  - SQL Server Settings-> Enable SQL Authentication
- Connect to VM using RDP
- Open SSMS and Create new DB named - SourceDB
- Let's explore our data
  - Customer.csv
  - Product_Category_Subcategory.csv
  - Transactions.csv
  - Dim+Create+&+DimDate+Load.sql
- Import Data
  - Copy CSV files to VM
  - Import data (Customer and Transactions) in database using Task->Import Data, SQL Server Native Client

## Create Azure SQL Server Database
- PAAS
- Chose Standard
- Connect to it using SQL Query editor
- Connect using SSMS


## Create Datalake
- Create ADL
- Create container named - staging


## Create Datafactory
- Create SSIR
- Create Linked Service (SQL Server) to connect to SQL Server
- Create Dataset to connect to Customer table
- Create Dataset to connect to Transactions table
- Create Linked Service to connect to Azure Datakake
- Create Dataset to connect to Azure Datakake container
- Create Pipeline
  - Copy activity to copy Customer table
  - In same pipeline connect another Copy activity to copy Transactions table
- Debug Pipeline
- Check data in Data lake
- Create another Linked service on SSIR for File System
- Create another activity to copy flat file - Product_Category_Subcategory.csv to Data lake
  - Create another Copy data activity after 2nd
  - Source - dataset -> File System
  - Destination - Data lake


## Do data flow to clean data and load to Azure SQL Database
