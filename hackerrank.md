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

Weather Observation Station 15
```
SELECT ROUND(LONG_W, 4)
FROM station
WHERE LAT_N = (SELECT MAX(LAT_N) FROM station WHERE LAT_N < 137.2345) 
```

Weather Observation Station 18
```
SELECT ROUND(ABS(MIN(LAT_N) - MAX(LAT_N)) + ABS(MIN(LONG_W) - MAX(LONG_W)), 4)
FROM station
```
