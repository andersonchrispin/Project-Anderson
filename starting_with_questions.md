Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: 

Which cities and countries have the highest level of transaction revenues on the site?
I am kind of confused on this one. Since the question asks for many countries, cities and transaction revenues, it may mean the highest level of transaction revenue for each city. It can also mean the city that has the highest level of transaction revenue. So I give both answers. 

SQL Queries:
Highest level of transaction revenue for each city

SELECT  
	city,
	country, 
	MAX(totaltransactionrevenue) 
FROM all_sessions
WHERE totaltransactionrevenue NOTNULL
GROUP BY country,city
ORDER BY city

The city that has the highest level of transaction revenue

SELECT  
	city,
	country, 
	totaltransactionrevenue
FROM all_sessions
WHERE 
totaltransactionrevenue = (SELECT MAX(totaltransactionrevenue) FROM all_sessions WHERE city != 'not available in demo dataset')


Answer: 
Highest level of transaction revenue for each city

City	      Country	      Max
Atlanta	      United States	  742480000
Austin	      United States	  122000000
Chicago	      United States	  306000000
Columbus	  United States	  21990000
Houston	      United States	  38980000
Los Angeles	  United States	  363000000
Mountain View United States	  156000000
Nashville	  United States	  157000000
New York	  Canada	      67990000
New York	  United States	  152000000
Palo Alto	  United States	  305000000
San Bruno	  United States	  103770000
San Francisco United States	  301000000
San Jose	  United States	  154000000
Seattle	      United States	  358000000
Sunnyvale	  United States	  649240000
Sydney	      Australia	      358000000
Tel Aviv-Yafo Israel	      602000000
Toronto	      Canada	      82160000
Zurich	      Switzerland	  16990000

The city that has the highest level of transaction revenue

City	  Country	      Max
Atlanta	  United States	  742480000

_______________________________________________________________________________________________________________________________

Question 2: 

What is the average number of products ordered from visitors in each city and country?

SQL Queries:

SELECT  
	city,
	country, 
	AVG(productquantity) 
FROM all_sessions
WHERE productquantity NOTNULL
GROUP BY country,city
ORDER BY city

Answer:

Country	        City	                        Average Product Quantity/City
Argentina	    not available in demo dataset	1
Canada	        not available in demo dataset	1
Colombia	    not available in demo dataset	1
Finland	        not available in demo dataset	1
France	        not available in demo dataset	1
India	        Bengaluru	                    1
Ireland	        Dublin	                        1
Mexico	        not available in demo dataset	1
Spain	        Madrid	                        10
United States	(not set)	                    1
United States	Ann Arbor	                    1
United States	Atlanta	                        4
United States	Chicago	                        1
United States	Columbus	                    1
United States	Dallas	                        1
United States	Detroit	                        1
United States	Houston	                        2
United States	Los Angeles	                    1
United States	Mountain View	                1
United States	New York	                    1
United States	not available in demo dataset	11
United States	Palo Alto	                    1
United States	Salem	                        8
United States	San Francisco	                1
United States	San Jose	                    1
United States	Seattle	                        1
United States	Sunnyvale	                    1

_____________________________________________________________________________________________________________________________________________

Question 3: 

Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

SELECT  
city,
country, 
v2productcategory,
productquantity
from all_sessions
where productquantity notnull
group by city,v2productcategory,productquantity,country

Answer:

County	        City	                        Category	                              Product Quantity
Argentina	    not available in demo dataset	Home/Bags/Backpacks/	                  1
Canada	        not available in demo dataset	Home/Apparel/Kid's/Kids-Youth/	          1
Canada	        not available in demo dataset	Home/Shop by Brand/YouTube/	              1
Colombia	    not available in demo dataset	Home/Apparel/Men's/Men's-T-Shirts/	      1
France	        not available in demo dataset	Headgear	                              1
India	        Bengaluru	                    Apparel	                                  1
Ireland	        Dublin	                        Home/Bags/	                              1
United States	Mountain View	                Apparel	                                  1
United States	Atlanta	                        Bags	                                  4
United States	not available in demo dataset	Bags	                                  50
United States	Detroit	                        Drinkware	                              1
United States	not available in demo dataset	Electronics	                              1
United States	not available in demo dataset	Home/Apparel/Headgear/	                  1
United States	New York	                    Home/Apparel/Men's/	                      1
United States	not available in demo dataset	Home/Apparel/Men's/	                      1
United States	Los Angeles	                    Home/Apparel/Men's/Men's-Outerwear/	      1
United States	Ann Arbor	                    Home/Apparel/Men's/Men's-T-Shirts/	      1
United States	New York	                    Home/Apparel/Men's/Men's-T-Shirts/	      1
United States	Mountain View	                Home/Apparel/Women's/Women's-Outerwear/	  1
United States	Mountain View	                Home/Nest/Nest-USA/	                      1
United States	New York	                    Home/Nest/Nest-USA/	                      2
United States	not available in demo dataset	Home/Nest/Nest-USA/	                      1
United States	Palo Alto	                    Home/Nest/Nest-USA/	                      1
United States	San Francisco	                Home/Nest/Nest-USA/	                      1
United States	San Jose	                    Home/Nest/Nest-USA/	                      1
United States	Seattle	                        Home/Nest/Nest-USA/	                      1
United States	Sunnyvale	                    Home/Nest/Nest-USA/	                      1
United States	not available in demo dataset	Home/Office/Notebooks & Journals/	      65
United States	Dallas	                        Home/Shop by Brand/YouTube/	              1
United States	New York	                    Home/Shop by Brand/YouTube/	              1
United States	Chicago	                        Lifestyle	                              1
United States	Mountain View	                Nest-USA	                              1
United States	not available in demo dataset	Nest-USA	                              1
United States	San Francisco	                Nest-USA	                              1
United States	Sunnyvale	                    Nest-USA	                              1
United States	not available in demo dataset	Office	                                  1

No relevant pattern
_________________________________________________________________________________________________________________________________________________

Question 4: 

What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?


SQL Queries:

 SELECT  
	 	city,
		country, 
		MAX(productquantity) as maxi
	 FROM all_sessions
	 WHERE productquantity NOTNULL 
	 --AND productquantity = (SELECT MAX(maxi) FROM tempo)
	 GROUP BY city,country
	 ORDER BY country

Answer:

City	                        Country	         Max
not available in demo dataset	Argentina	     1
not available in demo dataset	Canada	         1
not available in demo dataset	Colombia	     1
not available in demo dataset	Finland	         1
not available in demo dataset	France	         1
Bengaluru	                    India	         1
Dublin	                        Ireland	         1
not available in demo dataset	Mexico	         1
Madrid	                        Spain	         10
(not set)	                    United States	 1
Ann Arbor	                    United States	 1
Atlanta	                        United States	 4
Chicago	                        United States	 1
Columbus	                    United States	 1
Dallas	                        United States	 1
Detroit	                        United States	 1
Houston	                        United States	 2
Los Angeles	                    United States	 1
Mountain View	                United States	 1
New York	                    United States	 2
not available in demo dataset	United States	 65
Palo Alto	                    United States	 1
Salem	                        United States	 8
San Francisco	                United States	 1
San Jose	                    United States	 1
Seattle	                        United States	 1
Sunnyvale	                    United States	 1

__________________________________________________________________________________________________________________________________________________

Question 5: 

Can we summarize the impact of revenue generated from each city/country?

SQL Queries:

SELECT  
	city,
	country, 
	SUM(totaltransactionrevenue)
FROM all_sessions
WHERE totaltransactionrevenue NOTNULL
GROUP BY city,country
ORDER BY country

Answer:

City	         Country	      Total Revenue
Sydney	         Australia	      358000000
New York	     Canada	          67990000
Toronto	         Canada	          82160000
Tel Aviv-Yafo	 Israel	          602000000
Zurich	         Switzerland	  16990000
Mountain View	 United States	  483360000
San Francisco	 United States	  1564320000
Austin	         United States	  157780000
Houston	         United States	  38980000
San Bruno	     United States	  103770000
Seattle	         United States	  358000000
New York	     United States	  530360000
Nashville	     United States	  157000000
Los Angeles	     United States	  479480000
San Jose	     United States	  262380000
Atlanta	         United States	  854440000
                 United States	  5968560000
Palo Alto	     United States	  608000000
Sunnyvale	     United States	  992230000
Chicago	         United States	  449520000
Columbus	     United States	  21990000






