 -----------find all duplicate records
select distinct * from cleaned_analytics

 ---------- find the total number of unique visitors (`fullVisitorID`)
select  count(distinct(fullvisitorid)) from cleaned_analytics

 -------- find the total number of unique visitors by referring sites

select  count(distinct(fullvisitorid)) 
from cleaned_analytics
where channelgrouping = 'Referral'

---------find each unique product viewed by each visitor
select distinct fullvisitorid, productsku
from cleaned_all_sessions
group by fullvisitorid, productsku
order by fullvisitorid


---------compute the percentage of visitors to the site that actually makes a purchase

SELECT (purchasing_visitors*100/total_Visitors) as purchasing_percent
FROM

(select 
	count(distinct fullvisitorid) AS total_Visitors,
	(select 
		count(distinct fullvisitorid) AS purchasing_visitors
	from cleaned_analytics
	where units_sold != 0
	 )
from cleaned_analytics) AS ratio_table
----------------------------------------------------------------------------------

Question 1: countrys with the highest number of visitors

SQL Queries:
SELECT DISTINCT(country), COUNT (*) AS country_count
FROM cleaned_all_sessions
GROUP BY country
ORDER BY country_count DESC
Answer:


Question 2: HOW Many countries do we have most missing demography record in dataset

SQL Queries:
	
select country, count(*)
from cleaned_all_sessions
group by country, city
having city like '%not%'
order by count(*) desc

Answer: United state



Question 3: Days with the most traffic/visit

SQL Queries:
select date, count(*) 
from cleaned_all_sessions
group by date
order by count(*) desc

Answer:



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
