Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

SELECT country, city, SUM(trans_rev) AS total_trans_rev
FROM all_sessions
GROUP BY country, city
ORDER BY total_trans_rev DESC;

Answer:

US, not available in demo dataset, 2190.95
US, Sunnyvale, 200

**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:

SELECT country, city, AVG(prod_quant) AS avg_prod_quant 
FROM all_sessions
WHERE prod_quant > 0
GROUP BY country, city
ORDER BY avg_prod_quant DESC

Answer:

US, not available in demo dataset, 10.6
Spain, Madrid, 10
US, Salem, 8

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:

SELECT country, city, AVG(prod_quant) AS avg_prod_quant 
FROM all_sessions
WHERE prod_quant > 0
GROUP BY country, city
ORDER BY avg_prod_quant DESC

Answer:

Many nest-usa in the US
Apparel of all kinds across the board


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT country, city, prod_name, 
MAX(total_trans_rev) OVER (PARTITION BY country, city)
FROM all_sessions
WHERE total_trans_rev > 0
ORDER BY country, city;

SELECT country, city, prod_name, 
MAX(prod_quant) OVER (PARTITION BY country, city)
FROM all_sessions
WHERE prod_quant > 0
ORDER BY country, city;

SELECT country, city, prod_name, 
MAX(prod_rev) OVER (PARTITION BY country, city)
FROM all_sessions
WHERE prod_rev > 0
ORDER BY country, city;

Answer:

The nest line of products and google apparel seem to do very well in the US and internationaly, in both revenue and products sold.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT country, city, 
SUM(total_trans_rev) AS sum_total, 
SUM(prod_rev) AS sum_prod, 
SUM(trans_rev) AS sum_trans
FROM all_sessions
GROUP BY country, city
HAVING SUM(total_trans_rev) > 0 
OR SUM(prod_rev) > 0 
OR SUM(trans_rev) > 0

Answer:

The vast majority of consumers are based in the US. The city with the most total revenue that we can gleen from the dataset is San Francisco.
Various customers are scattered elsewhere in Canada, Australia, Europe, and more.