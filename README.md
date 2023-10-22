# Creating an Online-Store Database and Perform Sales Analysis
![online](https://github.com/mahmoudsamhoud/Online-Store-Sales-Analysis/assets/78819528/3026ce66-9474-4ca7-89d7-c4b0b96d7957)

## Introduction
    Imagine you're the owner of an online store that sells various products to customers.
    As the owner of the store, you want to keep track of the sales made by your customers and gain insights that will help you make better business decisions. In order to achieve that, you want to create a database that stores data about your products, sales, and customers to be able to perform different analyses using this data to make data-driven decisions that will help you grow your business.
    In this project, we will achieve that using Python as our programming language and MySQL as our database management system (DBMS).

## Aim of The Project
    In this project, a database will be created and 3 tables will be added to our database in order to store data about products, sales, and customers respectively. Also, two sales analyses will be performed just to show how to extract data from the database and use it in analysis.

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


