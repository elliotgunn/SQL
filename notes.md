## Views
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

## Case
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

## Coalesce
- returns the first non null value in a list

```
SELECT id, COALESCE(firstname, middlename, lastname) as name
FROM employee
```

```

