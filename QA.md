What are your risk areas? Identify and describe them.

The risk areas for this data set are:
	1 - Duplicate entries
	2 - Large amount of missing values 
	3 - Primary and foreign keys not properly define
	4 - Inconsistency of the data
	5 - Improper format for some arguments (date captured as numeric)

_____________________________________________________________________________________________________________________________________________

QA Process:

Describe your QA process and include the SQL queries used to execute it.

The tables "all_sessions" and "analytics" contain dynamic data set. The variability of the entries over time can be considered to validate.

1 - Let's use the login date to analyse the distribution of the visits over days for table all_sessions.

SQL:

SELECT DISTINCT * FROM 
(					SELECT DISTINCT DATE(date_), 
					COUNT(*) 
 					FROM all_sessions
					WHERE date_ NOTNULL
					GROUP BY 1
					ORDER BY date_ ASC
) AS tempo

The result is distributed over 366 days

SELECT ROUND(AVG(count),0) 
FROM 
(	SELECT DISTINCT * FROM 
 				(SELECT DISTINCT DATE(date_), 
				 COUNT(*)
				 FROM all_sessions
				 WHERE date_ NOTNULL
				 GROUP BY 1
				 ORDER BY date_ DESC
                ) as tempo
) AS tempo

The mean is 40


SELECT 
	*, 
	COALESCE(100 * (count/LAG(count) OVER(ORDER BY count DESC) - 1), 0) || '%'
FROM (
		SELECT DISTINCT * 
		FROM (
			  SELECT 
				DISTINCT DATE(date_) AS date_, 
			  	COUNT(*)
			  FROM all_sessions
			  WHERE date_ NOTNULL
			  GROUP BY 1
			  ORDER BY count DESC
			) AS tempo
     ) AS tempo
GROUP BY date_, count
ORDER BY date_

The variability stays under 200% that is acceptable for a random variable on website visits
_____________________________________________________________________________________________________________________

2 - Let's use the login date to analyse the distribution of the visits over days for table analytics.

SQL:

SELECT DISTINCT * FROM 
(					SELECT DISTINCT DATE(date_), 
					COUNT(*) 
 					FROM analytics
					WHERE date NOTNULL
					GROUP BY 1
					ORDER BY date_ ASC
) AS tempo

The result is distributed over 93 days

SELECT ROUND(AVG(count),0) 
FROM 
(	SELECT DISTINCT * FROM 
 				(SELECT DISTINCT DATE(date_), 
				 COUNT(*)
				 FROM analyticss
				 WHERE date_ NOTNULL
				 GROUP BY 1
				 ORDER BY date_ DESC
                ) as tempo
) AS tempo

The mean is 18702 which flags for further investigation on this table to ensure this flow is realistic.

SELECT 
	*, 
	COALESCE(100 * (count/LAG(count) OVER(ORDER BY count DESC) - 1), 0) || '%'
FROM (
		SELECT DISTINCT * 
		FROM (
			  SELECT 
				DISTINCT DATE(date_) AS date_, 
			  	COUNT(*)
			  FROM analytics
			  WHERE date_ NOTNULL
			  GROUP BY 1
			  ORDER BY count DESC
			) AS tempo
     ) AS tempo
GROUP BY date_, count
ORDER BY date_

The variability stays under 100% that is acceptable for a random variable on website visits. However, as mentioned above the mean flags for further 
investigation. And also the fact that after applying a primary key on "visitid", the table will loose about 97% of its information. It seems not 
reliable to use this information as it's aggregated into the table. It may be more useful to pursue more investigation to split this table. It is 
possible that rearanging this table into sub table may result to more reliable set of data.
__________________________________________________________________________________________________________________________________________

The tables "sales_report" and "products" contain static data set. To validate we can consider
	1 - The percentage of data loss after removing duplicates
	2 - Amount of missing data 
	3 - The credibility of the records
	4 - The data format
___________________________________________________________________________________________________________________________________________

"sales_report"

1 - SELECT COUNT(*) FROM sales_report = SELECT DISTINCT productsku FROM sales_report so productsku can be set as primary key without data loss.

2 - Using the script

SELECT COUNT(*) FROM sales_report 
where [column] = NULL

on all the other columns will return 0. No data loss

3 - The values stay within credible bounds from 0 to 465.

Using the scripts 
SELECT MAX([column]) FROM sales_report and SELECT MIN([column]) FROM sales_report

on all the columns will give the minimum and maximum bounds to compare the values

4 - The columns have the proper format


"products"

1 - SELECT COUNT(*) FROM products = SELECT DISTINCT SKU FROM products so SKU can be set as primary key without data loss.

2 - Using the script

SELECT COUNT(*) FROM products 
where [column] = NULL

on all the other columns will return 0. No data loss

3 - The values stay within credible bounds from 0 - 15170. But the mean is 143.53, so further investion on the company business may be helpful.

Using the scripts 
SELECT MAX([column]) FROM products and SELECT MIN([column]) FROM products

on all the columns will give the minimum and maximum bounds to compare the values

4 - The columns have the proper format
_____________________________________________________________________________________________________________________________

Let's analyse discrepencies between sales_report.productSKU and product.SKU


