Question 1: What is the average amount spent per unit product price?

-- Trying to make sense of the average amount of the unit price.

SELECT AVG(unit_cost / 10000) AS avg_unit_cost
    FROM price;

SELECT AVG(product_cost / 10000) AS avg_product_cost
    FROM price;	

SELECT product_cost, 
        unit_cost 
    FROM price
WHERE product_cost IS NOT NULL
ORDER BY product_cost DESC;	

SELECT AVG(product_cost) 
    FROM price
    WHERE product_cost IS NOT NULL;

Answer: 
As the average of the unit_cost didn't make sense to me at first - the column still has incomplete and innacurate values -, I tried to run different queries to make sense of the average amount of the producs sold by the ecommerce. Whereas the average of the cost of each unit or product came up to be about U$D 3.11, the average of the product cost was about 28.81. Even thought it was possible to get this informatio, it didn't shown up to be accurate and matching with each other.

Question 2: What is the relationship between the costumer, the products bought and the average price of them? 

-- Looking at the relationship between the costumer, the product that they bought and the average of the price of the product bought. 

SELECT visit_id, 
    visit_number, 
    AVG(unit_price)
	FROM analytics
	GROUP BY visit_number, 
            visit_id
	ORDER BY visit_id ASC;
	
SELECT visit_id, 
    visit_number, 
    AVG(unit_price)
	FROM analytics
	GROUP BY visit_number, 
            visit_id
	ORDER BY AVG(unit_price) DESC;

SELECT a.visit_id, 
    a.visit_number, 
	p.product_cost, 
	p.unit_cost
	FROM analytics a
	LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
	GROUP BY visit_number, 
		    visit_id, 
		    product_cost, 
		    unit_cost
	ORDER BY visit_id;

SELECT a.visit_id, 
    a.visit_number, 
	p.product_cost, 
	p.unit_cost
	FROM analytics a
	LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
    WHERE product_cost IS NOT NULL
	GROUP BY visit_number, 
		visit_id, 
		product_cost, 
		unit_cost
	ORDER BY product_cost DESC;

SELECT a.visit_id, 
    a.visit_number, 
	p.product_cost, 
	p.unit_cost
	FROM analytics a
	LEFT JOIN price p ON a.full_visitor_id = p.full_visitor_id
	WHERE visit_id = '1497154760'
	GROUP BY visit_number, 
		visit_id, 
		product_cost, 
		unit_cost
	ORDER BY product_cost DESC;

Answer: From that on, it was possible to see the customer with the visit id 1497154760 number spent U$D 298 and U$D 109.99 in the ecommerce. This customer came out to my curiosity because it had the higher buy on the company. Also, from that on, I tried to run more queries to grasp more information about this customer:

SELECT a.visit_id, 
	a.revenue,
	s.full_visitor_id,
	s.country, 
	s.city,
	s.purchase_date
	FROM analytics a
	LEFT JOIN all_sessions s ON a.full_visitor_id = s.full_visitor_id
    WHERE a.visit_id = '1497154760'
	GROUP BY a.visit_id,
	a.revenue,
	s.full_visitor_id,
	s.country, 
	s.city,
	s.purchase_date
	ORDER BY purchase_date DESC;

-- However, no information was possible to retrieve. Then, I decided to run one more query to see if any information was available:

SELECT * 
FROM analytics 
WHERE visit_id = '1497154760';

-- Not only this query work the best, but also with that it was possible to retrieve he is channeled in an "organic search" group, it is not social engaged and it has the visitor_id number "full_visitor_id" of "5.07878E+16".

Question 3: Is there a relationship between the sentiment of a costumer and the product bought?

-- Analyzing if there is a relationship that could be tracked down between the sentiment of a costumer in regards of the product they bought.

SQL Queries:

SELECT * 
FROM sales_report;

SELECT s.name, 
	s.total_ordered,
    ROUND(s.sentiment_score) AS sentiment_score, 
    ROUND(s.sentiment_magnitude) AS sentiment_magnitude
    FROM sales_report s
    GROUP BY name, 
	s.total_ordered,
    s.sentiment_score, 
    s.sentiment_magnitude
	ORDER BY sentiment_magnitude DESC
	LIMIT 10;

Answer:
It looks like the product "name" "Women's V-Neck Tee Charcoal" is the one highlighted. Even though it has same sentiment score and sentiment magnitude given by others, it is the one with a higher amount of orders (4 units). Therefore, the value of the column "total_ordered" was fundamental to analyze this data.

--X--

