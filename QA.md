What are your risk areas? Identify and describe them.
1. One of the risks in the data integrity is unnecessary data redundancy, since it can potentially lead to miscalculations (ex. calculating a sum of revenue with duplicates will overrepresent sales).
2. Another risk is missing data, since analysis made with data missing cannot be entirely credible or conclusive.
3. Incorrect datatypes is a problem since some data cannot be manipulated properly if it is the wrong type, and results will also not make sense if that data is used.
4. Consistency acrosss the database is also important, for readability and to prevent accidentally missing redundant data.


QA Process:
Describe your QA process and include the SQL queries used to execute it.
1. All naming conventions will follow `snake_case` for a standard in the database, and also since `postgres` is case insensitive.
2. Datatypes are adjusted during the cleaning process or as soon as there is a clear issue.
```SQL
-- An alteration of an attribute from an 'int' to 'money' type.
ALTER TABLE all_sessions
ALTER COLUMN unit_price TYPE money USING unit_price::money
```
3. `NULL` will be the standard for values that are empty or 'useless' data; some recorded info is effectively useless, but not technically `NULL` which can cause issues.
```SQL
-- An alteration for consistency of void values, setting them to NULL.
UPDATE all_sessions
SET
	country = NULL
WHERE
	country = '(not set)'
```
4. 
