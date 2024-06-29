# Pizza Sales Analysis SQL Project 
Softwares and Tools Used: Excel, MySQL

## Table of Contents
- [Project Overview](#project-overview)
- [CSV Files](#csv-files)
- [Database Schema](#database-schema)
- [Questions I Wanted To Answer From the Dataset](#questions-i-wanted-to-answer-from-the-dataset)

## Project Overview
In this project, we explore a dataset of pizza sales to derive meaningful insights. The analysis ranges from basic data exploration to advanced performance metrics, allowing us to understand sales trends, customer preferences, and more.

* **How I Create Problems For Myself...:**
I found these datasets on Kaggle and I analyzed these datasets as if I were the owner of a bustling pizza outlet. As an owner, I wanted to gather useful insights into pizza orders and customer preferences through datasets. 
So I began asking questions to get to know my pizza outlet.

* **... And How I Plan On Solving those Problems:**
To help extract meaningful insights from their extensive pizza orders dataset, I will utilize SQL for data extraction and analysis. By performing a series of SQL queries, I will uncover key metrics and trends. This structured analysis will provide actionable insights to refine their menu, optimize inventory, and enhance overall customer satisfaction.
By leveraging these data-driven insights, not only do I become the owner of the best pizza outlet in town but I can make informed decisions to drive business growth and improve the customer experience.

## CSV Files
The dataset used in this project is available in the following CSV files located in the `data` folder:

- [pizzas.csv](https://github.com/user-attachments/files/16040831/pizzas.csv): Contains information about the pizzas.
- [pizza_types.csv](https://github.com/user-attachments/files/16040832/pizza_types.csv): Contains information about the different pizza types.
- [orders.csv](https://github.com/user-attachments/files/16040833/orders.csv): Contains information about the orders.
- [order_details.csv](https://github.com/user-attachments/files/16040837/order_details.csv): Contains details about the orders.

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
     

## Questions I Wanted To Answer From the Dataset:

### General Insights

1. **How many pizza types are there in total?**
   ```sql
   SELECT COUNT(pizza_type_id) AS total_pizza_types FROM pizza_types;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/bbda71e7-8760-4505-b398-936f0b9399b1)

Knowing the total number of pizza types available helps in understanding the variety offered to customers. We have 32 different pizza types, which indicates a diverse menu catering to various tastes and preferences. This variety can attract a wider customer base and meet different customer needs, enhancing overall customer satisfaction.

2. **Retrieve the total number of orders placed.**
   ```sql
   SELECT COUNT(order_id) AS total_orders
   FROM orders;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/03bae979-121a-4e08-bbdd-f6106fb1b34f)
   
The total number of orders placed provides an overview of customer activity and business performance. Here the total orders are 21,350, which indicates steady demand and customer engagement.

3. **Identify the most common pizza size ordered.**
   ```sql
   SELECT pizzas.size, COUNT(order_details.order_details_id) AS order_count
   FROM pizzas
   INNER JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
   GROUP BY pizzas.size
   ORDER BY order_count DESC
   LIMIT 1;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/25c1d33c-fdb5-4305-8963-3432bccbb631)
   
Knowing the most popular pizza size helps in managing inventory and pricing strategies. The L i.e. LARGE size is the most popular, therefore ensuring a sufficient supply of its pizza ingredients is crucial.

### Order Analysis

2. **Determine the distribution of orders by hour of the day.**
   ```sql
   SELECT HOUR(order_time) AS hour, COUNT(order_id) AS frequency_of_orders
   FROM orders
   GROUP BY hour
   ORDER BY frequency_of_orders DESC;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/eeee8a3f-46ba-4ebf-83cc-860f3709bb88)

Analyzing the frequency of orders by hour reveals peak business hours. This can help optimize staffing and operational efficiency. Most orders are placed between 12 PM and 1 PM and then 4 PM to 6 PM, this time period is critical for maximizing service speed and customer satisfaction.

3. **Group the orders by date and calculate the average number of pizzas ordered per day.**
   ```sql
   SELECT AVG(quantity)
   FROM (SELECT orders.order_date AS date, SUM(order_details.quantity) AS quantity
   FROM orders
   JOIN order_details ON orders.order_id = order_details.order_id
   GROUP BY date) AS daily_quantities;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/0996424d-585c-449e-ab82-495de7ae5cdd)

### Revenue Analysis
1. **Calculate the total revenue generated from pizza sales.**

    ```sql
    SELECT ROUND(SUM(order_details.quantity * pizzas.price),2) AS total_revenue
    FROM order_details
    JOIN pizzas
    ON order_details.pizza_id = pizzas.pizza_id;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/69a062e9-8b64-4e40-90be-015c37bc09c3)

We can calculate the total revenue rounded to 2 decimal places by simply using the above query and adding ROUND to it.
    
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/ef63d096-645f-4a25-82b0-a2a8196259f4)

Calculating total revenue provides a measure of the business's financial performance. The total revenue is $817860.05, which indicates the overall sales success.

2. **Determine the top 3 most ordered pizza types based on revenue.**
   ```sql
   SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue
   FROM pizza_types
   JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.name
   ORDER BY revenue DESC
   LIMIT 3;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/ec06d756-8e81-4ea7-8e8c-08f68138b189)

Identifying the top 3 pizza types by revenue highlights the most profitable items. Focusing on these pizzas in marketing campaigns can maximize profitability.

4. **Calculate the percentage contribution of each pizza category to total revenue.**
   ```sql
   SELECT pizza_types.category, 
       SUM(order_details.quantity * pizzas.price) / (
           SELECT SUM(order_details.quantity * pizzas.price)
           FROM order_details 
           JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
       ) * 100 AS revenue_percentage
   FROM pizza_types
   JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.category
   ORDER BY revenue_percentage DESC;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/2f6ba56f-6148-4f16-92df-87d6928795f3)

Analyzing the revenue contribution by category helps understand which categories drive the most sales. Classic pizzas contribute 26.91% of total revenue.

### Customer Preferences
1. **Identify the highest-priced pizza.**
   ```sql
   SELECT pizza_types.name, pizzas.price, pizza_types.ingredients
   FROM pizza_types
   INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   ORDER BY pizzas.price DESC
   LIMIT 1;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/be016eab-75ae-4e35-8f71-3624aaee927b)

Knowing the highest-priced pizza helps in pricing strategy and understanding customer spending behaviour. If the most expensive pizza is a speciality pizza with premium ingredients, highlighting its unique features can justify its price.
At this pizza outlet, we have the highest-priced pizza as The Greek Pizza which has Kalamata Olives, Feta Cheese, Tomatoes, Garlic, Beef Chuck Roast, and Red Onions as its ingredients making it pricey. 

2. **List the top 5 most ordered pizza types along with their quantities.**
   ```sql
   SELECT pizza_types.name, SUM(order_details.quantity) AS total_quantity_ordered
   FROM pizza_types
   INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   INNER JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.name
   ORDER BY total_quantity_ordered DESC
   LIMIT 5;
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/cd50b945-380a-494d-bd0f-ea91e0b33ed6)
   
Identifying the top 5 pizza types in terms of quantity ordered highlights customer favourites. This information is useful for menu optimization and targeted marketing campaigns.

3. **Find the total quantity of each pizza category ordered.**
   ```sql
   SELECT pizza_types.category, SUM(order_details.quantity)
   FROM pizza_types
   INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
   GROUP BY pizza_types.category;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/ac0a4031-e112-4588-ae7a-90ecfba5441d)
   
Analyzing the total quantity ordered by pizza category helps understand customer preferences for different pizza styles. Classic pizzas are highly popular, so expanding this category could attract more customers.

4. **Analyze customer preferences by pizza category. Identify the least and most popular pizza categories.**
   ```sql
   SELECT pizza_types.category, SUM(order_details.quantity)
   FROM pizza_types
   INNER JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
   INNER JOIN order_details ON pizzas.pizza_id = pizzas.pizza_id
   GROUP BY pizza_types.category
   ORDER BY SUM(order_details.quantity);

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/4eee21e7-bfd8-4d71-961e-005a7fcdda3a)

Identifying the least and most popular pizza categories provides insights into customer preferences and areas for menu improvement. Veggie pizzas are the least popular, SO WWE MIGHT HAVE TO reconsider their recipes or pricing might be necessary.

5. **Identify repeat customers based on order frequency.**
   ```sql
   SELECT order_id, COUNT(order_id)
   FROM order_details
   GROUP BY order_id
   HAVING COUNT(order_id) > 1
   ORDER BY COUNT(order_id) DESC;


![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/dc127289-f65b-44f9-879d-f6a63a52710f)

*The table is quite long for this one, so I could not upload it entirely.*


Identifying repeat customers helps in understanding customer loyalty and designing loyalty programs.

### Seasonal Analysis
1. **Analyze seasonal variations in pizza orders.**
   ```sql
   SELECT MONTH(order_date) AS month, COUNT(order_id) AS order_count
   FROM orders
   GROUP BY month
   ORDER BY order_count DESC;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/efdb733a-2121-49ef-be10-df0442dede7f)


Understanding seasonal trends in orders can guide promotional strategies and menu adjustments. Here we see there is a spike in orders during July, therefore, launching seasonal specials during this period could boost sales further.

2. **Identify the busiest day for orders.**
   ```sql
   SELECT orders.order_date, SUM(order_details.quantity) AS total_quantity
   FROM orders
   JOIN order_details ON orders.order_id = order_details.order_id
   GROUP BY orders.order_date
   ORDER BY total_quantity DESC
   LIMIT 1;
   
![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/745c4cc1-f967-4608-95b2-d08b3ec83dc3)

3. **Analyze the cumulative revenue generated over time.**

   ```sql
   SELECT order_date, 
       SUM(revenue) OVER (ORDER BY order_date) AS cumulative_revenue
   FROM (
    SELECT orders.order_date, 
           SUM(order_details.quantity * pizzas.price) AS revenue
    FROM order_details
    JOIN pizzas ON order_details.pizza_id = pizzas.pizza_id
    JOIN orders ON order_details.order_id = orders.order_id
    GROUP BY orders.order_date
   ) AS daily_revenues;

![image](https://github.com/deeksha-dhawan/Pizza-Outlet-Analysis-using-SQL/assets/174144967/6a6efe0a-f81e-42fd-bdfd-2af582bd8ab8)

*The table is quite long for this one, so I could not upload it entirely.*


Analyzing cumulative revenue over time helps track business growth and identify trends. The revenue shows a steady increase month over month, it indicates positive business momentum.

## Contact
For any questions or suggestions, feel free to reach out to me at [dk.dhawan555@gmail.com].
   
