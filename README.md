# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
The goal of this project is:
1. To construct a database from a set of .csv files
2. Clean and understand the data
3. Ensure that the quality and integrity of the data is consistent
4. Analyze and discover patterns within the data

## Process
### 1. View the .csv files in a text ediot, excel to get an inital perception of the data
### 2. Import the .csv files to pgAdmin using the understanding gleaned from the initial viewing of the data
### 3. Clean the data and make adjustments so that the data can be manipulated for analysis
### 4. Analyze the data for required goals, and check that the results make sense (part of QA)
### 5. Go over the conclusions made and if they are sensible/expected (also done in step 4).

## Results
The database is a collection of information regarding an ecommerce website and details regarding its products, site analytics, and business transactions. The relationship between these pieces of data allows for discovery of patterns, and an exercise in what types of information businesses collect.

## Challenges 
One of the challenges I faced in this project was the ambiguity of data, and lack of descriptors. Some values, like time_on_site, are just a value; it is evidently a record of time, but what unit of time is unclear. Other values, like session_quality_dim, are entirely elusive without asking the business or data source/engineer for guidance. Values like total_ordered can also be ambiguous, because although the description and value make sense, it can be interpreted as an order by customer or and order by the business.\
Another challenge was initially importing the data and understanding what datatypes to set up for each column. There were alterations I had to make post-import because some values made more sense once they were viewed in the RDBMS, where their correct datatype was made more clear.\
A third challenge was the decision points during cleaning of data. The assumption is that whoever gathers the data had a reason for it, however many columns have entirely `NULL` values. Another column, social_engagement_type, was not `NULL` but consisted entirely of `int` 1, which is essentially useless when trying to discern patterns and led to me removing it. Perhaps in a more professional environment or live database system, you would not remove these columns, but for my goals I found it necessary.
The biggest, yet most insightful, challenge was (still is!) the learning and implementing of many different techniques to work in tandem for data analysis.

## Future Goals
With more time, I would try and implement a further normalization of the database. The all_sessions tables seemed unnecessarily large, and I feel there might be a way to more effectively parse the database if the tables are set up in a better fashion, including reduction and rebalancing. This would also help clarify a lot of the redundancy that is found in the database. I would also make functions, especially for the data analysis. This would help code readability and also make analysis more efficient.
