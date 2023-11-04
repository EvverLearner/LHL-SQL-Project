Question 1: What products are purchased in transactions with the highest money spent in a single session?

SQL Queries:
```SQL
-- Query to find revenue per visit.

-- First, find revenue made on each visit to the site.
WITH revenue_per_visit AS (
	SELECT 
		visit_id,
		SUM(revenue) as revenue
	FROM 
		analytics
	WHERE
		revenue IS NOT NULL
	GROUP BY
		visit_id
	ORDER BY
		revenue DESC
)

-- Second, join to all_session to get product name.
SELECT
	revenue_per_visit.visit_id,
	product_sku,
	v2_product_name as product_name,
	revenue
FROM
	revenue_per_visit
LEFT JOIN -- Checking if all_sessions has missing data on visits.
	all_sessions ON
	all_sessions.visit_id = revenue_per_visit.visit_id
WHERE
	v2_product_name IS NOT NULL
ORDER BY
	revenue DESC
```

Answer: 
Products that have sold for the most in a single visit to the site:
1. "Google Alpine Style Backpack" -> $1,002.78
2. "SPF-15 Slim & Slender Lip Balm" -> $649.16
3. "Nest® Learning Thermostat 3rd Gen-USA - White" -> $396.00



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
