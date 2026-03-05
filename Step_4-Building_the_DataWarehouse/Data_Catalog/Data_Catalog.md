# **Gold Layer Data Catalog**

### 1. **gold.dim_customers**
- **Description:** Stores customer information
- **Columns:**

| Column Name      | Data Type     | Description                                                                                   |
|------------------|---------------|-----------------------------------------------------------------------------------------------|
| customer_key     | INT           | Surrogate key uniquely identifying each customer record in the dimension table.               |
| customer_id      | INT           | Unique numerical identifier assigned to each customer.                                        |
| customer_number  | VARCHAR       | Alphanumeric identifier representing the customer, used for tracking and referencing.         |
| first_name       | VARCHAR       | The customer's first name.                                                                    |
| last_name        | VARCHAR       | The customer's last name or family name.                                                      |
| country          | VARCHAR       | The country of residence of the customer.                                                    |
| marital_status   | VARCHAR       | The marital status of the customer (ex: 'Married', 'Single').                                 |
| gender           | VARCHAR       | The gender of the customer (ex: 'Male', 'Female', 'n/a').                                     |
| birthdate        | DATE          | The date of birth of the customer, formatted as YYYY-MM-DD (ex: 1971-10-06).                  |
| create_date      | DATE          | The date and time when the customer record was created in the system                          |

---

### 2. **gold.dim_products**
- **Description:** Provides information about the products.
- **Columns:**

| Column Name         | Data Type     | Description                                                                                         |
|---------------------|---------------|-----------------------------------------------------------------------------------------------------|
| product_key         | INT           | Surrogate key uniquely identifying each product record in the product dimension table.              |
| product_id          | INT           | A unique identifier assigned to the product for internal tracking and referencing.                  |
| product_number      | VARCHAR       | A structured alphanumeric code representing the product, often used for categorization or inventory.|
| product_name        | VARCHAR       | Descriptive name of the product, including key details such as type, color, and size.               |
| category_id         | VARCHAR       | A unique identifier for the product's category.           |
| category            | VARCHAR       | The broader classification of the product (ex: Bikes, Components) to group related items.           |
| subcategory         | VARCHAR       | A more detailed classification of the product within the category, such as product type.            |
| maintenance_required| VARCHAR       | Indicates whether the product requires maintenance (ex: 'Yes', 'No').                               |
| cost                | INT           | The cost or base price of the product, measured in monetary units.                                  |
| product_line        | VARCHAR       | The specific product line or series to which the product belongs (e.g., Road, Mountain).            |
| start_date          | DATE          | The date when the product became available for sale or use                                          |

---

### 3. **gold.fact_sales**
- **Description:** Stores transactional sales data for analytical purposes.
- **Columns:**

| Column Name     | Data Type     | Description                                                                                   |
|-----------------|---------------|-----------------------------------------------------------------------------------------------|
| order_number    | VARCHAR       | A unique alphanumeric identifier for each sales order (ex: 'SO54496').                        |
| product_key     | INT           | Surrogate key linking the order to the product dimension table.                               |
| customer_key    | INT           | Surrogate key linking the order to the customer dimension table.                              |
| order_date      | DATE          | The date when the order was placed.                                                           |
| shipping_date   | DATE          | The date when the order was shipped to the customer.                                          |
| due_date        | DATE          | The date when the order payment was due.                                                      |
| sales_amount    | INT           | The total monetary value of the sale for the line item, in whole currency units (ex: 25).     |
| quantity        | INT           | The number of units of the product ordered for the line item (ex: 1).                         |
| price           | INT           | The price per unit of the product for the line item, in whole currency units (ex: 25).        |