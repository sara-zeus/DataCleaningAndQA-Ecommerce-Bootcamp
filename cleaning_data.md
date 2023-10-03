What issues will you address by cleaning the data?





Queries:
Below, provide the SQL queries you used to clean your data.


# The date column's type was initially as an INT instead of date time
# I made permanent changes to this type issue 

UPDATE analytics  
SET date= CAST(CAST( date AS char(8)) AS DATE) 
ALTER TABLE analytics MODIFY date DATE; 



**From the Hint, we need to divide the price by 1,000,000** 
SELECT unit_price/1000000 AS formated_price FROM analytics 


