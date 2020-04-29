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

Average Population of Each Continent
```
SELECT country.continent, FLOOR(AVG(city.population))
FROM city
INNER JOIN country 
ON city.countrycode = country.code
GROUP BY country.continent
```

Weather Observation Station 20
- this clever solution only works for odd numbers
```
SELECT ROUND(S.LAT_N, 4) median 
FROM station S 
WHERE
  (SELECT COUNT(Lat_N) FROM station WHERE Lat_N < S.LAT_N ) = 
  (SELECT COUNT(Lat_N) FROM station WHERE Lat_N > S.LAT_N)
```






