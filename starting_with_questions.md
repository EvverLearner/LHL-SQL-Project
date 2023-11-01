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


Answer: Countries with the highest transaction levels, in order:
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


Answer: Average order amount by country, from most to least:
1. Saudi Arabia -> 96
2. Kuwait -> 86
3. Ethiopia, Oman, Laos -> 85

Average order amount by city, from most to least:
1. Riyadh, Brno -> 319
2. Rexburg -> 251
3. Sacramento -> 189

(queries show more, these are just the top 3)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







