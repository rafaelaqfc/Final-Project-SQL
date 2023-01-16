Answer the following questions and provide the SQL queries used to find the answer.
    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

SELECT * 
    FROM all_sessions;

SELECT COUNT(country) AS country_count, 
    country,    
    MAX(total_transactions_revenue / 1000) 
FROM all_sessions
GROUP BY country
ORDER BY MAX(total_transactions_revenue) DESC
LIMIT 10;

SELECT COUNT(city) AS city_count, 
    city, 
    MAX(total_transactions_revenue / 1000) 
FROM all_sessions
GROUP BY city
ORDER BY MAX(total_transactions_revenue) DESC
LIMIT 10;

SELECT MAX(total_transactions_revenue)
FROM all_sessions;

SELECT units_sold, 
    revenue 
FROM analytics
GROUP BY units_sold, revenue
ORDER BY revenue DESC;

SELECT country, 
    city, 
    MAX(total_transactions_revenue)
FROM all_sessions
GROUP BY country, city, total_transactions_revenue
ORDER BY total_transactions_revenue DESC;

SELECT country, 
    city,
    MAX(total_transactions_revenue)
FROM all_sessions
WHERE country = 'United States'
GROUP BY country, city, total_transactions_revenue
ORDER BY total_transactions_revenue DESC;

Answer:








**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:

SELECT country, city, AVG(product_quantity)
FROM all_sessions
GROUP BY country, city, product_quantity
ORDER BY product_quantity DESC
LIMIT 10;

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:

SELECT COUNT(product_category) AS count_of_product_category,
	product_category,
	COUNT(product_name) AS count_of_product_name,
	product_name
	country, 
	city
FROM all_sessions
WHERE product_category IS NOT NULL
GROUP BY product_name, 
	product_category, 
	country, 
	city 
ORDER BY product_category DESC;

Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







