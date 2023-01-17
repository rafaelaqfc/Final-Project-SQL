### What are your *risk areas*? Identify and describe them.

<br/>

- One of the first risk areas of the database to be assessed is the way that the tables were organized. As the data is still not so structured and organized and more cleaning should be done, I started the `QA` process by acessing the tables. So I came back again to the spreadshhets in excel (the raw data) to see if I was missing something, and if the tables I created in `PgAdmin GUI` were consistent with the ones from the excel. One of the first things I did it here, it was to check the price table again (created as another table from the ones imported). The table `price` was renamed as `prices` because I want to stick with the consistency in the naming convention:

<br/>

```SQL
ALTER TABLE price
RENAME TO prices;

ALTER TABLE all_sessions
ADD COLUMN product_cost FLOAT;
	
INSERT INTO all_sessions(product_cost)
SELECT product_cost
FROM prices;

SELECT product_cost 
FROM all_sessions;

ALTER TABLE analytics
ADD COLUMN unit_cost FLOAT;

INSERT INTO analytics(unit_cost)
SELECT unit_cost
FROM prices;

SELECT unit_cost
FROM analytics
WHERE unit_cost IS NOT NULL;
```	
<br/>

- Therefore, by checking the price of the whole product manufactured and the price of the unit and seeing that they were different, but somewhat related - as already analyzed -, I decided to move them back to to their previous tables called `all_sessions` and `analytics` to see if they could provide me more information there by openning their perspective in relation to other columns. Then, after aktering the tables aforementioned, I decided to move forward and check the data values and types again. This was my second area addressed in the `QA`:

<br/>

```SQL
SELECT * 
    FROM all_sessions;

SELECT * 
    FROM all_sessions
    WHERE full_visitor_id IS NOT NULL;
```
<br/>

- By running those 2 queries, it is noted that there are many rows with empty values. In other words, the output didn't match. There are 2,112,746 rows in total in the `all_sessions` table, whereas there are 15134 rows with values associated with a `full_visitor_id`. The reason why the `full_visitor_id` was considered as an important value is because I would like to understand the story behing every visit at the company's website in order to create a `profile of their users` as most of the data showed that the customers only clicked once in the site and almost were never buying anything. I found this unusual for an ecommerce cmpany. So, this inconsistency must be addressed and another cleaning should be done. 
<br/>

- Also, by running the query below, I could see that the output of the `full_visitor_id` and the `visit_id` values matched, therefore the `full_visitor_id` and `visit_id` are important values to be considered when performing our analysis as they could give us the same output (meaning that the values are consistent and distributed when considering the search by the `full_visitor_id` and the `visit_id`):

<br/>

```SQL
SELECT * 
FROM all_sessions
WHERE full_visitor_id IS NOT NULL;

SELECT *
FROM all_sessions
WHERE visit_id IS NOT NULL;
```
<br/>

- After doing that, it was noted that the data still has lots of duplicate and `NULL` values. Therefore, it must be cleaned and validated again in order to gain quality. From this process, I learned that the `QA` is not a final step of a data analysis project, but mostly a step that you would come and go as you need to make your data consistent, accurate, objective, valid and unique. This is a good approach if you are seeking for more accurate results.




