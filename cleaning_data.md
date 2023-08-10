What issues will you address by cleaning the data?

Firstly i needed to check for the unique data in my tables to represent my primary Keys
STEP 1
I tried to identify empty rows for different rows starting from the  Table  "products" and Cloumn "sku"

	SELECT 
		DISTINCT 
		sku AS sku,
		COUNT(*) AS row_count 
	FROM products
	GROUP BY sku
	HAVING COUNT(*) > 1
	ORDER BY COUNT(*) DESC

No column was returned which means that the 'SKU' column passes as a primary key for the 'Product' Table 


STEP 2

I ran the query in STEP 1 for other tables and  was able to get the primary KEY as below
Primary KEY for products is sku
Primary KEY for sales_by_sku is productsku
Primary KEY for sales_report is productsku


Step 4
I noticed some repetition of data accross these 3 tables and decided to optimize. The eventually made me to take out the sales_report table since all its colums can be found in other tables 

-total_ordered: data present in the table sales_by_sku
-name: data present in the table 'product'
-stockLevel: data present in the table 'product'
-restockingLeadTime: data present in the table 'product'
-sentimentScore: data present in the table 'product'
-sentimentMagnitude: data present in the table 'product'
-ratio:Removed column because it can be calculated from "total_ordered/stockLevel"

STEP 5 
Search for Primary Key in the 'analytics' and 'all_sessions' table and i figured that the visitid and fullvisitorid together will make a good primary key



Queries:
Below, provide the SQL queries you used to clean your data.

-----to check if column is empty ----------------------------------------------
WITH analysis_rows_number AS (
SELECT 
	COUNT(fullVisitorId) AS fullVisitorId,
FROM all_sessions


----SALES_REPORT TABLE-----------------------------------------------------------
i did not make use of this table because it is a redundant table. All its data can be found in other tables

---ANALYTICS TABLE------------------------------------------------------------------
CREATE TABLE cleaned_analytics AS (
	SELECT DISTINCT 
		visitid,
		visitNumber,
		visitStartTime,
		date,
		fullvisitorId,
		--userid, --number of column is zero
		channelGrouping,
		--socialEngagementType, -- no data
		case
			when units_sold is null then 0
			else units_sold 
		END AS units_sold,
		pageviews,
		timeonsite,
		--bounces, -- no data
		case
			when units_sold is null then 0
			else units_sold*(unit_price/1000000) 
		END AS revenue,
		unit_price/1000000 as unit_price
	FROM analytics
)
---ALL_SESSION TABLE----------------------------------------------------------------
CREATE TABLE cleaned_all_sessions AS(
SELECT 
	DISTINCT(fullVisitorId) AS fullVisitorId,
	channelGrouping AS channelGrouping,
	time AS time,
	country AS country,
	CASE
		WHEN city LIKE 'not%' or city LIKE '_not%' THEN country
		ELSE city	
	END AS city, 
	totalTransactionRevenue/1000000 AS totalTransactionRevenue ,
	transactions AS transactions, 
	--timeOnSite AS timeOnSite,--available in analytics table
	--pageviews AS pageviews, --available in analytics table
	sessionQualityDim AS sessionQualityDim,
	date AS date,
	visitId AS visitId,
	type AS type,
	--productRefundAmount AS productRefundAmount, -- count is zero
	productQuantity AS productQuantity,
	productPrice/1000000 AS productPrice,
	--productRevenue AS productRevenue, -- can be calculated by productprice * productQuantity
	productSKU AS productSKU,
	v2ProductName AS v2ProductName,
	v2ProductCategory AS v2ProductCategory,
	productVariant AS productVariant,
	currencyCode  AS currencyCode,
	--itemQuantity AS itemQuantity ,  -- count is zero
	--itemRevenue AS itemRevenue,  -- count is zero
	--transactionRevenue AS transactionRevenue, -- repetition of total transaction revenue 
	transactionId AS transactionId,
	pageTitle AS pageTitle,
	--searchKeyword AS searchKeyword,  -- count is zero
	pagePathLevel1 AS pagePathLevel1,
	eCommerceAction_type AS eCommerceAction_type,
	eCommerceAction_step AS eCommerceAction_step,
	eCommerceAction_option AS eCommerceAction_option
FROM all_sessions 
)

--PRODUCT TABLE-------------------------------------------------------------------------------------
CREATE TABLE cleaned_products AS (
	SELECT DISTINCT *
	FROM products
	WHERE orderedquantity !=0 AND stocklevel != 0
)


--sales_by_sku TABLE
All data is distinct
