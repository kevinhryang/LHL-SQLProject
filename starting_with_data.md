Question 1: find the total number of unique visitors (`fullVisitorID`)

SQL Queries:

SELECT DISTINCT visitor_id FROM all_sessions

Answer: 14223



Question 2: find the number of visitors that viewed more than one product

SQL Queries: 

SELECT visitor_id, COUNT(sku) 
FROM all_sessions 
GROUP BY visitor_id 
HAVING COUNT(sku) > 1

Answer: 794



Question 3: compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:

SELECT 100 * (
	SELECT COUNT(*) FROM all_sessions WHERE total_trans_rev > 0
)::numeric / (
	SELECT COUNT(*) FROM all_sessions
)::numeric

Answer: 0.535%



Question 4: How many products share a name?

SQL Queries: 

SELECT * FROM (
	SELECT name, COUNT(*) AS dups 
	FROM products 
	GROUP BY name
) AS dup_count
WHERE dups > 1

Answer: 240



Question 5: How many products have orders equal to or greater than the quantity in stock?

SQL Queries:

SELECT * FROM sales_report WHERE ratio >= 1

Answer: 6
