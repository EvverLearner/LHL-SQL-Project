What issues will you address by cleaning the data?

**General**
1. Checking if the Primary Key (PK) for each table is suitable. This can be checked by comparing the amount of returned rows on a `SELECT *` to the amount returned looking for `DISTINCT` values in the PK; the same amount means that every row in that column is distinct, ergo it can be a possible PK.
2. Providing a PK for a table that has no suitable column. I achieved this by adding an auto-incrementing `id` column to such tables.

**all_sessions**
1. `country` column has some values that are `(not set)`, which I will set to `NULL`.
2. `city` column has many values that are `(not set)` or `not available in demo dataset`, which I will set to `NULL`.
3. `total_transaction_revenue` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
4. `unit_price` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
5. `product_revenue` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
6. `v2_product_category` column has some values that are `(not set)`, which I will set to `NULL`.
7. `product_variant` column has most values that are `(not set)`, which I will set to `NULL`.
8. `transaction_revenue` column has a few values that do not make sense, so we will adjust them by dividing them by 1 000 000.
   



Queries:
Below, provide the SQL queries you used to clean your data.

**all_sessions**

```SQL
-- Confirms that 'id' is a suitable PK for all_sessions table.
SELECT id FROM all_sessions

SELECT DISTINCT id FROM all_sessions
```
```SQL
-- Remove instances of '(not set)'.
UPDATE all_sessions
SET
	country = NULL
WHERE
	country = '(not set)'
```
```SQL
-- Remove instances of '(not set)' and 'not available in demo dataset'.
UPDATE all_sessions
SET
	city = NULL
WHERE
	city = '(not set)' 
	OR 
	city = 'not available in demo dataset'
```
```SQL
-- Make total transaction revenue data more sensible.
UPDATE all_sessions
SET
	total_transaction_revenue = total_transaction_revenue / 1000000
```
```SQL
-- Make unit price data more sensible.
UPDATE all_sessions
SET
	unit_price = unit_price / 1000000
```
```SQL
-- Make product revenue data more sensible.
UPDATE all_sessions
SET
	product_revenue = product_revenue / 1000000
```
```SQL
-- Remove instances of '(not set)'.
UPDATE all_sessions
SET
	v2_product_category = NULL
WHERE
	v2_product_category = '(not set)'
```
```SQL
-- Remove instances of '(not set)'.
UPDATE all_sessions
SET
	product_variant = NULL
WHERE
	product_variant = '(not set)'
```
```SQL
-- Make transaction revenue data more sensible.
UPDATE all_sessions
SET
	transaction_revenue = transaction_revenue / 1000000
```
