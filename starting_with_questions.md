Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

(SELECT 
	--cp.sku AS productsku,
	cas.city , 
	cas.country,
	SUM(cp.orderedquantity* cas.productprice) AS  revenue,
	'city with highest revenue' AS revenue_comment
FROM cleaned_all_sessions AS cas
JOIN cleaned_products AS cp ON cas.productsku = cp.sku
GROUP BY cas.city, cas.country
 ORDER BY cas.city 
 DESC LIMIT 1)

UNION ALL

(SELECT 
	--cp.sku AS productsku,
	cas.city , 
	cas.country,
	SUM(cp.orderedquantity* cas.productprice) AS  revenue,
	'country with higest revenue' AS revenue_comment
	
FROM cleaned_all_sessions AS cas
JOIN cleaned_products AS cp ON cas.productsku = cp.sku
GROUP BY cas.country, cas.city
 ORDER BY cas.country 
 DESC LIMIT 1)

Answer:
city with the hightest number of revenue is unknown in the dataset
country with the highest number of revenue is Switzerland



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

  SELECT 
  	city,
	country,
	AVG(orderedquantity) AS average_ordered_quantity

	FROM cleaned_all_sessions cas
	JOIN cleaned_products cp ON cas.productsku = cp.sku

GROUP BY city, country
 order by city desc

Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
select country, v2productcategory, count (v2productcategory)
from cleaned_all_sessions
group by v2productcategory,country
order by v2productcategory, country desc


Answer:
different country have specific product categories which are most ordered





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

select cas.city,  cp.name, count (cp.name)
from cleaned_all_sessions as cas
LEFT JOIN cleaned_products as cp
ON cas.productsku = cp.sku
group by city,cp.name
order by city, count (cp.name) desc

Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

	SELECT 
		cas.country,
		SUM(cp.orderedquantity * cas.productprice) AS Totaltransactionrevenue
	
	
	FROM cleaned_all_sessions AS cas
	JOIN cleaned_products AS cp 
	ON cas.productsku = cp.sku
	
GROUP BY country
ORDER By Totaltransactionrevenue DESC, country

Answer:







