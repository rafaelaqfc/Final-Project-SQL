### Starting with *data*

<br/>

*Question 1: What is the average amount spent per unit product price?*
<br/>

- This question came out because I was trying to make sense of the average amount of the `unit_cost` and `product_cost` to see if there are any parameters between these two variables:

<br/>

```SQL
SELECT AVG(unit_cost / 10000) AS avg_unit_cost
    FROM price;

SELECT AVG(product_cost / 10000) AS avg_product_cost
    FROM price;	

SELECT product_cost 
        ,unit_cost 
FROM price
WHERE product_cost IS NOT NULL
ORDER BY product_cost DESC;	

SELECT AVG(product_cost) 
FROM price
WHERE product_cost IS NOT NULL;
```

<br/>

*Answer:*

As the average of the `unit_cost` didn't make sense to me at first - the column still has incomplete and innacurate values -, I tried to run different queries to make sense of the average amount of the producs sold by the company. Whereas the `average` of the `cost of each unit` came up to be `U$D 3.11`, the average of the `product cost` was about `U$D 28.81`. Therefore, these 2 variables represent different aspects of the production cost, but it is not sure by the data provided in the demo dataset if the `unit_cost` means the value a single part of the product wheres the `product_cost` would mean the price of the whole product manufactured. Similarly, this information didn't give me much much more than that, because the unit cost and product cost represent different variables, even thought they were not described in the database. So, they could only give me much more information if seen in a related way which cannot be done right now, but maybe in the future: `for example, the average of the unit cost (U$D 3.11) could represent almost 10 times what represents the price of the product (U$D 28.81) in the end`. 

<br/>

*Question 2: Is there a relationship between a costumer profile, their products bought and the average price of them?* 

- Now, I am looking at the relationship between the costumer, the product that they bought and the average of the price of the product bought. 

<br/>

```SQL
SELECT visit_id 
    ,visit_number 
    AVG(unit_price)
FROM analytics
GROUP BY visit_number 
            ,visit_id
ORDER BY visit_id ASC;
	
SELECT visit_id 
    ,visit_number 
    ,AVG(unit_price)
FROM analytics
GROUP BY visit_number 
            ,visit_id
ORDER BY AVG(unit_price) DESC;

SELECT a.visit_id 
    ,a.visit_number 
	,p.product_cost 
	p.unit_cost
FROM analytics a
LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
GROUP BY visit_number 
		    ,visit_id 
		    ,product_cost 
		    unit_cost
ORDER BY visit_id;

SELECT a.visit_id 
    ,a.visit_number 
	,p.product_cost 
	p.unit_cost
FROM analytics a
LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
WHERE product_cost IS NOT NULL
GROUP BY visit_number 
		,visit_id 
		,product_cost 
		unit_cost
ORDER BY product_cost DESC;

SELECT a.visit_id 
    ,a.visit_number 
	,p.product_cost 
	,p.unit_cost
FROM analytics a
LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
WHERE visit_id = '1497154760'
GROUP BY visit_number 
		,visit_id 
		,product_cost 
		unit_cost
ORDER BY product_cost DESC;
```
<br/>

*Answer:*

From that on, it was possible to see that the customer with the 'visit_id' value of `1497154760` spent U$D 298 and U$D 109.99 in two products sold by the ecommerce company. This customer came out to my curiosity because he/she had the higher buy on the company. Also, after I tried to run more queries to grasp more information about this customer:

<br/>

```SQL
SELECT a.visit_id 
	,a.revenue
	,s.full_visitor_id
	,s.country 
	,s.city
	s.purchase_date
FROM analytics a
LEFT JOIN all_sessions s ON a.full_visitor_id = s.full_visitor_id
WHERE a.visit_id = '1497154760'
GROUP BY a.visit_id
	,a.revenue
	,s.full_visitor_id
	,s.country 
	,s.city
	s.purchase_date
ORDER BY purchase_date DESC;
```
<br/>

However, no information was possible to retrieve. Then, I decided to run one more query to see if any information was available:

<br/>

```SQL
SELECT * 
FROM analytics 
WHERE visit_id = '1497154760';

SELECT * 
FROM analytics a
LEFT JOIN all_sessions s
ON a.visit_id = s.visit_id
WHERE a.visit_id = '1497154760';
```
<br/>

With this query it was possible to retrieve some information about this customer, such as: is (1) channeled in an `organic search` group, (2) not social engaged, and (3) has the `full_visitor_id` value of `5.07878E+16`.

<br/>

*Question 3: Is there a relationship between the sentiment of a general costumer and the product bought?* 

- Now, I am trying to analyze if there is a relationship that could be tracked down between the sentiment of a common costumer in regards of the product they bought:

<br/>

```SQL
SELECT * 
FROM sales_report;

SELECT s.name
	,s.total_ordered
    ,ROUND(s.sentiment_score) AS sentiment_score 
    ,ROUND(s.sentiment_magnitude) AS sentiment_magnitude
FROM sales_report s
GROUP BY name 
	,s.total_ordered
    ,s.sentiment_score 
    ,s.sentiment_magnitude
ORDER BY sentiment_magnitude DESC
LIMIT 10;
```
<br/>

*Answer:*

 It looks like the product `name` `Women's V-Neck Tee Charcoal` is the one highlighted. Even though it has same `sentiment score` and `sentiment magnitude` given by others, it is the one with a higher amount of orders (4 units). Therefore, this made me to think that the value of the variable `total_ordered` was fundamental to analyze this data and show up the information with being more consumed by their customers and, probably, more likeable by them.




