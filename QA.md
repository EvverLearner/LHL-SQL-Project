What are your risk areas? Identify and describe them.
1. One of the risks in the data integrity is unnecessary data redundancy, since it can potentially lead to miscalculations (ex. calculating a sum of revenue with duplicates will overrepresent sales).
2. Another risk is missing data, since analysis made with data missing cannot be entirely credible or conclusive.
3. Incorrect datatypes is a problem since some data cannot be manipulated properly if it is the wrong type, and results will also not make sense if that data is used.
4. Consistency acrosss the database is also important, for readability and to prevent accidentally missing redundant data.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
1. All naming conventions will follow `snake_case` for a standard in the database, and also since `postgres` is case insensitive.
2. Datatypes are adjusted during the cleaning process or as soon as there is a clear issue.
```SQL
-- An alteration of an attribute from an 'int' to 'money' type.
ALTER TABLE all_sessions
ALTER COLUMN unit_price TYPE money USING unit_price::money
```
3. `NULL` will be the standard for values that are empty or 'useless' data; some recorded info is effectively useless, but not technically `NULL` which can cause issues.
```SQL
-- An alteration for consistency of void values, setting them to NULL.
UPDATE all_sessions
SET
	country = NULL
WHERE
	country = '(not set)'
```
4. Check for missing values and data that is unnecessarily redundant, such as when not being used as a key.
```SQL
-- Query to compare the same data in two tables.
SELECT 
	* 
FROM 
	sales_by_sku
LEFT JOIN -- sales_by_sku has more rows of product_sku
	sales_report ON 
	sales_report.product_sku = sales_by_sku.product_sku
```
This query returns sales_by_sku showing more product_sku rows, in fact 2 rows even have an amount ordered but there is no additonal data to connect it to, which is a QA concern that would require consulting the data source, or at least making a note of it in the analysis.\
5. Avoiding data that is not relevant to the scope of the project, i.e. not helpful in data analysis.
```SQL
-- Query checking if transactions column has any value.
SELECT 
	COUNT(DISTINCT(transactions)) 
FROM 
	all_sessions 
WHERE 
	total_transaction_revenue IS NOT NULL
-- This returns a count of 1, indicating no unique data for pattern analysis.
```
