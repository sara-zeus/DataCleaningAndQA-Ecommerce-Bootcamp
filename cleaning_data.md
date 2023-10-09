


Queries:
Below, provide the SQL queries you used to clean your data.

#analytics
# The date column's type was initially as an INT instead of date time. I made permanent changes to this type issue 



UPDATE analytics  
SET date= CAST(CAST( date AS char(8)) AS DATE) 
ALTER TABLE analytics MODIFY date DATE; 



# From the Hint, we need to divide the price by 1,000,000** 


SELECT unit_price/1000000 AS formated_price FROM analytics 




  



# Create a Virtual Table 
CREATE VIEW cleaned_analytics AS 
SELECT 
    *,
    ROUND(CAST(unit_price AS DECIMAL) / 1000000, 3) AS unit_price_formatted,
    to_timestamp(visitid::BIGINT) AS formatted_date,
    ROUND(CAST(revenue AS DECIMAL) / 100, 2) AS formatted_revenue, 
    
FROM ANALYTICS 
WHERE units_sold IS NOT NULL AND revenue IS NOT NULL AND city NOT NULL, 


# all_sessions
# Now we need to clean the city table because we find lots of cities tha are called "(Not set)" or "not available in demo dataset" 
SELECT DISTINCT city FROM all_sessions; 

# We correct this  

SELECT * FROM (
    SELECT 
        CASE 
            WHEN city IN ('not available in demo dataset', '(not set)') THEN NULL
            ELSE city 
        END AS corrected_city
    FROM all_sessions
) AS subquery
WHERE corrected_city IS NOT NULL;


# Also we need to filter out where totalrevenue is NOT NULL 

SELECT totaltransactionrevenue FROM all_sessions WHERE totaltransactionrevenue IS NOT NULL; 


# Transactions where NOT NULL 

SELECT transactions FROM all_sessions WHERE transactions IS NOT NULL; 

# Virtual Table: 
CREATE VIEW cleaned_all_sessions AS 
SELECT 
    *,
    CASE 
        WHEN city IN ('not available in demo dataset', '(not set)') THEN NULL 
        ELSE city 
    END AS corrected_city
FROM all_sessions 
WHERE 
    (city IS NOT NULL AND city NOT IN ('not available in demo dataset', '(not set)')) 
    AND totaltransactionrevenue IS NOT NULL 
    AND transactions IS NOT NULL;












