Notes:

SELECT DISTINCT COUNT(column) column(s) 
FROM table
WHERE column conditions
LIMIT number
ORDER BY column ASC/DESC;




DISTINCT keyword to return only distinct values due to duplicates, using: logical operators, AND, OR

WHERE to query just specific rows 
- WHERE first_name = ‘Jamie’ AND last_name = ’Smith’

COUNT returns number of rows that returns from the SELECT that matches a condition, but does not consider null values in the column
- to use count with distinct: SELECT COUNT(DISTINCT column): will count distinct rows for that column

LIMIT limit number of rows you get back, goes at end of a query

ORDER BY lets you sort the results by ascending or descending order
- ORDER BY column ASC/DESC

BETWEEN 
- value BETWEEN low AND high  >= <=
- value NOT BETWEEN low AND high: value < low OR value > high

IN is a subquery, or in a list 
- WHERE value IN (SELECT value FROM table), (1, 2) 
- WHERE value NOT IN…

LIKE kinda like regex
-  WHERE column LIKE ‘Jen%’;
- % matching any sequence of characters 
- _ matching any single character 
- ‘_her%’ 
- NOT LIKE…

MIN MAX AVG SUM 
- SELECT AVG(column) 
- SELECT ROUND( AVG(column), decimals)

GROUP BY divides rows returned from SELECT statement into groups, for each group you can apply an aggregate function (e.g. count # of items in the groups)
- SELECT column1 MAX(column2) FROM table GROUP BY column1

HAVING used after the GROUP BY to filter group rows that do not satisfy a specific condition
- HAVING condition

AS rename columns as aliases 

JOIN for each row in table 1, scans table 2 to check if there are any row that matches the condition; this comes after FROM 
- SELECT table1.column, table2.column
FROM table1
INNER JOIN table 2 AS tab2 ON table1.column = table2.column

UNION operator removes all duplicate rows unless the UNION AL is used; often used to combine data from similar tables that are not perfectly normalized; almost like concatenate 

Timestamps 
- SELECT extract (day from payment_date) 

Mathematical operators, string functions 
- Google this

Subquery 

Self Join: performance is better than subqueries esp if refer to same table 
- use when combining rows with other rows in the same table 
- to perform the self join operation, must use a table alias to help SQL distinguish the left from right table: AS statement 
SELECT e1.employee_name
FROM employee AS e1, employee AS e2
WHERE e1.employee_location = e2.employee_location;

SELECT e1.employee_name
FROM employee AS e1
JOIN employee AS e2
ON e1.employee_location = e2.employee_location;











