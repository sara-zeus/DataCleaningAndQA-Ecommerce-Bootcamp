Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

# For cities
```sql
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
```


# Countries 

''' SELECT 
    
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
    '''


Answer:


"Sunnyvale"	"United States"	6722299.92
"Seattle"	"United States"	3580000.00
"San Francisco"	"United States"	3126799.98
"Chicago"	"United States"	3060000.00
"Toronto"	"Canada"	2587200.00
"Mountain View"	"United States"	1599700.00
"Palo Alto"	"United States"	1510000.00
"New York"	"United States"	1399500.00
"Zurich"	"Switzerland"	169900.00

    

"United States"	20998299.90
"Canada"	2587200.00
"Switzerland"	169900.00

 

# In the USA 


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:





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

Answer:

"Sunnyvale"	"United States"	45
"San Francisco"	"United States"	13
"Toronto"	"Canada"	8
"Mountain View"	"United States"	7
"New York"	"United States"	5
"Seattle"	"United States"	3
"Chicago"	"United States"	2
"Palo Alto"	"United States"	1
"Zurich"	"Switzerland"	1


    
    


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:






SELECT  cas.corrected_city AS city, cas.country, p.name AS Name
FROM cleaned_all_sessions AS cas 
JOIN products AS p 
ON cas.productSKU = p.SKU
GROUP BY p.name, cas.corrected_city, cas.country
ORDER BY p.name 



Answer:

The query results reveal insights into the top-selling products in various cities and highlight a pattern in consumer preferences that is influenced by geographical and climatic factors. The selected cities in the United States and Switzerland exhibit distinct consumer behavior, showcasing a tangible connection between the local climate and the type of products purchased.


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




Answer:
This summary provides insights into the cities and countries that contribute the most to the total transaction revenues. It is evident that larger cities in the United States, such as Sunnyvale, Seattle, and San Francisco, are the top revenue generators, followed by other cities in the United States and Canada. 







