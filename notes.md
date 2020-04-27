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

## 
