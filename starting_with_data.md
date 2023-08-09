Question 1: countrys with the highest number of visitors

SQL Queries:
SELECT DISTINCT(country), COUNT (*) AS country_count
FROM cleaned_all_sessions
GROUP BY country
ORDER BY country_count DESC
Answer:


Question 2: HOW Many countries do we most missing demography record

SQL Queries:
	
select country, count(*)
from cleaned_all_sessions
group by country, city
having city like '%not%'
order by count(*) desc

Answer: United state



Question 3: Days with the most visit

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
