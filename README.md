# 🍕 Pizza Sales Mini Case Study

## 📌 Solution

### 1. How much was the total revenue?
We want to know the total money earned from all pizza sales. To calculate this, we sum the corrected_total_price column, which contains the revenue from each order line.
````sql
SELECT SUM(corrected_total_price) AS total_revenue 
FROM pizza_sales;
````
**Answer:**

<img width="141" alt="image" src="https://github.com/user-attachments/assets/7a131112-3f5d-4761-a420-9dea3fd37283">

There was sold $881.654,30 from pizzas.
### 2. How much was the average order value?

````sql
SELECT SUM(corrected_total_price)/COUNT(DISTINCT order_id) AS average_order_value 
FROM pizza_sales;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/122c74c0-da5e-4e46-bfd5-3c7e70833e33)

The average order value from all the pizzas sold was $41,29.
### 3. How many pizzas were sold?

````sql
SELECT SUM(quantity) AS total_pizzas_sold 
FROM pizza_sales;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/443f2ba8-c1b1-4549-a6b3-4cbc989eb8c8)

The amount of pizzas sold is 49574.

### 4. How many orders were made?

````sql
SELECT COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/cee7a761-89ea-4a96-add5-fe7c6142655d)

The amount of orders made is 21350.

### 5. How much is the average pizzas per order?

````sql
SELECT CAST(
    CAST(SUM(quantity) AS DECIMAL(10,2)) /
    CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) 
    AS DECIMAL(10,2)
) AS average_pizzas_per_order 
FROM pizza_sales;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/5d08456d-13a9-4689-9f0e-ef499ec7c966)

The amount of pizzas per order is 2,32.

### 6. What are the days with the most orders?

````sql
SET LANGUAGE English;
SELECT 
    DATENAME(WEEKDAY, order_date) AS order_day,
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(WEEKDAY, order_date), DATEPART(WEEKDAY, order_date)
ORDER BY DATEPART(WEEKDAY, order_date);
````
**Answer:**

![Image](https://github.com/user-attachments/assets/be1e81e2-74b6-4b65-8bcb-d64da578a90e)

The days with most orders are Friday and Saturday.

### 7. Which hour of the day has the most amount of orders?

````sql
SELECT 
    DATENAME(HOUR, order_time) AS order_hour, 
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(HOUR, order_time)
ORDER BY DATENAME(HOUR, order_time);
````
**Answer:**

![Image](https://github.com/user-attachments/assets/37a0a939-9c71-4ca5-9cc5-2704a9de0f44)

The hour with most orders is 12pm.

### 8. Which hour of the day has the most amount of orders?

````sql
SELECT 
    DATENAME(HOUR, order_time) AS order_hour, 
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(HOUR, order_time)
ORDER BY DATENAME(HOUR, order_time);
````
**Answer:**

![Image](https://github.com/user-attachments/assets/37a0a939-9c71-4ca5-9cc5-2704a9de0f44)

The hour with most orders is 12pm.

### 9. How many orders each pizza category have?

````sql
SELECT 
    pizza_category, 
    SUM(corrected_total_price) AS total_sales, 
    CAST(
        SUM(corrected_total_price) * 100.0 /
        (SELECT SUM(corrected_total_price) FROM pizza_sales WHERE MONTH(order_date) = 1) 
        AS DECIMAL(10,2)
    ) AS percentage
FROM pizza_sales
WHERE MONTH(order_date) = 1
GROUP BY pizza_category
ORDER BY percentage DESC;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/968b4b68-988d-46f9-97b4-b8282004db43)

The pizza category with most orders is the classic.

### 10. Wich pizza size sells the most?

````sql
SELECT 
    pizza_size, 
    SUM(corrected_total_price) AS total_sales, 
    CAST(
        SUM(corrected_total_price) * 100.0 /
        (SELECT SUM(corrected_total_price) FROM pizza_sales) 
        AS DECIMAL(10,2)
    ) AS percentage
FROM pizza_sales
GROUP BY pizza_size
ORDER BY percentage DESC;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/544218bf-3e5d-4d10-b59a-46dfcd2c1be2)

The pizza size with most sells is the large.

### 11. Wich pizza size sells the most?

````sql
SELECT 
    pizza_size, 
    SUM(corrected_total_price) AS total_sales, 
    CAST(
        SUM(corrected_total_price) * 100.0 /
        (SELECT SUM(corrected_total_price) FROM pizza_sales) 
        AS DECIMAL(10,2)
    ) AS percentage
FROM pizza_sales
GROUP BY pizza_size
ORDER BY percentage DESC;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/544218bf-3e5d-4d10-b59a-46dfcd2c1be2)

The pizza size with most sells is the large.

### 12. Wich pizza sells the most in terms of quantity?

````sql
SELECT 
   TOP 5 pizza_name, 
    SUM(quantity) AS total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_pizzas_sold DESC;
````
**Answer:**

![Image](https://github.com/user-attachments/assets/f15583d1-0a28-4778-8949-a8daac571783)

The pizza with most quantity of orders is the Classic Deluxe.

### 13. Wich pizza sells the worst in terms of quantity?

````sql
SELECT 
    TOP 5 
    pizza_name, 
    SUM(quantity) AS total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_pizzas_sold ASC
````
**Answer:**

![Image](https://github.com/user-attachments/assets/1fa8e817-5443-43e2-b7e4-0bc9cc8c58bf)

The pizza with the less quantity of orders is the Brie Carre.




