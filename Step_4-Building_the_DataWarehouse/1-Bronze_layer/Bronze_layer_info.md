# **🏗️Building the bronze layer**
### To build the bronze layer, 3 steps are required: 
- Analyzing the source system
- Ingesting the data and validating the schemas and completeness of the data

## **Analyzing the source system**
#### In this phase, it's important to contact the source system to better understand the data they are providing, and it will also help in understanding how to handle this data. For this, let's go over some important topics.
### Business context and ownership:
    - Who's responsible for the data?
    - What business process does it support?
        - Customer Transation?
        - Logistics?
        - Supply Chain?
        - Finance?
    - System and Data documentation
    - Data Model and Catalog
        - Are there descriptions about the tables, columns and schemas?

### Architecture and Technological Stack
    - How is data stored? Is it an on-premise model, cloud, or a hybrid?
    - What are the integration capabilities? (How can I get the data?)
        - This question is used to understand which tecnology will be used to extract the data

### Extract and Load
	This is the part with the most technical questions
    - Full Load vs Incremental Load?
    - Data Scope: How much historical data should we keep?
    - What's the size of the data in question?
    - Are there data volume limitations? Some older store systems might struggle with performance if the data extraction process is demanding more than what it can take. It's important to keep in mind the source system's limitations and avoid causing issues.
    - How to acess the data in the source system?
        - Tokens? SSH? Keys?

## **Ingesting the Data and validating the schemas and completeness of the data**
The CSV files already have the columns, and since it was set before that the names should stay unchanged for this layer, I'm using the following queries to define the structure of the DataWarehouse:
### Creating the tables
```SQL
CREATE TABLE bronze.crm_cust_info (
    cst_id              INT,
    cst_key             VARCHAR(50),
    cst_firstname       VARCHAR(50),
    cst_lastname        VARCHAR(50),
    cst_marital_status  VARCHAR(50),
    cst_gndr            VARCHAR(50),
    cst_create_date     DATE
);

CREATE TABLE bronze.crm_sales_details (
    sls_ord_num  VARCHAR(50),
    sls_prd_key  VARCHAR(50),
    sls_cust_id  INT,
    sls_order_dt INT,
    sls_ship_dt  INT,
    sls_due_dt   INT,
    sls_sales    INT,
    sls_quantity INT,
    sls_price    INT
);

CREATE TABLE bronze.crm_prd_info (
    prd_id       INT,
    prd_key      VARCHAR(50),
    prd_nm       VARCHAR(50),
    prd_cost     INT,
    prd_line     VARCHAR(50),
    prd_start_dt TIMESTAMP,
    prd_end_dt   TIMESTAMP
);

CREATE TABLE bronze.erp_loc_a101 (
    cid    VARCHAR(50),
    cntry  VARCHAR(50)
);

CREATE TABLE bronze.erp_cust_az12 (
    cid    VARCHAR(50),
    bdate  DATE,
    gen    VARCHAR(50)
);

CREATE TABLE bronze.erp_px_cat_g1v2 (
    id           VARCHAR(50),
    cat          VARCHAR(50),
    subcat       VARCHAR(50),
    maintenance  VARCHAR(50)
);
```
And it's currently looking like this:


![Bronze layer tables](https://raw.githubusercontent.com/TomasRCa/SQL-Data-Warehouse-Project/main/Step_4-Building_the_DataWarehouse/1-Bronze_layer/Bronze%20layer%20tables.jpg)

### Populating the tables
To populate the table, I'm uploading the file like this, while making sure the **HEADER** option is on, and repeating the same process for each table:

![CSV import](https://raw.githubusercontent.com/TomasRCa/SQL-Data-Warehouse-Project/main/Step_4-Building_the_DataWarehouse/1-Bronze_layer/CSV%20import.jpg)

And to confirm it was uploaded correctly:

![Insert sucess](https://raw.githubusercontent.com/TomasRCa/SQL-Data-Warehouse-Project/main/Step_4-Building_the_DataWarehouse/1-Bronze_layer/Insert%20sucess.jpg)

Alternatively, I could also use the following query on the PSQL:
```SQL
\COPY bronze.cust_info (
    cst_id,
    cst_key,
    cst_firstname,
    cst_lastname,
    cst_marital_status,
    cst_gndr,
    cst_create_date
)
FROM 'C:\Users\Tomás\Desktop\SQL DataWarehouse\CRM\cust_info.csv'
DELIMITER ','
CSV HEADER;
```
### Creating a stored procedure
Some tasks might be repeated several times in the future. For this, a stored procedure is the way to go since it stores the query used, which means that we can re-use the code quickly.
To create a stored procedure, firstly I'll put my csv files in a folder acessible to the server(meaning that the file must exist on the same machine that the PostgreSQL server is running), then run the following query:
```SQL
CREATE OR REPLACE PROCEDURE bronze.load_bronze()
LANGUAGE plpgsql
AS $$
DECLARE
    start_time TIMESTAMP;
    end_time TIMESTAMP;
    batch_start_time TIMESTAMP;
    batch_end_time TIMESTAMP;
BEGIN
    -- Batch start time
    batch_start_time := NOW();
    RAISE NOTICE 'Loading Bronze Layer';

    -- Loading CRM Tables
    RAISE NOTICE '------------------------------------------------';
    RAISE NOTICE 'Loading CRM Tables';
    RAISE NOTICE '------------------------------------------------';

    -- Load crm_cust_info
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.crm_cust_info';
    TRUNCATE TABLE bronze.crm_cust_info;
    RAISE NOTICE '>> Inserting Data Into: bronze.crm_cust_info';
    COPY bronze.crm_cust_info FROM 'C:\Program Files\PostgreSQL\18\data\imports\cust_info.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Load crm_prd_info
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.crm_prd_info';
    TRUNCATE TABLE bronze.crm_prd_info;
    RAISE NOTICE '>> Inserting Data Into: bronze.crm_prd_info';
    COPY bronze.crm_prd_info FROM 'C:\Program Files\PostgreSQL\18\data\imports\prd_info.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Load crm_sales_details
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.crm_sales_details';
    TRUNCATE TABLE bronze.crm_sales_details;
    RAISE NOTICE '>> Inserting Data Into: bronze.crm_sales_details';
    COPY bronze.crm_sales_details FROM 'C:\Program Files\PostgreSQL\18\data\imports\sales_details.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Loading ERP Tables
    RAISE NOTICE '------------------------------------------------';
    RAISE NOTICE 'Loading ERP Tables';
    RAISE NOTICE '------------------------------------------------';

    -- Load erp_loc_a101
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.erp_loc_a101';
    TRUNCATE TABLE bronze.erp_loc_a101;
    RAISE NOTICE '>> Inserting Data Into: bronze.erp_loc_a101';
    COPY bronze.erp_loc_a101 FROM 'C:\Program Files\PostgreSQL\18\data\imports\LOC_A101.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Load erp_cust_az12
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.erp_cust_az12';
    TRUNCATE TABLE bronze.erp_cust_az12;
    RAISE NOTICE '>> Inserting Data Into: bronze.erp_cust_az12';
    COPY bronze.erp_cust_az12 FROM 'C:\Program Files\PostgreSQL\18\data\imports\CUST_AZ12.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Load erp_px_cat_g1v2
    start_time := NOW();
    RAISE NOTICE '>> Truncating Table: bronze.erp_px_cat_g1v2';
    TRUNCATE TABLE bronze.erp_px_cat_g1v2;
    RAISE NOTICE '>> Inserting Data Into: bronze.erp_px_cat_g1v2';
    COPY bronze.erp_px_cat_g1v2 FROM 'C:\Program Files\PostgreSQL\18\data\imports\PX_CAT_G1V2.csv' DELIMITER ',' CSV HEADER;
    end_time := NOW();
    RAISE NOTICE '>> Load Duration: % seconds', EXTRACT(EPOCH FROM (end_time - start_time));
    RAISE NOTICE '>> -------------';

    -- Batch end time
    batch_end_time := NOW();
    RAISE NOTICE '==========================================';
    RAISE NOTICE 'Loading Bronze Layer Completed';
	RAISE NOTICE '>> Load Duration: % hours % minutes % seconds',
    EXTRACT(HOUR FROM (end_time - start_time)),
    EXTRACT(MINUTE FROM (end_time - start_time)),
    EXTRACT(SECOND FROM (end_time - start_time));
    RAISE NOTICE '==========================================';
EXCEPTION
    WHEN OTHERS THEN
        RAISE NOTICE '==========================================';
        RAISE NOTICE 'ERROR OCCURRED DURING LOADING BRONZE LAYER';
        RAISE NOTICE 'Error Message: %', SQLERRM;
        RAISE NOTICE '==========================================';
END;
$$;
```

To use the created procedure, we can simply run:
```SQL
CALL bronze.load_bronze();
```

Here's how it looks like inside the PGAdmin:
![Bronze procedure](https://raw.githubusercontent.com/TomasRCa/SQL-Data-Warehouse-Project/main/Step_4-Building_the_DataWarehouse/1-Bronze_layer/Bronze%20procedure.jpg)
