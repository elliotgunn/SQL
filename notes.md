## VIEW
- database object that is of a stored query, simplifies the complexity of a query

Using a view
```
Select * 
FROM view
```

Create a view
```
CREATE VIEW view-name AS
SELECT ...
FROM ....
```

Then query it directly:
```
SELECT * 
FROM view-name
```

Alter view:
```
ALTER VIEW view-name
RENAME TO new-name
```

Delete view:
```
DROP VIEW view-name
```

## CASE
- goes through conditions and returns a value when the first condition is met
- like IF-THEN-ELSE statement

Case: specific column
```
SELECT name, CASE country
              WHEN 'Ger' then 'German'
              WHEN 'Mexico' then 'Mexican'
              ELSE 'Not known'
              END AS nationality
FROM customers
ORDER BY nationality
```

Case: no specific column
```
SELECT productid, CASE
                    WHEN unitprice < 20 THEN 'Cheap'
                    WHEN unitprice < 80 THEN 'Moderate'
                    ELSE 'Expensive'
                    END AS Expense
FROM products
ORDER BY Expense
```

## COALESCE
- returns the first non null value in a list

```
SELECT id, COALESCE(firstname, middlename, lastname) as name
FROM employee
```

## CAST & CONVERT
- converts a value to a specified datatype 
- CONVERT has an additional style parameter

Both these queries evaluate to the same output:
```
SELECT id, name, birthday, CAST(birthday as nvarchar) as convertedbirthday
FROM employees
```

```
SELECT id, name, birthday, CONVERT(nvarchar, birthday) as convertedbirthday
FROM employees
```

## IF & NULLIF
- IF: evaluates to a true and false value in the parameters

```
SELECT movieid, movietitle
IF (movietitle LIKE '%the%', 'THE MOVIE', NULL) AS message
FROM movies
```

- NULLIF: returns a null value if the two expressions are equal 

```
SELECT movieid, movietitle
NULLIF (genreid, 2) AS scifi_null
FROM movies
```
