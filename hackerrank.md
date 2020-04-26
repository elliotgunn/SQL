The Blunder

```
SELECT CEIL(AVG(Salary) - AVG(REPLACE(Salary, '0', '')))
FROM employees
```

Top Earners
```
SELECT (salary*months) as earnings, COUNT(*)
FROM employee
GROUP BY earnings
ORDER BY earnings desc
LIMIT 1
```
