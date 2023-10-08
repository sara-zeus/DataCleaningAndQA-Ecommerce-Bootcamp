Question 1: How many duplicated vsitorid do we have? 

SQL Queries:

SELECT fullvisitorid, COUNT(*) as count
FROM cleaned_all_sessions
GROUP BY fullvisitorid
HAVING COUNT(*) > 1;


Answer: 
NONE! 



Question 2: Calculating conversion rate by product: 

SQL Queries:

  SELECT 
    cas.corrected_city AS city, 
    (SUM(CAST(pp.orderedquantity AS INT)) / NULLIF(COUNT(DISTINCT cas.fullvisitorid), 0)) * 100 AS conversion_rate
FROM 
    products pp
JOIN 
    cleaned_all_sessions cas ON cas.productsku = pp.sku
GROUP BY 
    cas.corrected_city
ORDER BY 
    conversion_rate DESC, cas.corrected_city DESC;


Answer:




Question 3: Hiighest selling product how many of it has been sold? 

SQL Queries:

SELECT 
    ABS(CAST((CAST(stocklevel AS INT) - CAST(orderedquantity AS INT)) AS INT)) AS sold 
FROM 
    products
	ORDER BY sold  DESC
	LIMIT 1;



Answer:
14447 



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
