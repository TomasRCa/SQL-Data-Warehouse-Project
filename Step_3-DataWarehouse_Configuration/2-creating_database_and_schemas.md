# 🌐**Creating Database and Schemas**
#### Now that the architecture, design and naming conventions are set, it's time to build the foundation for our Data Warehouse. Starting by creating our database in PostgreSQL, and then the Schemas.
### Query to create the Database
```SQL
CREATE DATABASE "DataWarehouse";
```
### Creating the Schemas
```SQL
CREATE SCHEMA bronze;
CREATE SCHEMA silver;
CREATE SCHEMA gold;
```
This is how if looks like so far:


![Database and Schemas](https://raw.githubusercontent.com/TomasRCa/SQL-Data-Warehouse-Project/main/Step_3-DataWarehouse_Configuration/Database%20and%20Schemas.jpg)
