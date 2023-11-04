Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```SQL
-- Query for highest level of transaction by country
SELECT
	country,
	SUM(total_transaction_revenue) AS total_revenue -- Sum of all transactions from a country
FROM 
	all_sessions
WHERE -- Only want countries with valid transactions
	total_transaction_revenue IS NOT NULL
	AND
	country IS NOT NULL
GROUP BY
	country
ORDER BY
	total_revenue DESC -- Viewing highest revenues
```
```SQL
-- Query for highest level of transaction by city
SELECT
	city,
	SUM(total_transaction_revenue) AS total_revenue -- Sum of all transactions from a city
FROM 
	all_sessions
WHERE -- Only want cities with valid transactions
	total_transaction_revenue IS NOT NULL
	AND
	city IS NOT NULL
GROUP BY
	city
ORDER BY
	total_revenue DESC -- Viewing highest revenues
```


Answer: 
Countries with the highest transaction levels, in order:
1. United States
2. Israel
3. Australia

Cities with the highest transaction levels, in order:
1. San Francisco
2. Sunnyvale
3. Atlanta




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries: 
```SQL
-- Query for average order amount by country
SELECT 
	country,
	ROUND(AVG(total_ordered)) AS avg_ordered -- Rounding value since there are no portion products
FROM 
	all_sessions
JOIN
	sales_report ON -- The sales_report table includes total order values
	all_sessions.product_sku = sales_report.product_sku
GROUP BY
	country
ORDER BY 
	avg_ordered DESC -- Viewing highest average orders
```
```SQL
-- Query for average order amount by city
SELECT 
	city,
	ROUND(AVG(total_ordered)) AS avg_ordered -- Rounding value since there are no portion products
FROM 
	all_sessions
JOIN
	sales_report ON -- The sales_report table includes total order values
	all_sessions.product_sku = sales_report.product_sku
GROUP BY
	city
ORDER BY 
	avg_ordered DESC -- Viewing highest average orders
```


Answer: 
Average order amount by country, from most to least:
1. Saudi Arabia -> 96
2. Kuwait -> 86
3. Ethiopia, Oman, Laos -> 85

Average order amount by city, from most to least:
1. Riyadh, Brno -> 319
2. Rexburg -> 251
3. Sacramento -> 189





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```SQL
-- Query for types of products ordered by country

-- First, grouping the categories of purchased products to each country
WITH category_per_country AS (
	SELECT 
		DISTINCT(country),
		v2_product_category AS prod_category
	FROM 
		all_sessions 
	WHERE 
		country IS NOT NULL 
		AND 
		v2_product_category IS NOT NULL
	ORDER BY
		country
)

-- Second, filtering to find patterns. This example is looking for a string match in countries.
SELECT
	country,
	prod_category
FROM
	category_per_country
WHERE
	country = 'Saudi Arabia'
```
```SQL
-- Query for types of products ordered by city

-- First, grouping the categories of purchased products to each city
WITH category_per_city AS (
	SELECT 
		DISTINCT(city),
		v2_product_category AS prod_category
	FROM 
		all_sessions 
	WHERE 
		city IS NOT NULL 
		AND 
		v2_product_category IS NOT NULL
	ORDER BY
		city
)

-- Second, filtering to find patterns. This example is looking for a string match in category of product.
SELECT
	city,
	prod_category
FROM
	category_per_city
WHERE
	prod_category LIKE '%Lifestyle%'
```


Answer:
A pattern found in Saudi Arabia's orders is that they involve many purchases in 'Apparel' categories, especially T-Shirts. They also have purchases from Android, Google, and YouTube branded products. Combing this with the previous observation in Saudi Arabia's high average order amount, we can potentially glean business growth opportunities.

A pattern observed in city sales regarding category of products, is that 'Lifestyle' products are not very popular. A query for this category only returns 35 rows of data, which is drastically less than all other categories offered on the site ('Bags' = 161 rows, 'Apparel' = 616 rows, 'Drinkware' = 147 rows, etc.). Perhaps a re-evaluation of these products is in order, and whether they are worth keeping.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL
-- Query for top-selling product by country

-- First, group different amounts of products sold in a transaction, per country
WITH sales_per_country AS (	
	SELECT
		country,
		total_ordered
	FROM 
		all_sessions 
	JOIN 
		sales_report ON 
		sales_report.product_sku = all_sessions.product_sku
	GROUP BY
		country, 
		total_ordered
	ORDER BY -- Viewing alphabetically, each country's different order amounts
		country
),

-- Second, filter for the highest order amount, per country
highest_sale_per_country AS (
	SELECT 
		country, 
		MAX(total_ordered) AS highest_order_amnt
	FROM
		sales_per_country
	GROUP BY
		country
)

-- Lastly, join the highest order amount of each country to sales_report to get the name of the product ordered
SELECT
	country,
	name,
	highest_order_amnt
FROM
	highest_sale_per_country
JOIN
	sales_report ON
	sales_report.total_ordered = highest_sale_per_country.highest_order_amnt
WHERE
	highest_order_amnt != 0
	AND
	country IS NOT NULL
GROUP BY
	country,
	name,
	highest_order_amnt
ORDER BY
	highest_order_amnt DESC
```
```SQL
-- Query for top-selling product by city

-- First, group different amounts of products sold in a transaction, per city
WITH sales_per_city AS (	
	SELECT
		city,
		total_ordered
	FROM 
		all_sessions 
	JOIN 
		sales_report ON 
		sales_report.product_sku = all_sessions.product_sku
	GROUP BY
		city, 
		total_ordered
	ORDER BY -- Viewing alphabetically, each city's different order amounts
		city
),

-- Second, filter for the highest order amount, per city
highest_sale_per_city AS (
	SELECT 
		city, 
		MAX(total_ordered) AS highest_order_amnt
	FROM
		sales_per_city
	GROUP BY
		city
)

-- Lastly, join the highest order amount of each city to sales_report to get the name of the product ordered
SELECT
	city,
	name,
	highest_order_amnt
FROM
	highest_sale_per_city
JOIN
	sales_report ON
	sales_report.total_ordered = highest_sale_per_city.highest_order_amnt
WHERE
	highest_order_amnt != 0
	AND
	city IS NOT NULL
GROUP BY
	city,
	name,
	highest_order_amnt
ORDER BY
	highest_order_amnt DESC
```



Answer: *There is missing data for each transaction's specific amount of product sold; these answers are based on the data from total orders for each product and who bought those products, which may not be accurate*\
Top-selling product by country:
1. "Ballpoint LED Light Pen" -> 12 countries
2. "17oz Stainless Steel Sport Bottle" -> 7 countries
3. "Leatherette Journal" -> 6 countries

Top-selling product by city:
1. "Ballpoint LED Light Pen" -> 12 cities
2. "17oz Stainless Steel Sport Bottle" -> 7 cities
3. "Leatherette Journal" -> 9 countries

A pattern we see in the top-selling products is that they are mostly stationary/office supplies, which can be useful information if we want to capitalize on this category of product to increase sales or have more targeted marketing. We also see sports bottles as the second highest selling product, and other bottle accessories appear in the top 10 products; this can be another potential area of business growth. Analysis of cost per product can give more insight.






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
-- Query for total revenue by country
SELECT
	country, 
	SUM(total_transaction_revenue) AS total_rev
FROM 
	all_sessions 
WHERE 
	total_transaction_revenue IS NOT NULL
GROUP BY
	country
ORDER BY 
	total_rev DESC
```
```SQL
-- Query for total revenue by city
SELECT
	city,
	SUM(total_transaction_revenue) AS total_rev
FROM 
	all_sessions 
WHERE
	city IS NOT NULL
	AND
	total_transaction_revenue IS NOT NULL
GROUP BY
	city
ORDER BY
	total_rev DESC
```



Answer: *A lot of missing revenue data*\
To summarize revenue by country, by far the biggest customer base is the United States. Generating $13,154.17 in sales, that is 92.1% of recorded revenue by country from the United States alone, which means trends for products purchased by customers in that region should have more weight in decision making. Summarizing revenue by city is more nuanced; San Francisco generated the most recorded revenue at $1,564.32, which is 19.1% of revenue by city. Perhaps observing trends in cities in the United States can lead to information that will maximize profits.







