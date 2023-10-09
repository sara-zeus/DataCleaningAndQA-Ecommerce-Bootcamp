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



Question 4: List of top 5 cities with the highest sentiment Scores. 

SQL Queries:

SELECT 
    cas.corrected_city AS city, ROUND(AVG(CAST(p.sentimentscore AS NUMERIC)),2) AS average_sentiment
FROM 
    products AS p
JOIN
    cleaned_all_sessions AS cas
ON
    cas.productsku = p.sku 
GROUP BY
    cas.corrected_city
	ORDER BY ROUND(AVG(CAST(p.sentimentscore AS NUMERIC)),2) DESC
	LIMIT 5; 
	


Answer:

"Atlanta"	0.70
"Columbus"	0.70
"San Jose"	0.65
"Houston"	0.50
"Austin"	0.40



Question 5: Whats the average overall sentiment magnitude from all products? 

SQL Queries:

SELECT 
    ROUND(AVG(CAST(sentimentmagnitude AS NUMERIC)),2) AS average_sentiment_magnitude 
FROM 
    products 


Answer: 0.77
