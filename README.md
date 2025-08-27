-- =============================================
-- PIZZA SALES ANALYSIS
-- Organized SQL Script
-- =============================================

-- 1. CHECK ORIGINAL DATA
SELECT * FROM pizza_sales;

-- 2. FIX DATA STRUCTURE
ALTER TABLE pizza_sales
ADD corrected_unit_price DECIMAL(10,2);

ALTER TABLE pizza_sales
ADD corrected_total_price DECIMAL(10,2);

UPDATE pizza_sales
SET 
    corrected_unit_price = CASE 
        WHEN LEN(CAST(CAST(unit_price AS BIGINT) AS VARCHAR)) = 2 THEN unit_price
        WHEN LEN(CAST(CAST(unit_price AS BIGINT) AS VARCHAR)) = 3 THEN unit_price / 10.0
        WHEN LEN(CAST(CAST(unit_price AS BIGINT) AS VARCHAR)) = 4 THEN unit_price / 100.0
        ELSE unit_price
    END,
    corrected_total_price = CASE 
        WHEN LEN(CAST(CAST(total_price AS BIGINT) AS VARCHAR)) = 2 THEN total_price
        WHEN LEN(CAST(CAST(total_price AS BIGINT) AS VARCHAR)) = 3 THEN total_price / 10.0
        WHEN LEN(CAST(CAST(total_price AS BIGINT) AS VARCHAR)) = 4 THEN total_price / 100.0
        ELSE total_price
    END;

-- 3. KEY METRICS (KPIs)
SELECT SUM(corrected_total_price) AS total_revenue FROM pizza_sales;

SELECT SUM(corrected_total_price)/COUNT(DISTINCT order_id) AS average_order_value FROM pizza_sales;
SELECT SUM(quantity) AS total_pizzas_sold FROM pizza_sales;
SELECT COUNT(DISTINCT order_id) AS total_orders FROM pizza_sales;

SELECT CAST(
    CAST(SUM(quantity) AS DECIMAL(10,2)) /
    CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) 
    AS DECIMAL(10,2)
) AS average_pizzas_per_order 
FROM pizza_sales;

-- 4. TIME TREND ANALYSIS
SELECT 
    DATENAME(WEEKDAY, order_date) AS order_day,
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(WEEKDAY, order_date), DATEPART(WEEKDAY, order_date)
ORDER BY DATEPART(WEEKDAY, order_date);

SELECT 
    DATENAME(HOUR, order_time) AS order_hour, 
    COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATENAME(HOUR, order_time)
ORDER BY DATENAME(HOUR, order_time);

-- 5. CATEGORY AND SIZE ANALYSIS
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

-- 6. PRODUCT RANKING
SELECT 
    pizza_name, 
    SUM(quantity) AS total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_pizzas_sold DESC;

SELECT 
   TOP 5 pizza_name, 
    SUM(quantity) AS total_pizzas_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_pizzas_sold DESC;

