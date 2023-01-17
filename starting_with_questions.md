Answer the following questions and provide the SQL queries used to find the answer.
    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

```SQL
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
GROUP BY units_sold, 
        revenue
ORDER BY revenue DESC;

SELECT country, 
    city, 
    MAX(total_transactions_revenue)
FROM all_sessions
GROUP BY country, 
            city, 
            total_transactions_revenue
ORDER BY total_transactions_revenue DESC;

SELECT country, 
        city,
        MAX(total_transactions_revenue)
FROM all_sessions
WHERE country = 'United States'
GROUP BY country, 
            city, 
        total_transactions_revenue
ORDER BY total_transactions_revenue DESC;

Answer:
After trying some of the queries above, I found that the top 10 countries with the highest total transactions revenue were: United States, Israel, Australia, Canada, Switzerlad, Uganda, Montenegro, Venezuela, Cambodia and Sweden. In regards of the cities, these were the top 10: Atlanta, Sunnyvale, Tel Aviv-Yafo, Los Angeles, Sydney, Seatlle, Chicago, Palo Alto and San Francisco. However, it is important to note that the row with the MAX amount of total_transactions_revenue didn't have the city names available (they were missing values).

**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:

SELECT country, 
        city, 
        AVG(product_quantity)
FROM all_sessions
GROUP BY country, 
        city, 
        product_quantity
ORDER BY product_quantity DESC
LIMIT 10;

SELECT country, 
        city, 
        AVG(product_quantity)
FROM all_sessions
WHERE product_quantity IS NOT NULL
GROUP BY country, 
        city, 
        product_quantity
ORDER BY product_quantity DESC
LIMIT 10;
````

Answer: 
I tried both queries just to make sure if the results would be the same. As I did the clean up of the NULL values of the "product_quantity" column, this filtering condition wouldn't be mandatory in this case. So, the average products ordered from visitors in each city and country pointed to us an inexact answer: first, we have the United States (with some of the missing city values) which leaded the average of products ordered, with the average of 65, and, after, the United States is shown up again (with another group of missing cities) with the average bought of 50. Following USA, we had Spain (Madri) with the average of products ordered by 10 and other cities of USA, such as Salem, Atlanta, New York and Houston as the ones with the higher average products ordered by their visitors.  

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:

```SQL
SELECT COUNT(product_category) AS count_of_product_category,
	product_category,
	product_name,
	product_quantity,
	country, 
	city
FROM all_sessions
WHERE product_category IS NOT NULL
GROUP BY product_name, 
	product_category, 
	product_quantity,
	country, 
	city 
ORDER BY product_quantity DESC;

SELECT COUNT(product_category) AS count_of_product_category,
	product_category,
	product_name,
	product_quantity,
	country, 
	city
FROM all_sessions
WHERE product_category = 'Bags'
GROUP BY product_name, 
	product_category, 
	product_quantity,
	country, 
	city 
ORDER BY product_quantity DESC;

SELECT COUNT(product_category) AS count_of_product_category,
	product_category,
	product_name,
	product_quantity,
	country, 
	city
FROM all_sessions
WHERE product_category = 'Home/Office/Notebooks & Journals/'
GROUP BY product_name, 
	product_category, 
	product_quantity,
	country, 
	city 
ORDER BY product_quantity DESC;
````

Answer: 
After running this query, it is noted that most of the products ordered were related to the home office category in the first place and in the reusing bags cattegory in the second. Both of the products which belonged to those 2 categories were ordered by visitors from USA. Althoug it was not possible to retrieve the city because the value was not available in the dataset, USA cities counted for 115 products ordered inside of those 2 categories. Also, after running the second and third query shown up above, it was noted that from 54 reusable shopping bags were ordered by customers from USA cities (4 at least from customers from Atlanta); and of 65 of notebooks/journals bought all of them were ordered by the USA. 

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```SQL
SELECT SUM(product_quantity),
		product_category,
		product_name,
		country, 
		city
FROM all_sessions
WHERE product_name IS NOT NULL
GROUP BY product_category, 
		product_name,
		product_quantity,
		country, 
		city 
ORDER BY product_quantity DESC;

SELECT *
    FROM analytics;

 SELECT SUM(product_quantity),
		product_category,
		product_name,
		country, 
		city
FROM all_sessions
WHERE country = 'Mexico'
GROUP BY product_category, 
		product_name,
		product_quantity,
		country, 
		city 
ORDER BY product_quantity DESC;

SELECT SUM(product_quantity),
		product_category,
		product_name,
		country, 
		city
FROM all_sessions
WHERE country = 'Finland'
GROUP BY product_category, 
		product_name,
		product_quantity,
		country, 
		city 
ORDER BY product_quantity DESC;
````

Answer:
I found this question very similar to the 3rd, therefore I obtained the same results from the question above. This query showed that USA has the higher amount of home office (notebooks and journals) and reusable bags ordered by their customers. I also tried to check the "untis_sold" column in the "analytics" table, but it gave mostly 0 products sold. So, coming back to our first query of this question, it was also possible to find that the most sold product in Spain was a specific type of socks with cat design. Even with the other queries which ran with a filtering condition of a specific country, they couldn't provide significative information about the top products from their countries.

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
SELECT country,
        city, 
        SUM(transaction_revenue) AS transaction_revenue,
		SUM(total_transactions_revenue) AS total_revenue
FROM all_sessions
WHERE total_transactions_revenue != 0
GROUP BY country,
        city,
        transaction_revenue,
		total_transactions_revenue
ORDER BY total_revenue DESC;

SELECT country, 
		SUM(total_transactions_revenue) AS total_revenue
FROM all_sessions
WHERE total_transactions_revenue != 0
GROUP BY country
ORDER BY total_revenue DESC;

SELECT city, 
		SUM(total_transactions_revenue) AS total_revenue
FROM all_sessions
WHERE total_transactions_revenue != 0
GROUP BY city
ORDER BY total_revenue DESC;

SELECT country,
        city, 
        SUM(transaction_revenue) AS transaction_revenue,
		SUM(total_transactions_revenue) AS total_revenue
FROM all_sessions
WHERE country = 'United States'
GROUP BY country,
        city,
        transaction_revenue,
		total_transactions_revenue
ORDER BY total_revenue DESC;

SELECT country,
        city, 
        SUM(transaction_revenue) AS transaction_revenue,
		SUM(total_transactions_revenue) AS total_revenue
FROM all_sessions
WHERE country = 'Canada'
GROUP BY country,
        city,
        transaction_revenue,
		total_transactions_revenue
ORDER BY total_revenue DESC;
````

Answer: 
After running those queries and trying filtering the country value with the WHERE condition, it was noted that USA (USD$ 13,154,170,000), Israel, Australia, Canada and Switzerland were the five countries with the higher total revenue. Also, the cities of each country which genereated the total amount revenue where Sunnyvale and Atlanta, in the USA; and Toronto and New York, in Canada.

--X--








