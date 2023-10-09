What are your risk areas? Identify and describe them.


Risk Areas:

Invalid Dates in 'analytics' Table: Risk of incorrect or missing date values that may impact date-based analysis.
Invalid Price Formatting in 'analytics' Table: Risk of NULL values in the 'unit_price_formatted' column, affecting price-related calculations.
'cleaned_analytics' View Creation: Risk that the view was not created correctly, potentially leading to incomplete data.
NULL Values in 'city' Column in 'all_sessions' Table: Risk of missing or inaccurate location data due to NULL city values.
Unresolved 'City' Values in 'cleaned_all_sessions' View: Risk that certain city values may remain unresolved, impacting geographic analysis.
NULL Values in 'totaltransactionrevenue' Column in 'all_sessions' Table: Risk of inaccurate revenue-based analysis due to NULL revenue values.
NULL Values in 'transactions' Column in 'all_sessions' Table: Risk of affecting transaction-related analysis and metrics due to NULL transaction values.

QA Process:
Describe your QA process and include the SQL queries used to execute it.
 
Data Validation in 'analytics' Table: Check for invalid dates and NULL price formats.
Validation of 'cleaned_analytics' View: Ensure successful creation of the 'cleaned_analytics' view.
Data Validation in 'all_sessions' Table: Verify NULL city, revenue, and transaction values.
Validation of 'cleaned_all_sessions' View: Confirm successful creation of the 'cleaned_all_sessions' view.
Summary of Data Quality Issues: Provide a summary of identified data quality problems.
Proposed Data Cleaning Actions: Recommend specific actions to address the identified data quality issues.






-- Data Quality Assurance Query

-- Check if the 'analytics' table has been updated correctly to date type
SELECT COUNT(*) AS invalid_date_count
FROM analytics
WHERE DATE(date) IS NULL;

-- Check if the unit_price formatting has been applied correctly
SELECT COUNT(*) AS invalid_price_format_count
FROM analytics
WHERE unit_price_formatted IS NULL;

-- Check if the 'cleaned_analytics' view has been created correctly
SELECT COUNT(*) AS cleaned_analytics_view_count
FROM information_schema.views
WHERE table_name = 'cleaned_analytics';

-- Check for NULL values in 'all_sessions' city column
SELECT COUNT(*) AS null_city_count
FROM all_sessions
WHERE city IS NULL;

-- Check if 'not available in demo dataset' or '(not set)' values in city column have been corrected in 'cleaned_all_sessions' view
SELECT COUNT(*) AS corrected_city_count
FROM cleaned_all_sessions
WHERE corrected_city IN ('not available in demo dataset', '(not set)');

-- Check for NULL values in 'totaltransactionrevenue' column in 'all_sessions'
SELECT COUNT(*) AS null_revenue_count
FROM all_sessions
WHERE totaltransactionrevenue IS NULL;

-- Check for NULL values in 'transactions' column in 'all_sessions'
SELECT COUNT(*) AS null_transactions_count
FROM all_sessions
WHERE transactions IS NULL;

-- Check if the 'cleaned_all_sessions' view has been created correctly
SELECT COUNT(*) AS cleaned_all_sessions_view_count
FROM information_schema.views
WHERE table_name = 'cleaned_all_sessions';


SELECT
    (SELECT COUNT(*) FROM analytics) AS total_records_in_analytics,
    (SELECT COUNT(*) FROM all_sessions) AS total_records_in_all_sessions,
    (SELECT COUNT(*) FROM cleaned_analytics) AS total_records_in_cleaned_analytics,
    (SELECT COUNT(*) FROM cleaned_all_sessions) AS total_records_in_cleaned_all_sessions,
    invalid_date_count AS invalid_dates_found,
    invalid_price_format_count AS invalid_price_formats_found,
    null_city_count AS NULL_cities_found,
    corrected_city_count AS corrected_cities_found,
    null_revenue_count AS NULL_revenues_found,
    null_transactions_count AS NULL_transactions_found
FROM (
    SELECT COUNT(*) AS invalid_date_count
    FROM analytics
    WHERE DATE(date) IS NULL
) AS date_issue,
(
    SELECT COUNT(*) AS invalid_price_format_count
    FROM cleaned_analytics
    WHERE unit_price_formatted IS NULL
) AS price_format_issue,
(
    SELECT COUNT(*) AS null_city_count
    FROM all_sessions
    WHERE city IS NULL
) AS null_city_issue,
(
    SELECT COUNT(*) AS corrected_city_count
    FROM cleaned_all_sessions
    WHERE corrected_city IN ('not available in demo dataset', '(not set)')
) AS corrected_city_issue,
(
    SELECT COUNT(*) AS null_revenue_count
    FROM all_sessions
    WHERE totaltransactionrevenue IS NULL
) AS null_revenue_issue,
(
    SELECT COUNT(*) AS null_transactions_count
    FROM all_sessions
    WHERE transactions IS NULL
) AS null_transactions_issue;


