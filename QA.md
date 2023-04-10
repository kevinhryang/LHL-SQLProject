What are your risk areas? Identify and describe them.

- pgadmin crashes often after making large updates/alterations to tables
- some fields are difficult to make sense of without context, i.e. the time column in all_sessions table
- analytics.csv is way too large to view directly
- hint says to divide prices by a million, decimal arithmetic during analysis later in the pipeline may get affected if rounded

QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Capitalize SQL keywords
- Lowercase and underscores for attributes
- Tend towards separate, shorter queries rather than comprehensive, long ones, especially when updating or altering tables
- Counting rows before and after updating/altering tables
e.g.
SELECT * FROM all_sessions -- 15134
SELECT * FROM all_sessions WHERE transactions IS NULL; -- 15053
SELECT * FROM all_sessions WHERE transactions IS NOT NULL; -- 81
-- 15053 + 81 = 15134
- imputate only if it makes sense to do so, otherwise leave as null
- pull a small subset from analytics.csv into test.csv for surface level entry view
- keep price precision with numeric/decimal types
- ensure dates were converted properly
e.g. after new date column was populated, former date_int column still exists
SELECT * FROM all_sessions WHERE date_int::text::date != date
-- should have 0 entries, which means conversion succeeded and date_int column can be dropped
ß