
__Below, provide the SQL queries you used to clean your data.__ 

__analytics__
__The date column's type was initially  an INT instead of datetime. I made permanent changes to this type issue__ 

```sql 


UPDATE analytics  
SET date= CAST(CAST( date AS char(8)) AS DATE) 
ALTER TABLE analytics MODIFY date DATE; 
```



__From the Hint, we need to divide the price by 1,000,000__ 

```sql 

SELECT unit_price/1000000 AS formated_price FROM analytics 

```
 


  



__Create a Virtual Table__ 

```sql 

CREATE VIEW cleaned_analytics AS 
SELECT 
    *,
    ROUND(CAST(unit_price AS DECIMAL) / 1000000, 3) AS unit_price_formatted,
    to_timestamp(visitid::BIGINT) AS formatted_date,
    ROUND(CAST(revenue AS DECIMAL) / 100, 2) AS formatted_revenue, 
    
FROM ANALYTICS 
WHERE units_sold IS NOT NULL AND revenue IS NOT NULL AND city NOT NULL,
```



__all_sessions__ 
__Now we need to clean the city table because we find lots of cities tha are called "(Not set)" or "not available in demo dataset"__ 
```sql 
SELECT DISTINCT city FROM all_sessions;
```


__We correct this:__ 
```sql 


SELECT * FROM (
    SELECT 
        CASE 
            WHEN city IN ('not available in demo dataset', '(not set)') THEN NULL
            ELSE city 
        END AS corrected_city
    FROM all_sessions
) AS subquery
WHERE corrected_city IS NOT NULL;
```



__Also we need to filter out where totalrevenue is NOT NULL:__ 
```sql 

SELECT totaltransactionrevenue FROM all_sessions WHERE totaltransactionrevenue IS NOT NULL;
```



__Filtering out NULL Transactions:__ 
```sql 

SELECT transactions FROM all_sessions WHERE transactions IS NOT NULL;
```


__Filtering out Null revenues:__ 
```sql 

SELECT COUNT(*) AS null_revenue_count
    FROM all_sessions
    WHERE totaltransactionrevenue IS NULL
```


__Virtual Table:__ 
```sql 
 

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

SELECT COUNT(*) AS null_revenue_count
FROM all_sessions
WHERE totaltransactionrevenue IS NULL;





```





