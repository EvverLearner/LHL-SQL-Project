Question 1: What products are purchased in transactions with the highest money spent in a single session?

SQL Queries:

Answer: 



Question 2: Are there any products that need to be flagged for restocking? Are there any orders that need more supply right away?

SQL Queries:
```SQL
-- Query to check products with empty stock.
SELECT
	DISTINCT(product_sku),
	name,
	total_ordered,
	stock_level,
	restocking_lead_time
FROM
	sales_report
WHERE
	stock_level = 0
	AND
	total_ordered = 0
```
```SQL
-- Query to find products that have been ordered more than what is in stock.
SELECT
	product_sku,
	name,
	total_ordered,
	stock_level,
	restocking_lead_time
FROM
	sales_report
WHERE
	total_ordered > stock_level
```

Answer: *total_ordered can mean ordered by customers, or ordered by the business from suppliers. I am assuming the former for this question*\
Using the first query, there are 78 products that appear with a stock amount of 0, with no orders for resupply placed. Another query also shows 2 items that have been ordered by customers above current stock levels, which is of immediate concern. These items can be flagged appropriately.


Question 3: 

SQL Queries:

Answer:
