# ✍ Data WareHouse Design:
#### To be able to organize the data and meet the requirements of the project, I'm using the Medallion Architecture model, which consists in separating the data into 3 layers. The **Bronze** layer, where data is extracted and kept unchanged in it's raw state, the **Silver** layer, where data is transformed into a cleaner and more concise version, and the **Gold** layer, where data is polished to be ready to be analyzed, as well as building Machine Learning Models on and comply with laws and business rules. Works similarly to a Data Mart.
#### Here's a visual breakdown:
![WareHouse Design](Step_2-Designing_Data_Architecture/WareHouse%20Design.jpg)

# 🏡 Data WareHouse Architecture:
#### After defining our Data Warehouse design, it's time to set it's architecture. The data will be extracted from a source (csv files in this case) into the Data Warehouse and processed throughout the layers previously mentioned until it reaches the stakeholders. For this project, I'll be using PostgreSQL. 
#### Here's a visual breakdown:
![WareHouse Architecture](WareHouse%20Architecture.jpg)
