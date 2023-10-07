Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:



Answer:
# For cities

SELECT 

	cas.corrected_city AS City,     
	cas.country AS Country, 
    SUM(ca.formatted_revenue) AS TotalTransactionRevenues
FROM 
    cleaned_all_sessions AS cas
JOIN 
    cleaned_analytics AS ca ON ca.fullvisitorId = cas.fullVisitorId 
GROUP BY 
    cas.country, cas.corrected_city 
ORDER BY 
    TotalTransactionRevenues DESC 

    

# Countries 

SELECT 
    
    cas.country AS Country, 
    SUM(ca.formatted_revenue) AS TotalTransactionRevenues
FROM 
    cleaned_all_sessions AS cas
JOIN 
    cleaned_analytics AS ca ON ca.fullvisitorId = cas.fullVisitorId 
GROUP BY 
    cas.country
ORDER BY 
    TotalTransactionRevenues DESC 


 

# In the USA 


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:

SELECT 

	cas.corrected_city AS City,     
	cas.country AS Country, 
SUM(CAST(ca.units_sold AS INT)) AS average_sold
FROM 
    cleaned_all_sessions AS cas
JOIN 
    cleaned_analytics AS ca ON ca.fullvisitorId = cas.fullVisitorId 
GROUP BY 
    cas.country, cas.corrected_city 
ORDER BY 
    average_sold DESC 


    
    


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:


SELECT  cas.corrected_city AS city, cas.country, p.name AS Name
FROM cleaned_all_sessions AS cas 
JOIN products AS p 
ON cas.productSKU = p.SKU
GROUP BY p.name, cas.corrected_city, cas.country
ORDER BY p.name 









**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:



# In the colder cities like SF, I see lots of orders like Hoodies and Neck Tee, but in LA, a hotter place, short sleeves and shors 

SELECT  cas.corrected_city AS city, cas.country, p.name AS Name
FROM cleaned_all_sessions AS cas 
JOIN products AS p 
ON cas.productSKU = p.SKU
GROUP BY p.name, cas.corrected_city, cas.country
HAVING cas.corrected_city IN ('Zurich')
ORDER BY p.name  

# Here we can see in Swizeerland, a colder place the order ia :  Men's 3/4 Sleeve Henley 
# In LA:   Women's Short Sleeve Hero Tee White 

SELECT  cas.corrected_city AS city, cas.country, p.name AS Name
FROM cleaned_all_sessions AS cas 
JOIN products AS p 
ON cas.productSKU = p.SKU
GROUP BY p.name, cas.corrected_city, cas.country
HAVING cas.corrected_city IN ('LA')
ORDER BY p.name 




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:

SELECT 

	cas.corrected_city AS City,     
	cas.country AS Country, 
    SUM(ca.formatted_revenue) AS TotalTransactionRevenues
FROM 
    cleaned_all_sessions AS cas
JOIN 
    cleaned_analytics AS ca ON ca.fullvisitorId = cas.fullVisitorId 
GROUP BY 
    cas.country, cas.corrected_city 
ORDER BY 
    TotalTransactionRevenues DESC 









