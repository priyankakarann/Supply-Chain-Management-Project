# Supply Chain Management for Cars in Power BI: Dashboard Project
![Supply chain](https://github.com/user-attachments/assets/5a6c7f74-d026-4472-a9b4-e4b161f3267f)


Supply Chain Management for Cars in Power BI project includes working with **Power BI, SQL, and Python**. This project is ideal for beginners who want to learn and understand an end-to-end **Power BI dashboard** project.


---

## Table of Contents
- [Overview](#overview)
- [Flow of the Project](#flow-of-the-project)
- [Project Architecture](#project-architecture)
- [Data Gathering](#data-gathering)
- [Data Cleaning](#data-cleaning)
- [Modeling in Power BI](#modeling-in-power-bi)
- [Dashboard Creation in Power BI](#dashboard-creation-in-power-bi)

---

## Overview
Supply Chain Management for Cars in Power BI provides a comprehensive overview of the **entire supply chain process** for automotive companies. This includes tracking the flow of materials, components, and finished products from **suppliers to manufacturers, distributors, and customers**.

Using Power BI, companies can **visualize key supply chain metrics** such as:
- Inventory levels
- Production schedules
- Delivery performance
- Supplier quality

Additionally, historical data analysis helps optimize operations and improve efficiency while reducing costs. This project touches on fundamental topics of **data analysis using Power BI, SQL, and Python**.


---

## Flow of the Project
1. **Project Architecture**
2. **Project BRD or FRD Documents**
3. **Data Gathering**
4. **Data Cleaning / Data Transformation**
5. **Data Modeling**
6. **Mockup Preparation**
7. **DAX Functions (DAX Calculations)**
8. **Create Visuals (For Dashboard)**
9. **Add Navigation**

---

## Project Architecture

![image](https://github.com/user-attachments/assets/1e2a14ce-cd97-44ff-b8e8-86e872316396)


---

## Data Gathering
According to the **BRD document**, we connect the **MySQL database** with **Kaggle APIs** to bring data automatically into the database using Python. From there, we load the data into **Power BI Desktop** using the **Get Data** option.

### Steps:
1. **Download dataset from Kaggle** using the API
2. **Read dataset using Pandas**
3. **Store dataset into MySQL**
4. **Import MySQL data into Power BI**

### Install Required Libraries
```python
!pip install pandas
!pip install zipfile
!pip install kaggle
!pip install numpy
!pip install pymysql
```

### Python Code for Data Extraction and Storage
```python
import pandas as pd
import zipfile
import kaggle

# Download dataset from Kaggle
!kaggle datasets download -d prashantk93/supply-chain-management-for-car

# Extract file
zipfile_name = 'supply-chain-management-for-car.zip'
with zipfile.ZipFile(zipfile_name, 'r') as file:
    file.extractall()

# Read dataset
cars = pd.read_csv("Car_SupplyChainManagementDataSet.csv")
cars.to_excel('supply-chain-final.xlsx', sheet_name='Data')
```

### Store Data in MySQL
```python
import pymysql

myconnection = pymysql.connect(host='host_name', user='user_name', passwd='your_password', database='your_db_name')
cur = myconnection.cursor()
df = pd.read_excel('supply-chain-final.xlsx')

# Remove unwanted column
df = df.iloc[:, 1:]

# Create table in MySQL
cur.execute("CREATE TABLE cars (SupplierID TEXT, SupplierAddress TEXT, SupplierName TEXT, SupplierContactDetails TEXT, ProductID TEXT, CarMaker TEXT, CarModel TEXT, CarColor TEXT, CarModelYear TEXT, CarPrice TEXT, CustomerID TEXT, CustomerName TEXT, Gender TEXT, JobTitle TEXT, PhoneNumber TEXT, EmailAddress TEXT, City TEXT, Country TEXT, CountryCode TEXT, State TEXT, CustomerAddress TEXT, OrderDate TEXT, OrderID TEXT, ShipDate TEXT, ShipMode TEXT, Shipping TEXT, PostalCode TEXT, Sales TEXT, Quantity TEXT, Discount TEXT, CreditCardType TEXT, CreditCard TEXT, CustomerFeedback TEXT)")

# Insert data into table
sql = "INSERT INTO cars VALUES ({})".format(','.join(['%s'] * len(df.columns)))
for i in range(len(df)):
    cur.execute(sql, tuple(df.iloc[i]))
    myconnection.commit()
```

📌 **Now, import the MySQL data into Power BI using ‘Get Data’ option.**

---

## Data Cleaning
Using **Power Query Editor**, we remove unnecessary attributes and keep only important columns. We also create a **Calendar Table (DateMaster)** for time intelligence functions.

![image](https://github.com/user-attachments/assets/1e595d83-05a7-4fa7-a981-ff85c693799b)

```DAX
DateMaster = CALENDAR(MIN(cars_data[OrderDate]), MAX(cars_data[OrderDate]))
OrderMonth Num = MONTH(DateMaster[OrderDate])
OrderMonth Name = FORMAT(DateMaster[OrderDate], "MMM")
OrderQuarter Num = QUARTER(DateMaster[OrderDate])



```

---

## Modeling in Power BI
After creating the **DateMaster Table**, we establish **relationships** between tables using **one-to-many relationships** in **Model View**.
![image](https://github.com/user-attachments/assets/f26cdb27-46f3-4b61-978a-14ee6038893b)


📌 **Creating a Total Sales Measure**
```DAX
TotalSales = SUM(cars_data[Sales])
```



---

## Dashboard Creation in Power BI
In this project, we create **four pages** with navigation between them:
1. **Home Page** - Navigation images and company logo
2. **Order Page** - Car orders, top 10-15 orders, availability of car models, and map chart
3. **Sales Page** - Total sales, discounts, sales by month, shipping, ship mode, and quarter
4. **Customer View Page** - Customer insights, sentiment analysis, and decomposition tree

### **Home Page**
![image](https://github.com/user-attachments/assets/38acff20-289f-4ca2-9633-653acd006d65)


### **Order Page**
![image](https://github.com/user-attachments/assets/65503c2d-7a1e-4ede-a457-671db6d66c91)


### **Sales Page**
![image](https://github.com/user-attachments/assets/e23cc877-4e45-4d02-b78f-bda55a067556)


### **Customer View Page**
![image](https://github.com/user-attachments/assets/ef98a892-fe32-48aa-8d8f-c1d240e61c3b)


📌 **Include navigation images on every page for better UX.**

---

## Conclusion
This **Supply Chain Management Dashboard** provides valuable insights into automotive **supply chain operations**, helping companies optimize their processes and enhance collaboration with suppliers and partners. By utilizing **Power BI, SQL, and Python**, this project showcases end-to-end data analytics capabilities for effective supply chain management.



