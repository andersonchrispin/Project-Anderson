

**Question 1: What is the product category that has the highest level of sale


SQL Queries:

WITH tempo AS (
	SELECT 
		name_, 
		SUM(total_ordered) 
	FROM sales_report
	WHERE total_ordered NOTNULL
	GROUP by name_
             ) 
SELECT 
	name_, 
	sum
FROM tempo
WHERE sum = ( SELECT MAX(sum) FROM tempo )


Answer:

Category	              Total Sales
Ballpoint LED Light Pen	  456

_________________________________________________________________________________________________________________________________________


**Question 2: How many products was ordered for more than 15 units


SQL Queries:

SELECT COUNT(*) FROM products
WHERE orderedquantity > 15


Answer:

497

______________________________________________________________________________________________________________________________________

**Question 3: What is the average visits per day on the site


SQL Queries:

WITH tempo AS (
	   SELECT date_,
	          COUNT(*) 
       FROM all_sessions
       GROUP BY date_
              )
SELECT ROUND(AVG(count),2) FROM tempo


Answer:

39.77

__________________________________________________________________________________________________________________________________________

**Question 4: The average time spent on the site by visit


SQL Queries:

  SELECT 
	AVG(timeonsite) 
  FROM all_sessions


Answer:

223,60



**Question 5: 

SQL Queries:



Answer:
