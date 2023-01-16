# Data cleaning: What issues will you address by cleaning the data?

This is a summary of the issues that were addressed during the data cleaning:

1. Initially, it is important to understandthe data and to see if it really does make sense. For that first part, I retrieved some data and started to have a look at them to see if I can understand a little bit more about it.

2. After exploring the data, I had the impression that most of the columns had missing, empty or NULL values. Then, I started to filtering columns to identify the NULL values. This step tooked most of my hours and involved:
2.1 cleaning up NULL or empty values, especially from "all_sessions" and "analytics" tables, 
2.2 removing the excess of zeros of the "unit_price" column - which had its name altered to "unit_cost" (from "analytics" table),
2.3 deleting some columns with no values added, such as: "search_keyword", "product_refund_amount", "product_variant", "item_quantity", "item_revenue" - all of them were from "all_sessions" table, and the "user_id" column, from "analytics" table, was also deleted because it only had null values.

3. Part of the data cleaning involved using the CAST function to check the datatype in SELECT statements to run some queries before altering the datatype of some columns. Also, because of that part of the data cleaning, a new table called "price" was created after within already the FLOAT datatype which helped me to perform the correction of the "unit_cost" and "product_price" values;

4. Also, the process of renaming some columns with SQL queries was also performed. Even thought I did a bit of cleaning in the spreadsheets before (e.g. double and single quotes were removed and also some columns renamed there in order to change the column table names in a more consistent naming convention before importing them to the PgAdmin), other columns had their names changed afterwards when needed. Besides, I tried to preserve the naming convetion between the tables database.

5. Finally, I made sure there were common keys between the tables and created a column with a foreing key in the "rpice" table. 

6. Just as a quick addendum, even though I noticed that some of the values between the columsn weren't matching from similar columns in other tables, I found better to maintain them. This also lead me to think that most of them could be randomly put together.


# Queries: what queries were used to clean up the data? 

These were the queries mostly used to clean up the data followed by comments (the queries to create the tables initially in the PgAdmin weren't added here):

-- First, it was chceked the information of the "all_sessions" table to see if it matched with the raw data in the spreadsheet (for example, 32 columns and 15134 rows):

SELECT * 
	FROM all_sessions;
	
SELECT COUNT(*) 
	FROM all_sessions;
	
SELECT channel_grouping, 
    country, 
    v2_product_category 
	FROM all_sessions 
	GROUP BY channel_grouping, country, v2_product_category 
	ORDER BY country
	LIMIT 20;

-- Second, it was performed the same query, but with the other tables (e.g., "analytics" was matched with 14 columns and 1048574 rows, "products" was matched with 7 columns and 1092 rows, "sales_record" with 8 columns and 454 rows, and "sales_by_sku" with 2 columns and 462 rows):

SELECT * 
	FROM analytics;
	
SELECT COUNT(*) 
	FROM analytics;

SELECT * 
	FROM products;
	
SELECT COUNT(*) 
	FROM products;

SELECT * 
	FROM sales_report;

SELECT COUNT(*) 
	FROM sales_report;  

SELECT * 
	FROM sales_by_sku;  

SELECT COUNT(*)  
	FROM sales_by_sku;

-- Third, in order to identify if there were any NULL values in our tables, we used the COUNT(*) function to check the rows followed by the WHERE filtering clause:

SELECT COUNT(total_transactions_revenue) AS total_transactions_revenue
	FROM all_sessions
	WHERE total_transactions_revenue IS NOT NULL; 

SELECT COUNT(transactions) As transactions
	FROM all_sessions
	WHERE transactions IS NOT NULL;

SELECT COUNT(session_quality_dim) AS session_quality
	FROM all_sessions
	WHERE session_quality_dim IS NOT NULL;
	
SELECT COUNT(product_refund_amount) AS product_refund
	FROM all_sessions
	WHERE product_refund_amount IS NOT NULL;
	
SELECT COUNT(product_quantity) AS product_quantity
	FROM all_sessions
	WHERE product_quantity IS NOT NULL;	
	
SELECT COUNT(product_revenue) AS product_revenue
	FROM all_sessions
	WHERE product_revenue IS NOT NULL;	
	
SELECT COUNT(item_quantity) AS item_quantity
	FROM all_sessions
	WHERE product_revenue IS NOT NULL;
	
SELECT COUNT(item_revenue) AS item_revenue
	FROM all_sessions
	WHERE item_revenue IS NOT NULL;			

SELECT COUNT(transaction_revenue) AS transaction_revenue
	FROM all_sessions
	WHERE transaction_revenue IS NOT NULL;	
	
SELECT COUNT(transaction_id) AS transaction_id
	FROM all_sessions
	WHERE transaction_id IS NOT NULL;	
	
SELECT COUNT(ecommerce_action_option) AS ecommerce_action_option
	FROM all_sessions
	WHERE ecommerce_action_option IS NOT NULL;	

 -- Then, it was deleted some columns with empty values in all rows from the table "all_sessions":

 ALTER TABLE public.all_sessions
    DROP COLUMN search_keyword, 
                product_refund_amount, 
                product_variant, 
                item_quantity, 
                item_revenue
;                

-- Afterwards, we checked the possibility of changing the rows that were NULL in the columns which had some data filled out along them. This was done with the COALESCE function, firstly, and then with the UPDATE function. We filled out the rows whose data type was an integer with 0 and a string 'N/A with the ones that were VARCHAR:

SELECT COALESCE(total_transactions_revenue, 0)
    FROM all_sessions
    WHERE total_transactions_revenue IS NULL;

SELECT COALESCE(session_quality_dim, 0)
    FROM all_sessions
    WHERE session_quality_dim IS NULL;

SELECT COALESCE(transactions, 0)
    FROM all_sessions
    WHERE transactions IS NULL;

SELECT COALESCE(product_revenue, 0)
    FROM all_sessions
    WHERE product_revenue IS NULL;

SELECT COALESCE(ecommerce_action_option, 0)
    FROM all_sessions
    WHERE ecommerce_action_option IS NULL;

SELECT COALESCE(transaction_id, 0)
    FROM all_sessions
    WHERE transaction_id IS NULL;

SELECT COALESCE(product_quantity , 0)
    FROM all_sessions
    WHERE product_quantity IS NULL;

-- With this analyze, part of the cleaning was proceded and we could visualize a cleaner and logic table with the queries below (for example, 81 rows were filled out with 0 to substitute the NULL in the "total_revenue_amount" column in the "all_sessions" table), but so much more was done under the other columns:

UPDATE all_sessions  
	SET total_transactions_revenue = 0  
	WHERE  
	total_transactions_revenue IS NULL; 

UPDATE all_sessions  
	SET session_quality_dim = 0   
    WHERE  
    session_quality_dim IS NULL; 

UPDATE all_sessions  
	SET transactions = 0   
    WHERE  
    transactions IS NULL; 

 UPDATE all_sessions  
	SET product_revenue = 0   
    WHERE  
    product_revenue IS NULL;      

UPDATE all_sessions  
	SET ecommerce_action_option = 'N/A'   
    WHERE  
    ecommerce_action_option IS NULL; 

UPDATE all_sessions  
	SET transaction_id = 'N/A'   
    WHERE  
    transaction_id IS NULL;

UPDATE all_sessions  
	SET product_quantity = 0   
    WHERE  
    product_quantity IS NULL;

-- From that on, we proceed to check and clean up also the "analytics" table:

SELECT * 
	FROM analytics;

SELECT user_id
	FROM analytics;
	
SELECT COUNT(user_id)
	FROM analytics 
	WHERE user_id IS NOT NULL;

SELECT COUNT(units_sold)
	FROM analytics 
	WHERE units_sold IS NOT NULL;

SELECT COUNT(time_on_site)
	FROM analytics 
	WHERE time_on_site IS NOT NULL;

SELECT COUNT(revenue)
	FROM analytics 
	WHERE revenue IS NOT NULL;
	
ALTER TABLE public.analytics 
    DROP COLUMN user_id;

SELECT *
	FROM analytics;

-- Again, along that table ("analytics"), it was used the UPDATE function to write the integer 0 in the rows previously marked as NULL in the column "units_sold", initially, and in other columns such as "bounces" and "revenue"(the queries were done one by one in order to better visualize the columns):

UPDATE all_sessions  
	SET total_transactions_revenue = 0  
	WHERE  
	total_transactions_revenue IS NULL; 

UPDATE all_sessions  
	SET session_quality_dim = 0   
    WHERE  
    session_quality_dim IS NULL; 

UPDATE all_sessions  
	SET transactions = 0   
    WHERE  
    transactions IS NULL; 

 UPDATE all_sessions  
	SET product_revenue = 0   
    WHERE  
    product_revenue IS NULL;      

UPDATE all_sessions  
	SET ecommerce_action_option = 'N/A'   
    WHERE  
    ecommerce_action_option IS NULL; 

UPDATE all_sessions  
	SET transaction_id = 'N/A'   
    WHERE  
    transaction_id IS NULL;

UPDATE all_sessions  
	SET product_quantity = 0   
    WHERE  
    product_quantity IS NULL;

-- And, just to confirm if the modifications were done:

SELECT *
	FROM analytics;

-- Then, I came back to the columns names of some tables to check them again if they would need to be renamed in a more consistent way (remember that I had renamed them already before creating the tables in the PgAdmin, so this was a second check to rename some of them in case it was needed):

SELECT *
	FROM analytics;

ALTER TABLE public.analytics
	RENAME COLUMN unit_price TO unit_cost; 
		
SELECT * 
 	FROM all_sessions;
	
SELECT v2_product_name 
 	FROM all_sessions;
	
ALTER TABLE public.all_sessions
	RENAME COLUMN v2_product_name TO product_name; 
	
SELECT v2_product_category
 	FROM all_sessions;
	
ALTER TABLE public.all_sessions
	RENAME COLUMN v2_product_category TO product_category; 	

-- Also, it was cleaned up the excess of zeros of the old "unit_price" column already set up as "unit_cost" from "analytics" (it looks like it was added an irregular amount of zeros in each unit price of the product). Those queries were tried and retrieved the same results:

SELECT div(unit_cost, 1000000)
    FROM analytics;

SELECT unit_cost / 1000000
    FROM analytics
	GROUP BY unit_price;

SELECT div(unit_cost, 1000000)
    FROM analytics;

SELECT unit_cost / 1000000
    FROM analytics
	GROUP BY unit_price;

SELECT s.country, s.product_price, a.unit_cost 
	FROM all_sessions s
	JOIN analytics a
	ON s.visit_id = a.visit_id
	GROUP BY s.country, s.product_price, a.unit_cost;

SELECT s.country, 
	s.product_price / 1000000 AS product_price, 
	s.product_revenue, 
	a.unit_cost / 1000000 AS unit_cost, 
	a.revenue 
	FROM all_sessions s
	LEFT JOIN analytics a
	ON s.visit_id = a.visit_id
	GROUP BY s.country, s.product_price, s.product_revenue, a.unit_cost, a.revenue
	ORDER BY country;

-- After checking the columns "unit_cost" and "product_price" their values were updated to a better reader type format:  

SELECT *
	FROM products;

SELECT *
	FROM analytics;

UPDATE analytics
	SET unit_cost = unit_cost / 1000000
;

SELECT unit_cost
	FROM analytics; 

SELECT product_price
	FROM all_sessions;

UPDATE all_sessions
	SET product_price = product_price / 1000000
;

SELECT product_price 
	FROM all_sessions;

-- Obs.: AS I ran into some issues with my query above which was ran twice by mistake, I had to create a new table with the columns "unit_cost" and "product_cost" (previously called "unit_price" and "product_price" and import data again of them to redo my mistake. After creating the new table called "price" with those columns, I added to it 2 keys: the "full_visitor_id" (primary key) and "product_sku" (foreign key) with the INSERT INTO statement:

CREATE TABLE price (
	full_visitor_id VARCHAR,
	unit_price FLOAT,
	product_price FLOAT
	);
	
SELECT unit_price / 1000000 AS unit_cost,
    product_price / 1000000 As product_cost
    FROM price;
	
UPDATE price
SET unit_price = unit_price / 1000000
;

ALTER TABLE price
RENAME COLUMN unit_price TO unit_cost
;

UPDATE price  
SET product_price = product_price / 1000000
;

ALTER TABLE price
RENAME COLUMN product_price TO product_cost
;

ALTER TABLE price
ADD COLUMN name VARCHAR
;

SELECT * FROM price;

SELECT price.*, 
    sales_by_sku.full_visitor_id
    FROM price
    LEFT JOIN sales_by_sku
    ON price.full_visitor_id = sales_by_sku.full_visitor_id;

INSERT INTO sales_by_sku (full_visitor_id)
    SELECT full_visitor_id
    FROM price
;

SELECT * FROM sales_by_sku
    ORDER BY full_visitor_id DESC;

SELECT * FROM price;

ALTER TABLE price
    ADD COLUMN product_sku VARCHAR
;

INSERT INTO price(product_sku)
    SELECT product_sku
    FROM sales_by_sku
;

-- By joining these tables to visualize their values and the way the prices were formatted, I noticed some NULL values that weren't noticed before under "unit_cost" and "revenue" columns. They didn't have much of rows with NULL values and I thought about replacing them with a 0 at first, but as I was not sure about this changing yet, I left it for now.

SELECT * 
	FROM all_sessions;
	
SELECT * 
	FROM analytics;
	
SELECT COUNT(unit_cost) 
	FROM analytics
	WHERE unit_cost IS NOT NULL;

SELECT * 
	FROM products;
	
SELECT COUNT(sentiment_score)
	FROM products
	WHERE sentiment_score IS NOT NULL;

-- Finally, I noticed some columns had the same value in every row and it looked like they wouldn't make any difference for this project (they might be important to other ones, but it seems that not for this one). Due to that, I decided to drop some more of them in the "all_sessions" table, such as the ones below: 

SELECT COUNT(ecommerce_action_type) 
	FROM all_sessions;

SELECT ecommerce_action_type 
	FROM all_sessions
	WHERE ecommerce_action_type = 0;
	
SELECT COUNT(ecommerce_action_type) 
	FROM all_sessions
	WHERE ecommerce_action_type = 0;

SELECT ecommerce_action_step 
	FROM all_sessions;
	
SELECT COUNT(ecommerce_action_step) 
	FROM all_sessions;

SELECT COUNT(ecommerce_action_step) 
	FROM all_sessions
	WHERE ecommerce_action_step = 1;
	
SELECT ecommerce_action_option 
	FROM all_sessions;
	
SELECT COUNT(ecommerce_action_option) 
	FROM all_sessions
	WHERE ecommerce_action_otpion = N/A;

ALTER TABLE all_sessions
	DROP COLUMN ecommerce_action_type,
	DROP COLUMN ecommerce_action_step,
	DROP COLUMN ecommerce_action_option
;

SELECT * 
	FROM all_sessions;
	
SELECT * 
	FROM analytics;
	
SELECT bounces
	FROM analytics;
	
SELECT COUNT(bounces) 
	FROM analytics
	WHERE bounces = 0;
	
ALTER TABLE analytics
	DROP COLUMN bounces
;

SELECT * 
	FROM products;
	
SELECT * 
	FROM sales_report;
	
SELECT p.sku, s.product_sku
	FROM products p
	LEFT JOIN sales_report s
	ON p.name = s.name
	GROUP BY p.sku, s.product_sku, p.name;
	
ALTER TABLE products
    RENAME COLUMN sku to product_sku
;













	


