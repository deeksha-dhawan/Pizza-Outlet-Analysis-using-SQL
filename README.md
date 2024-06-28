# Pizza Sales Analysis SQL Project 
Softwares and Tools Used: Excel, MySQL

## Table of Contents
- [Project Overview](#project-overview)
- [Database Schema](#database-schema)
- [CSV Files](#csv-files)
- [Questions I Wanted To Answer From the Dataset](#questions-i-wanted-to-answer-from-the-dataset)
- [How to Run the Queries](#how-to-run-the-queries)
- [Contact](#contact)
- [License](#license)

## Project Overview
In this project, we explore a dataset of pizza orders to derive meaningful insights. The analysis ranges from basic data exploration to advanced performance metrics, allowing us to understand sales trends, customer preferences, and more.

* **How I Create Problems For Myself...:**
I analyzed these datasets as if I were the owner of a bustling pizza outlet. As an owner, I wanted to gather useful insights into pizza orders and customer preferences through datasets. 
So I began asking questions to get to know my pizza outlet.

* **... And How I Plan On Solving those Problems:**
To help extract meaningful insights from their extensive pizza orders dataset, I will utilize SQL for data extraction and analysis. By performing a series of SQL queries, I will uncover key metrics and trends. This structured analysis will provide actionable insights to refine their menu, optimize inventory, and enhance overall customer satisfaction.
By leveraging these data-driven insights, not only do I become the owner of the best pizza outlet in town but I can make informed decisions to drive business growth and improve the customer experience.

## Database Schema
The database consists of the following tables:

1. **Pizza**
   - `pizza_id`: Unique identifier for each pizza
   - `type`: Type of pizza
   - `size`: Size of the pizza
   - `price`: Price of the pizza

2. **Pizza Type**
   - `pizza_type_id`: Unique identifier for each pizza type
   - `name`: Name of the pizza type
   - `category`: Category of the pizza (e.g., vegetarian, non-vegetarian)
   - `ingredients`: Ingredients of the pizza type

3. **Orders**
   - `order_id`: Unique identifier for each order
   - `order_date`: Date when the order was placed
   - `order_time`: Time when the order was placed

4. **Order Details**
   - `order_details_id`: Unique identifier for each order detail
   - `order_id`: Unique identifier for each order (foreign key)
   - `pizza_id`: Unique identifier for each pizza (foreign key)
   - `quantity`: Quantity of the pizza ordered
     
## CSV Files
The dataset used in this project is available in the following CSV files located in the `data` folder:

- `data/pizza.csv`: Contains information about the pizzas.
- `data/pizza_type.csv`: Contains information about the different pizza types.
- `data/orders.csv`: Contains information about the orders.
- `data/order_details.csv`: Contains details about the orders.
- 
## Questions I Wanted To Answer From the Dataset:

1. **How many pizzas are there in total?**
   ```sql
   select count(pizza_type_id)
   from pizza_types;
