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

Contest Leaderboard 
- really excellent problem that tests knowledge of groupby, subquery, ordering, aggreg
```
SELECT m.hacker_id, h.name, SUM(m.score) AS total_score
FROM
    (SELECT s.hacker_id, s.challenge_id, MAX(s.score) AS score
    FROM submissions as s
    GROUP BY s.hacker_id, s.challenge_id) AS m
INNER JOIN hackers as h 
ON m.hacker_id = h.hacker_id
GROUP BY m.hacker_id, h.name
HAVING total_score > 0 
ORDER BY total_score DESC, hacker_id ASC
```

Placements
- great problem about joining three tables
```
SELECT s.Name
FROM students as s
JOIN friends as f ON s.id = f.id
JOIN packages p1 ON s.id = p1.id
JOIN packages p2 ON f.friend_id = p2.id
WHERE p2.salary > p1.salary
ORDER BY p2.salary
```

New Companies
- great problem for advanced select
```
SELECT c.company_code, c.founder, COUNT(DISTINCT l.lead_manager_code), 
      COUNT(DISTINCT s.senior_manager_code),        
      COUNT(DISTINCT m.manager_code), COUNT(DISTINCT e.employee_code)
FROM company c, lead_manager l, senior_manager s, manager m, employee e
WHERE c.company_code = l.company_code
    AND s.lead_manager_code = l.lead_manager_code
    AND s.senior_manager_code = m.senior_manager_code
    AND e.manager_code = m.manager_code
GROUP BY c.company_code, c.founder
ORDER BY c.company_code
```
