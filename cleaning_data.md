What issues will you address by cleaning the data?

1 - Filter to remove duplicate row(s)
2 - Set a primary key
3 - Remove NULL columns
4 - Remove useless/unrelevant NOTNULL columns
5 - Reformat NOTNULL colummns if necessary (data type, data lisibility, switch columns and so on)

_____________________________________________________________________________________________________________________________________

Queries:
Below, provide the SQL queries you used to clean your data.

Table "all_sessions"

1 - Filter to remove duplicate row(s)
	
	Step 1 - 
	UPDATE all_sessions
	SET fulluserid = visitid,
          visitid = fulluserid	
	
	Run one by one
	ALTER TABLE all_sessions
   	RENAME fulluserid TO visitid1

	ALTER TABLE all_sessions
   	RENAME visitid TO fullvisitorid1

	ALTER TABLE all_sessions
   	RENAME fulluserid1 TO fullvisitorid

	ALTER TABLE all_sessions
   	RENAME visitid1 TO visitid
	
	Step 2 - 
	CREATE TABLE all_sessions1 (LIKE all_sessions)

	Step 3 -
	WITH temp1 AS (
	SELECT  
		    visitid,
    		channelgrouping,
    		time_ ,
    		country,
    		city ,
    		totaltransactionrevenue,
    		transactions,
    		timeonsite,
    		pageviews,
    		sessionqualitydim,
    		date_,
    		fullvisitorid,
    		type_,
    		productRefundAmount,
    		productquantity,
    		productprice,
    		productrevenue,
    		productsku,
    		v2productname,
    		v2productcategory,
    		productvariant,
    		currencycode,
    		itemQuantity,
    		itemRevenue,
    		transactionrevenue,
    		transactionid,
    		pagetitle,
    		searchkeyword,
    		pagepathlevel1,
    		ecommerceaction_type,
    		ecommerceaction_step,
    		ecommerceaction_option
		ROW_NUMBER() OVER(PARTITION BY
			    visitid,
    			channelgrouping,
    			time_ ,
    			country,
    			city ,
    			totaltransactionrevenue,
    			transactions,
    			timeonsite,
    			pageviews,
    			sessionqualitydim,
    			date_,
    			fullvisitorid,
    			type_,
    			productRefundAmount,
    			productquantity,
    			productprice,
    			productrevenue,
    			productsku,
    			v2productname,
    			v2productcategory,
    			productvariant,
    			currencycode,
    			itemQuantity,
    			itemRevenue,
    			transactionrevenue,
    			transactionid,
    			pagetitle,
    			searchkeyword,
    			pagepathlevel1,
    			ecommerceaction_type,
    			ecommerceaction_step,
    			ecommerceaction_option) AS duplicate
	FROM all_sessions
		)
	INSERT INTO all_session1 (
		visitid,
    		channelgrouping,
    		time_ ,
    		country,
    		city ,
    		totaltransactionrevenue,
    		transactions,
    		timeonsite,
    		pageviews,
    		sessionqualitydim,
    		date_,
    		fullvisitorid,
    		type_,
    		productRefundAmount,
    		productquantity,
    		productprice,
    		productrevenue,
    		productsku,
    		v2productname,
    		v2productcategory,
    		productvariant,
    		currencycode,
    		itemQuantity,
    		itemRevenue,
    		transactionrevenue,
    		transactionid,
    		pagetitle,
    		searchkeyword,
    		pagepathlevel1,
    		ecommerceaction_type,
    		ecommerceaction_step,
    		ecommerceaction_option  )
	SELECT 
		visitid,
    		channelgrouping,
    		time_ ,
    		country,
    		city ,
    		totaltransactionrevenue,
    		transactions,
    		timeonsite,
    		pageviews,
    		sessionqualitydim,
    		date_,
    		fullvisitorid,
    		type_,
    		productRefundAmount,
    		productquantity,
    		productprice,
    		productrevenue,
    		productsku,
    		v2productname,
    		v2productcategory,
    		productvariant,
    		currencycode,
    		itemQuantity,
    		itemRevenue,
    		transactionrevenue,
    		transactionid,
    		pagetitle,
    		searchkeyword,
    		pagepathlevel1,
    		ecommerceaction_type,
    		ecommerceaction_step,
    		ecommerceaction_option
	FROM temp1
	WHERE duplicate = 1

	Step 4 -
	DROP DOWN TABLE all_sessions
	
	Step 5 -
	ALTER TABLE all_sessions1
	RENAME TO all_sessions
	
New Table has 14,556 rows after removing duplicate

2 - Set "visitid" as primary key
	
	Since SELECT COUNT(*) FROM all_sessions = SELECT DISTINCT visitid FROM all_sessions so "visitid" can be set as primary key
	
	Run:

	ALTER TABLE all_sessions
	ADD PRIMARY KEY(visitid)

3 - NULL columns removed:
	productRefundAmount - itemQuantity - itemRevenue - searchKeyWord

4 - Useless/unrelevant NOTNULL columns removed:
      "transactionrevenue" as containing same data than "totaltransactionrevenue"

5 - Reformating done:
	- "productprice" divided by 1,000,000
	- "date_" changed to format Date
	- "time" changed to format Time

________________________________________________________________________________________________________________________________________________


Table "analytics"

1 - Filter to remove duplicate row(s)
	
	Step 1 - 
	UPDATE analytics
	SET visitnumber = visitid,
          visitid = visitnumber	
	
	Run one by one
	ALTER TABLE all_sessions
   	RENAME visitnumber TO visitid1

	ALTER TABLE all_sessions
   	RENAME visitid TO visitnumber1

	ALTER TABLE all_sessions
   	RENAME visitnumber1 TO visitnumber

	ALTER TABLE all_sessions
   	RENAME visitid1 TO visitid
	
	Step 2 - 
	CREATE TABLE analytics1 (LIKE analytics)

	Step 3 -
	WITH temp1 AS (
		SELECT
			    visitid, 
                visitnumber,
	            visitStartTime, 
                Date_,
                fullVisitorId,
                userId,
                channelGrouping,
         		socialEngagementType,
         		units_sold,
         		pageviews,
         		timeOnSite,
         		bounces,
         		revenue,
         		unit_price,  
           		ROW_NUMBER() OVER(PARTITION BY 
							    visitid,
                                visitnumber, 
                                visitStartTime, 
                                Date_,
         						fullVisitorId,
         						userId,
         						channelGrouping,
         						socialEngagementType,
         						units_sold,
         						pageviews,
         						timeOnSite,
         						bounces,
         						revenue,
         						unit_price) AS duplicate
          	FROM analytics
			  )
	INSERT INTO analytics1 (
			visitid, 
                  visitnumber,
	            visitStartTime, 
                  Date_,
                  fullVisitorId,
                  userId,
                  channelGrouping,
         		socialEngagementType,
         		units_sold,
         		pageviews,
         		timeOnSite,
         		bounces,
         		revenue,
         		unit_price )
	SELECT 
			visitid, 
                  visitnumber,
	            visitStartTime, 
                  Date_,
                  fullVisitorId,
                  userId,
                  channelGrouping,
         		socialEngagementType,
         		units_sold,
         		pageviews,
         		timeOnSite,
         		bounces,
         		revenue,
         		unit_price
	FROM temp1
	where duplicate = 1

	Step 4 -
	DROP DOWN TABLE analytics
	
	Step 5 -
	ALTER TABLE analytics1
	RENAME TO analytics
	
New Table has 1,739,308 rows after removing duplicate	

2 - Set "visitid" as primary key

	SELECT COUNT(*) FROM analytics > SELECT DISTINCT visitid FROM analytics so "visitid" is not unique after removing duplicate
	
	SELECT COUNT(*) FROM analytics = 1,739,308 and SELECT DISTINCT visitid FROM analytics = 148,642
	after cleaning "userid" we will lose 91,45% of the data, this table does not seem reliable to be used

	If we do have to use the remaining data we can apply those further cleanings
	
3 - NULL columns removed:
	userid

3 - Useless/unrelevant NOTNULL columns removed: Not Applicable

4 - Reformating done:
	- "unit_price" divided by 1,000,000	
	- "date_" changed to format Date
	- "timeOnSite" changed to format Time
	
__________________________________________________________________________________________________________________________________________

Table "products"

1 - Filter to remove duplicate row(s)

	SELECT COUNT(*) FROM products = SELECT DISTINCT sku FROM products. So no duplicate to remove 

2 - Set "SKU" as primary key

	ALTER TABLE products
	ADD PRIMARY KEY(sku)

3 - NULL columns removed: Not Applicable

4 - Useless/unrelevant NOTNULL columns removed: Not Applicable

5 - Reformating done: Not applicable

___________________________________________________________________________________________________________________________________________

Table "sales_by_SKU"

This table is included in the table sales_report. There is no need to consider it.

___________________________________________________________________________________________________________________________________________

Table "sales_report"

1 - Filter to remove duplicate row(s)

	SELECT COUNT(*) FROM sales_report = SELECT DISTINCT productsku FROM sales_report. So no duplicate to remove

2 - Set "productSKU" as primary key

	ALTER TABLE sales_report
	ADD PRIMARY KEY(productsku)

3 - NULL columns removed: Not Applicable

4 - Useless/unrelevant NOTNULL columns removed:
	"ration" because it can be computed from 2 stored data: "ratio = total_ordered/stockLevel"

5 - Reformating done: Not applicable

