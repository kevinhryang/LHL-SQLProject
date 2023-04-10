What issues will you address by cleaning the data?

Across all datasets
- Inconsistent header names

all_sessions
- total transaction revenue is either null or huge
- sessions never have more than one transaction
- date is given as a number
- product refund amount is empty
- product price is either 0 or huge
- product quantity has many null values
- item quantity is empty, redundant with product quantity
- item revenue is empty, redundant with product revenue
- transaction revenue is either null or huge
- search keyword is empty
- product revenue is either null or huge

analytics
- date is given as number
- user id is empty, redundant with visitor id
- social engagement type has only one value, not socially engaged
- bounces is either null, 1, or 2
- unit price is either 0 or huge
- lots of near duplicates. most differ by only unit price, but some have other differences as well

products
- one product has null values, in sentiment score and magnitude

sales by sku
- sales by sku is included in sales report, save for 8 entries

sales report
- many redundant attributes with products: name, stock, restock lead time, sentiment score, sentiment magnitude
- ratio is total ordered / stock, redundant if all 3 are included


Queries:
Below, provide the SQL queries you used to clean your data.

-- all_sessions
ALTER TABLE all_sessions
ALTER COLUMN total_trans_rev TYPE decimal;

UPDATE all_sessions
SET total_trans_rev = total_trans_rev / 1000000
WHERE total_trans_rev IS NOT NULL;

UPDATE all_sessions
SET total_trans_rev = 0
WHERE total_trans_rev IS NULL;

UPDATE all_sessions
SET transactions = 0
WHERE transactions IS NULL;

ALTER TABLE all_sessions
ADD COLUMN date date;
UPDATE all_sessions
SET date = date_int::text::date;
ALTER TABLE all_sessions
DROP COLUMN date_int;

ALTER TABLE all_sessions
DROP COLUMN prod_refund_amt;

ALTER TABLE all_sessions
ALTER COLUMN prod_price TYPE decimal;

UPDATE all_sessions
SET prod_price = prod_price / 1000000
WHERE prod_price IS NOT NULL;

UPDATE all_sessions
SET prod_quant = 0
WHERE prod_quant IS NULL;

ALTER TABLE all_sessions
DROP COLUMN item_quant;

ALTER TABLE all_sessions
DROP COLUMN item_rev;

ALTER TABLE all_sessions
ALTER COLUMN trans_rev TYPE decimal;

UPDATE all_sessions
SET trans_rev = trans_rev / 1000000
WHERE trans_rev IS NOT NULL;

UPDATE all_sessions
SET trans_rev = 0
WHERE trans_rev IS NULL;

ALTER TABLE all_sessions
DROP COLUMN search_keyword;

ALTER TABLE all_sessions
ALTER COLUMN prod_rev TYPE decimal;

UPDATE all_sessions
SET prod_rev = 0
WHERE prod_rev IS NULL;

UPDATE all_sessions
SET prod_rev = prod_rev / 1000000;

-- analytics

ALTER TABLE analytics
ADD COLUMN date date;
UPDATE analytics
SET date = date_int::text::date;
ALTER TABLE analytics
DROP COLUMN date_int;

ALTER TABLE analytics
DROP COLUMN user_id;

ALTER TABLE analytics
DROP COLUMN social_engage;

UPDATE analytics
SET bounces = 0
WHERE bounces IS NULL

ALTER TABLE analytics
ALTER COLUMN unit_price TYPE decimal;

UPDATE analytics
SET unit_price = unit_price / 1000000;

INSERT INTO analytics_array
SELECT visit_num, visit_id, visit_start_time, 
visitor_id, channel, units_sold, 
page_views, time_site, bounces, 
revenue, ARRAY_AGG(unit_price) as unit_prices, date
FROM analytics 
GROUP BY visit_num, visit_id, visit_start_time, 
visitor_id, channel, units_sold, 
page_views, time_site, bounces, 
revenue, date;

DROP TABLE analytics;

-- products

DELETE FROM products WHERE NOT (products IS NOT NULL)

-- sales_by_sku

DROP TABLE sales_by_sku;

-- sales_report

ALTER TABLE sales_report
DROP COLUMN name,
DROP COLUMN stock,
DROP COLUMN restock_time;

ALTER TABLE products
DROP COLUMN sent_score,
DROP COLUMN sent_mag;