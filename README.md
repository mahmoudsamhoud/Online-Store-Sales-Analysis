# Creating an Online-Store Database and Perform Sales Analysis
![online](https://github.com/mahmoudsamhoud/Online-Store-Sales-Analysis/assets/78819528/3026ce66-9474-4ca7-89d7-c4b0b96d7957)

## Introduction
    Imagine you're the owner of an online store that sells various products to customers.
    As the owner of the store, you want to keep track of the sales made by your customers and gain insights
    that will help you make better business decisions.
    In order to achieve that, you want to create a database that stores data about your products, sales, and customers
    to be able to perform different analyses using this data to make data-driven decisions that will help you grow your business.
    In this project, we will achieve that using Python as our programming language and MySQL as our database management system (DBMS).

## Aim of The Project
    In this project, a database will be created and 3 tables will be added to our database
    in order to store data about products, sales, and customers respectively.
    Also, two sales analyses will be performed just to show how to extract data from the database and use it in analysis.

## Creating The Database, Adding Tables and Sample Data.
    Firstly, the following queries were specified:
        1. create_db_query: to create the database.
        2. create_products_table_query: to create the products table.
        3. create_sales_table_query: to create the sales table.
        4. create_customers_table_query: to create the customers' table.
        5. insert_products_data: to insert the products data
        6. insert_sales_data: to insert the sales data
        7. insert_customers_data: to insert the customers' data

These queries are provided in detail in the [Code.ipynb](https://github.com/mahmoudsamhoud/Online-Store-Sales-Analysis/blob/main/Code.ipynb) file.

The database is created using the following code.
```sql
        try:
    with connect(
        host="localhost",
        user = input ("Username:"),
         password = getpass ("Password:"),
    ) as connection:
        create_db_query = "CREATE DATABASE IF NOT EXISTS online_store_sales"
        with connection.cursor() as cursor:
            cursor.execute(create_db_query)
            connection.commit()
except Error as err:
    print(err)
```
Then we connect to the database and add tables and sample data using the following code
```sql
try:
    with connect(
        host="localhost",
        user = input ("Username:"),
         password = getpass ("Password:"),
        database = "online_store_sales"
    ) as connection:
        with connection.cursor() as cursor:
            print (connection)
        
        # Addint tables to the database
        with connection.cursor() as cursor:
            cursor.execute(create_products_table_query)
            cursor.execute(create_customers_table_query)
            cursor.execute(create_sales_table_query)
            connection.commit()   
        
        # Adding data to products table
        with connection.cursor() as cursor:
            cursor.executemany(insert_products_data, products)
            connection.commit()
        
        # Adding data to customers table
        with connection.cursor() as cursor:
            cursor.executemany(insert_customers_data, customers)
            connection.commit()
        
        # Adding data to sales table
        with connection.cursor() as cursor:
            for i in range(1000):
                #pick a random day between 1-1-2023 and 1-10-2023
                sale_date = start_date + datetime.timedelta(days=random.randint(0, 303))
                # pick a random customer_id 
                customer_id = random.randint(1, len(customers))
                # pick a random product_id
                product_id = random.randint(1, len(products))
                # pick a random quantity
                quantity = random.randint(1, 10)
                # pick a random unit_price
                unit_price = products[product_id-1][1]
                total_price = quantity * unit_price
                cursor.execute(insert_sales_data,(sale_date, customer_id, product_id, quantity, unit_price, total_price))
            connection.commit()
        
            print ("Database created successfully!")
except Error as err:
    print(err)
```


## About Data
    The data used in this project is randomly generated.
    We have three tables of data
    1. Products
   
| Column                  | Description                                | Data Type      |
| :---------------------- | :----------------------------------------- | :------------- |
| product_id              | Product id number and its a unique value   | INT            |
| product_name            | name of the product                        | VARCHAR(50)    |
| unit_cost               | price of the product                       | DECIMAL(10, 2) |

    1. Customers

| Column                  | Description                                | Data Type      |
| :---------------------- | :----------------------------------------- | :------------- |
| customer_id             | Customer id number and its a unique value  | INT            |
| first_name              | first name of a customer                   | VARCHAR(15)    |
| last_name               | last name of a customer                    | VARCHAR(15)    |
| email                   | email of the customer                      | VARCHAR(50)    |
| phone_num               | phone number of the customer               | VARCHAR(15)    |

    3. Sales

| Column                  | Description                                | Data Type      |
| :---------------------- | :----------------------------------------- | :------------- |
| sale_id                 | sale id number and its a unique value      | INT            |
| sale_date               | date when the sale occured                 | DATE           |
| customer_id             | customer id imported as a foriegn key      | INT            |
| product_id              | product id imported as a foriegn key       | INT            |
| quantity                | quantity of the sold product               | INT            |
| unit_price              | price of one unit of a product             | DECIMAL(10,2)  |
| total_price             | the total price paid by the customer       | Decimal(10,2)  |

    Sales table has two foriegn key, cutomer_id and product_id which are the primary keys of the customers table and products table respectively.

## Performing Sales Analysis.
    Two types of sales analysis were performed in order to show how to extract data from the database and use it in analysis.
1. Total sales by Month.
![sales_by_month](https://github.com/mahmoudsamhoud/Online-Store-Sales-Analysis/assets/78819528/742f18b7-069b-4fa5-a2a1-81faac2c682f)

2. Total sales by product.
![sales_by_product](https://github.com/mahmoudsamhoud/Online-Store-Sales-Analysis/assets/78819528/a29d28b1-853a-41a9-9ee8-245474745775)
   
PS: the data used in this project is randomly generated. So, every time we run the code we will get different results.
