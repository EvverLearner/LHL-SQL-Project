What issues will you address by cleaning the data?
1. The Primary Key (PK) for each table is suitable. This can be checked by comparing the amount of returned rows on a `SELECT *` to the amount returned looking for `DISTINCT` values in the PK; the same amount means that every row in that column is distinct, ergo it can be a possible PK.




Queries:
Below, provide the SQL queries you used to clean your data.

```SQL
-- Confirms that id is a suitable PK for all_sessions table. The total rows returned is the same 
SELECT DISTINCT id FROM all_sessions
```
