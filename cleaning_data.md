What issues will you address by cleaning the data?

**General**
1. Column names have been standardized to `snake_case` across the database.
2. Checking if the Primary Key (PK) for each table is suitable. This can be checked by comparing the amount of returned rows on a `SELECT *` to the amount returned looking for `DISTINCT` values in the PK; the same amount means that every row in that column is distinct, ergo it can be a possible PK.
3. Providing a PK for a table that has no suitable column. I achieved this by adding an auto-incrementing `id` column to such tables.

**all_sessions**
1. `country` column has some values that are `(not set)`, which I will set to `NULL`.
2. `city` column has many values that are `(not set)` or `not available in demo dataset`, which I will set to `NULL`.
3. `total_transaction_revenue` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
4. `unit_price` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
5. `product_revenue` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
6. `v2_product_category` column has some values that are `(not set)`, which I will set to `NULL`.
7. `product_variant` column has most values that are `(not set)`, which I will set to `NULL`.
8. `transaction_revenue` column has a few values that do not make sense, so we will adjust them by dividing them by 1 000 000.
9. After adjustments, `transaction_revenue` is found to be a column with entirely redundant data to the `total_transaction_revenue` column. Thus, we will remove it; we will include it with the columns in `step 10`.
10. `product_refund_amount`, `item_quantity`, `item_revenue`, `search_keyword` are all empty columns, so we will remove them for general efficiency.

**analytics**
1. Created an autoincrementing `id` column as a PK using pgAdmin GUI during import of cvs files.
2. `revenue` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000.
3. `unit_price` column has values that do not make sense, so we will adjust them by dividing them by 1 000 000
4. `user_id` is an empty column, so we will remove it for general efficiency. Additionally, the `social_engagement_type` and `bounces` columns consist of the same value for every row respectively, so for the purposes of this analysis we weill remove them as well.

**products**
1. `name` table has some string values that have unnecessary whitespace leading and/or trailing them that needs to be removed.

**sales_report**
1. `name` table has some string values that have unnecessary whitespace leading and/or trailing them that needs to be removed.




Queries:
Below, provide the SQL queries you used to clean your data.

**all_sessions**

```SQL
-- Confirm that 'id' is a suitable PK for all_sessions table.
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
```SQL
-- Checking to see if the transaction_revenue column is redundant.
-- We also check for different values to make sure there is no unique data stored in it.
SELECT 
	total_transaction_revenue, 
	transaction_revenue
FROM 
	all_sessions
WHERE 
	total_transaction_revenue = transaction_revenue

SELECT 
	total_transaction_revenue, 
	transaction_revenue
FROM 
	all_sessions
WHERE 
	total_transaction_revenue != transaction_revenue
```
```SQL
-- Remove empty columns
ALTER TABLE all_sessions
DROP COLUMN product_refund_amount,
DROP COLUMN item_quantity,
DROP COLUMN item_revenue,
DROP COLUMN search_keyword
```

**analytics**
```SQL
-- Make revenue data more sensible.
UPDATE analytics
SET
	revenue = revenue / 1000000
```
```SQL
-- Make unit price data more sensible.
UPDATE analytics
SET
	unit_price = unit_price / 1000000
```
```SQL
-- Remove empty columns
ALTER TABLE analytics
DROP COLUMN user_id,
DROP COLUMN social_engagement_type,
DROP COLUMN bounces
```

**products**
```SQL
-- Confirm that product_sku is a suitable PK for products table.
SELECT product_sku FROM products

SELECT DISTINCT(product_sku) FROM products
```
```SQL
-- Remove whitespace around string data.
UPDATE products
SET
	name = trim(name)
```

**sales_by_sku**
```SQL
-- Confirm that product_sku is a suitable PK for sales_by_sku table.
SELECT product_sku FROM sales_by_sku

SELECT DISTINCT(product_sku) FROM sales_by_sku
```

**sales_report**
```SQL
-- Confirm that product_sku is a suitable PK for sales_report table.
SELECT product_sku FROM sales_report

SELECT DISTINCT(product_sku) FROM sales_report
```
```SQL
-- Remove whitespace around string data.
UPDATE sales_report
SET
	name = trim(name)
```
