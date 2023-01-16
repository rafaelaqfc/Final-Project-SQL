Question 1: What is the average amount spent per unit product price?

--- Trying to make sense of the average amount of the unit price.

SELECT unit_price / 1000000 AS unit_cost, 
    AVG(unit_price) / 1000000 AS avg_unit_price
	FROM analytics
	GROUP BY unit_cost
	ORDER BY unit_cost;

Answer: 



Question 2: What is the relationship between the costumer, the products bought and the average price of them? 

--- Looking at the relationship between the costumer, the product that they bought and the average of the price of the product bought. 

SELECT visit_id, 
    visit_number, 
    AVG(unit_price)
	FROM analytics
	GROUP BY visit_number, visit_id
	ORDER BY visit_id ASC;
	
SELECT visit_id, 
    visit_number, 
    AVG(unit_price)
	FROM analytics
	GROUP BY visit_number, visit_id
	ORDER BY AVG(unit_price) DESC;

Answer: From that on, we can see the visitor id number "1498662260" spent 






Question 3: 

SQL Queries:

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
