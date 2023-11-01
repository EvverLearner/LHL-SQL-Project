What issues will you address by cleaning the data?

**General**
1. Checking if the Primary Key (PK) for each table is suitable. This can be checked by comparing the amount of returned rows on a `SELECT *` to the amount returned looking for `DISTINCT` values in the PK; the same amount means that every row in that column is distinct, ergo it can be a possible PK.
2. Providing a PK for a table that has no suitable column. I achieved this by adding an auto-incrementing `id` column to such tables.

**all_sessions**
1. I am unsure what the values in the `time` column represents. As a result, I will be cautious in adjusting or using that column for analysis. 




Queries:
Below, provide the SQL queries you used to clean your data.

**all_sessions**

```SQL
-- Confirms that 'id' is a suitable PK for all_sessions table.
SELECT id FROM all_sessions

SELECT DISTINCT id FROM all_sessions
```
