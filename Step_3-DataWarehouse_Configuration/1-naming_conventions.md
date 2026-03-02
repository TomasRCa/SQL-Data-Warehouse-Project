# 💻**Naming conventions**
#### Before building the Data WareHouse, it's important to set naming conventions to organize and prevent future confusion, since many different people might use this Warehouse, and each of them might have it's own way of naming things. By setting naming convention rules, we're standardizing these to make it easier for everyone involved to work together and stay organized.

## **Table of Contents**

1. [General Rules](#general-rules)
2. [Table Naming Conventions](#table-naming-conventions)
    - [Bronze and Silver Layers](#bronze-and-silver-layers)
    - [Gold Layer](#gold-layer)
3. [Column Naming Conventions](#column-naming-conventions)
   - [Surrogate Keys](#surrogate-keys)
   - [Technical Columns](#technical-columns)
4. [Stored Procedures](#stored-procedures)
---

## **General rules**
- **Naming Convention**: **snake_case**, it's an easily readable format consisting in all lowercase letter and underscores(_) separating each word.
- **Avoiding SQL Reserved Words**: words such as **TABLE** or **CREATE** might cause confusion if used as names in the schemas, tables, etc... So all the names created for these must avoid SQL Reserved words.

## **Table naming conventions**
### Bronze and Silver Layers
On these layers, I won't be renaming nor changing their data model, therefore, they will have the same structure and conventions.
- All names must start with the source system name, and table names must match their original names verbatim.
-  **`<sourcesystem>_<entity>`** 
  - `<sourcesystem>`: Name of the source system (e.g., `crm`, `erp`).  
  - `<entity>`: Exact table name from the source system.  
   - Example: `crm_customer_info` → Customer information from the CRM system.
### Gold Layer
In this layer, there will be integration of multiple sources together, so a new data model will be created, as well as renaming it's content.
- All names must use meaningful, business-aligned names for tables, starting with the category prefix.
- **`<category>_<entity>`**  
  - `<category>`: Describes the role of the table, such as `dim` (dimension table) or `fact` (fact table).  
  - `<entity>`: Descriptive name of the table, aligned with the business domain (e.g., `customers`, `products`, `sales`).  
  - Examples:
    - `dim_customers` → Dimension table for customer data.  
    - `fact_sales` → Fact table containing sales transactions.

### Glossary of Category Patterns

| Pattern     | Meaning                           | Example(s)                              |
|-------------|-----------------------------------|-----------------------------------------|
| `dim_`      | Dimension table                  | `dim_customer`, `dim_product`           |
| `fact_`     | Fact table                       | `fact_sales`                            |
| `report_`   | Report table                     | `report_customers`, `report_sales_monthly`   |

## **Column Naming Conventions**

### Surrogate Keys
System-generated IDs to replace natural keys, reducing dependencies on source system changes and improving performance in relational joins.
- All primary keys in dimension tables must use the suffix `_key`.
- **`<table_name>_key`**  
  - `<table_name>`: Refers to the name of the table or entity the key belongs to.  
  - `_key`: A suffix indicating that this column is a surrogate key.  
  - Example: `customer_key` → Surrogate key in the `dim_customers` table.
  
### Technical Columns
Metadata colums added by the people that create the DataWarehouse, usually aimed for auditability and performance tuning.
- All technical columns must start with the prefix `dwh_`, followed by a descriptive name indicating the column's purpose.
- **`dwh_<column_name>`**  
  - `dwh`: Prefix exclusively for system-generated metadata.  
  - `<column_name>`: Descriptive name indicating the column's purpose.  
  - Example: `dwh_load_date` → System-generated column used to store the date when the record was loaded.
 
## **Stored Procedures**
Storing the procedures done to the database.
- All stored procedures used for loading data must follow the naming pattern:
- **`load_<layer>`**.
  
  - `<layer>`: Represents the layer being loaded, such as `bronze`, `silver`, or `gold`.
  - Example: 
    - `load_bronze` → Stored procedure for loading data into the Bronze layer.
    - `load_silver` → Stored procedure for loading data into the Silver layer.