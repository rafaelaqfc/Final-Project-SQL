### What are your risk areas? Identify and describe them.

##### One of the risk areas of the database is the way that the tables were organized. As the data is still not so structured and organized and more cleaning should be done, I started the `QA` process by acessing the tables. So I started by chequing again the spreadshhets in excel (the raw data) to see if I was missing something and the tables I created in `PgAdmin GUI` to make sure there is consistency and that I wasn't missing anything. One of the first things I did it here, it was to check the price table again (created as another table from the ones imported) The table price was renamed as prices because I want to stick with the consistency in the naming convention:


```SQL
ALTER TABLE price
    RENAME TO prices
;

ALTER TABLE all_sessions
    ADD COLUMN product_cost FLOAT
;
	
INSERT INTO all_sessions(product_cost)
    SELECT product_cost
    FROM prices
;

SELECT product_cost 
    FROM all_sessions
;

ALTER TABLE analytics
    ADD COLUMN unit_cost FLOAT
;

INSERT INTO analytics(unit_cost)
    SELECT unit_cost
    FROM prices
;

SELECT unit_cost
    FROM analytics
    WHERE unit_cost IS NOT NULL
;
```	


##### Therefore, the price of the whole and the price of the unit are different things and they should be analyzed as different, but still somewhat related, values. This is the reason why I moved them to their previous tables called `all_sessions` and `analytics`. Then, after checking this table and droping it, I decided to move forward and check the data values and types again. This was my second area addressed in my QA:



```SQL
SELECT * 
    FROM all_sessions;

SELECT * 
    FROM all_sessions
    WHERE full_visitor_id IS NOT NULL;
````

##### By running those 2 queries, it is noted that there are many rows with empty values. In other words, the output doesn't match. There are 2,112,746 rows in total in the all_sessions table, whereas there are 15134 rows with values associated with a `full_visitor_id`. The reason why the `full_visitor_id` was considered is because I would like to understand the story behing every visit at the company's website in order to create a profile of their users. This inconsistency must be addressed and another cleaning should be done. On the other hand, by running the query below, I could see that the output of the `full_visitor_id` and the `visit_id` values matched, therefore the full_visitor_id and visit_id are important values to be considered when performing our analysis as they could give us the same output (values are consistent and distributed):


```SQL
SELECT * 
FROM all_sessions
WHERE full_visitor_id IS NOT NULL;

SELECT *
FROM all_sessions
WHERE visit_id IS NOT NULL;
````

##### After doing that, it was noted that the data still has lots of duplicate and `NULL` values. Therefore, it must be cleaned and validated again in order to gain quality. From this process, I learned that the `QA` is not a final step of a data analysis project, but mostly a step that you come and go as you need to make your data consistent and unique. This is a good approach if you are seeking for more accurate results.


