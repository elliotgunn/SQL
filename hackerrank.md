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

Challenges
- pretty tricky join

- create a table aggregating the total challenges created per individual. then exclude individuals who created the same number of challenges that is not the maximum number of challenges. this should output individuals with either: the total number of challenges created is the maximum, or the number of challenges appears once.

- first, include students whose output is the maximum total number of challenges created 
```
SELECT h.hacker_id, h.name, COUNT(c.challenge_id) AS total
FROM hackers h
JOIN challenges c
ON h.hacker_id = c.challenge_id
GROUP BY h.hacker_id, h.name
HAVING total = (SELECT COUNT(c2.challenge_id)
                FROM challenges c2
                GROUP BY c2.hacker_id
                ORDER BY COUNT(*) DESC
                LIMIT 1)
ORDER BY total DESC
```

- second, include students whose output only appears once
- the NOT IN filters against duplicate output numbers (if the student's output is the same as all other student's outputs)
- the `HAVING c3.hacker_id <> c.hacker_id` condition filters the same total output but across *different* `hacker_id`s
```
SELECT c.hacker_id, h.name, COUNT(c.challenge_id) AS total
FROM hackers h
JOIN challenges c
ON h.hacker_id = c.hacker_id
GROUP BY h.hacker_id, h.name
HAVING total = (SELECT COUNT(c2.challenge_id)
                FROM challenges c2
                GROUP BY c2.hacker_id
                ORDER BY COUNT(*) DESC
                LIMIT 1) 
    OR total NOT IN (SELECT COUNT(c3.challenge_id)
                 FROM challenges c3
                 GROUP BY c3.hacker_id
                 HAVING c3.hacker_id <> c.hacker_id)
ORDER BY total DESC,
         c.hacker_id ASC
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

The Report
- good example for testing CASE and ORDER BY
```
SELECT (CASE 
            WHEN g.grade < 8 THEN 'NULL'
            ELSE s.name
            END), g.grade, s.marks
FROM students s
INNER JOIN grades g
ON s.marks 
BETWEEN g.min_mark AND g.max_mark
ORDER BY g.grade DESC, 
         s.name ASC, 
         s.marks ASC
```

Symmetric Pairs
- tough one 
- UNION: removes duplicate rows
- the first query returns for the condition where: x equals y, if (x, y) appears twice, it is included
- the second query uses two tables to iterate through all the records and returns for the condition where: x is not equal to y, and symmetric pairs, and exclude duplicates e.g. (20,21) and (21,20) are a symmetric pair but we only want the smaller x
```
SELECT f1.X, f1.Y 
FROM functions as f1
WHERE f1.X = f1.Y
AND (SELECT COUNT(*) 
     FROM functions 
     WHERE X = f1.X AND Y = f1.X) > 1
UNION
SELECT f1.X, f1.Y 
FROM functions f1, functions f2
WHERE f1.X <> f1.Y AND f1.X = f2.Y AND f1.Y = f2.X AND f1.X < f2.X
ORDER BY X
```

Occupations
- pivot problem 
- this is tricky because the draft solution creates many nulls in the columns
- if you reshape occupation into their four columns it creats many NULLs
```
SELECT CASE 
            WHEN occupation =  'Doctor' THEN name
            END as Doctor,
        CASE 
            WHEN occupation =  'Professor' THEN name
            END as Professor,
        CASE 
            WHEN occupation =  'Singer' THEN name
            END as Singer,
        CASE 
            WHEN occupation =  'Actor' THEN name
            END as Actor  
FROM occupations
ORDER BY Doctor, Professor, Singer, Actor
```
- So we want to create a temp table with `row_number`, `doctor`, `professor` etc. as columns
- Then query that using min() and groupby:
    - using MAX() or MIN() can get the first non-null value after GROUP BY. it will return a name for specific index and  specific occupation. if there is no name, it returns NULL. 
    - so we can aggregate rows together by the row number
```
SET @r1=0, @r2=0, @r3=0, @r4=0;

SELECT MIN(Doctor), MIN(Professor), MIN(Singer), MIN(Actor)
FROM (
      SELECT CASE
                WHEN occupation='Doctor' THEN (@r1:=@r1+1)
                WHEN occupation='Professor' THEN (@r2:=@r2+1)
                WHEN occupation='Singer' THEN (@r3:=@r3+1)
                WHEN occupation='Actor' THEN (@r4:=@r4+1)
                END AS row_number,
             CASE
                WHEN occupation='Doctor' THEN name END AS Doctor,
            CASE
                WHEN occupation='Professor' THEN name END AS Professor,
            CASE 
                WHEN occupation='Singer' THEN name END AS Singer,
            CASE 
                WHEN occupation='Actor' THEN name END AS Actor
      FROM occupations
      ORDER BY name
      ) temp
GROUP BY row_number
```

Top Competitors
- basic joins 
- then filter to ensure (1) full scores in the correct (2) difficulty level
- GROUP BY so that we can COUNT > 1 full score
- remember: can't select a column if it's not in the GROUP BY condition
```
SELECT s.hacker_id, h.name
FROM submissions s
    JOIN challenges c ON s.challenge_id = c.challenge_id
    JOIN difficulty d ON d.difficulty_level = c.difficulty_level
    JOIN hackers h ON s.hacker_id = h.hacker_id  
WHERE s.score = d.score
    AND c.difficulty_level = d.difficulty_level
GROUP BY h.hacker_id, h.name
HAVING COUNT(s.hacker_id) > 1
ORDER BY COUNT(s.hacker_id) DESC,
         s.hacker_id ASC
```

Ollivander's Inventory
- joins
- the first part is self-explanatory
- then we want to filter by only `is_evil` = 0
- and then we want to find the lowest price for wands at the same `age` & `power`
```
SELECT w.id, wp.age, w.coins_needed, w.power
FROM wands w
JOIN wands_property wp
ON w.code = wp.code
WHERE wp.is_evil = 0 
    AND w.coins_needed = (SELECT MIN(coins_needed)
                          FROM wands w2
                          JOIN wands_property wp2
                          ON w2.code = wp2.code
                          WHERE w.power = w2.power
                          AND wp.age = wp2.age)
ORDER BY w.power DESC,
         wp.age DESC
```

SQL Project Planning
- advanced join

First, if `start_date` appears in `end_date`, we can exclude it as an `end_date` for a project. So we want to find the `start_date`s excluded, since this would indicate they are project `start_date`s. 
```
SELECT Start_Date 
FROM Projects 
WHERE Start_Date 
NOT IN (SELECT End_Date FROM Projects);
```

Second, if `end_date` appears in `start_date`, we can also exclude it as an `end_date` for a project. So we want to find the `end_date`s excluded, since this would indicate they are project `end_date`s. 
```
SELECT End_Date 
FROM Projects 
WHERE End_Date 
NOT IN (SELECT Start_Date FROM Projects);
```

Third, we cross join these two tables to generate all potential pairs. We first filter them down logically by picking only pairs where `start_date` < `end_date`. But this still gives us many potential pairs. So then we GROUP BY `start_date`, and then pick the `MIN(end_date)` to get the correct pair.
```
SELECT start_date, MIN(end_date)
FROM 
    (SELECT start_date FROM projects WHERE start_date NOT IN (SELECT end_date FROM projects)) a,
    (SELECT end_date FROM projects WHERE end_date NOT IN (SELECT start_date FROM projects)) b 
WHERE start_date < end_date
GROUP BY start_date
```

Finally, sort by number of days in ascending order, then by `start_date`:
```
SELECT start_date, MIN(end_date)
FROM 
    (SELECT start_date FROM projects WHERE start_date NOT IN (SELECT end_date FROM projects)) a,
    (SELECT end_date FROM projects WHERE end_date NOT IN (SELECT start_date FROM projects)) b 
WHERE start_date < end_date
GROUP BY start_date
ORDER BY DATEDIFF(MIN(end_date), start_date) ASC,
         start_date ASC
```

Interviews
- advanced join

For the two main tables, we can do the sum(totals) by `challenge_id`:
```
SELECT ss.challenge_id, SUM(ss.total_submissions) AS total_submissions, SUM(ss.total_accepted_submissions) AS total_accepted submissions
FROM submission_stats ss
GROUP BY ss.challenge_id

SELECT vs.challenge_id, SUM(vs.total_views) AS total_views, SUM(vs.total_unique_views) AS total_unique_views
FROM view_stats vs
GROUP BY vs.challenge_id
```

Then we join the results with the three other tables and group by the non-aggregated ones:
```
SELECT c.contest_id, c.hacker_id, c.name, SUM(ss2.total_submissions), 
       SUM(ss2.total_accepted_submissions), SUM(vs2.total_views), SUM(vs2.total_unique_views)
FROM contests c
JOIN colleges col ON c.contest_id = col.contest_id
JOIN challenges ch ON ch.challenge_id = col.college_id
LEFT JOIN (SELECT ss.challenge_id, SUM(ss.total_submissions) AS total_submissions, 
                  SUM(ss.total_accepted_submissions) AS total_accepted submissions
           FROM submission_stats ss
           GROUP BY ss.challenge_id) AS ss2
           ON ss2.challenge_id = ch.challenge_id
LEFT JOIN (SELECT vs.challenge_id, SUM(vs.total_views) AS total_views, 
                  SUM(vs.total_unique_views) AS total_unique_views
           FROM view_stats vs
           GROUP BY vs.challenge_id) as vs2
           ON vs2.challenge_id = ch.challenge_id
GROUP BY c.contest_id, c.hacker_id, c.name;
```

Finally, exclude results if all their sums are 0:
```
SELECT c.contest_id, c.hacker_id, c.name, 
       SUM(ss2.total_submissions), SUM(ss2.total_accepted_submissions), 
       SUM(vs2.total_views), SUM(vs2.total_unique_views)
FROM Contests AS c
JOIN Colleges AS col ON c.contest_id = col.contest_id
JOIN Challenges AS cha ON cha.college_id = col.college_id
LEFT JOIN
    (SELECT ss.challenge_id, SUM(ss.total_submissions) AS total_submissions,           
            SUM(ss.total_accepted_submissions) AS total_accepted_submissions 
     FROM Submission_Stats AS ss GROUP BY ss.challenge_id) AS ss2
    ON cha.challenge_id = ss2.challenge_id
LEFT JOIN
    (SELECT vs.challenge_id, SUM(vs.total_views) AS total_views, 
            SUM(vs.total_unique_views) AS total_unique_views
     FROM View_Stats AS vs 
     GROUP BY vs.challenge_id) AS vs2
    ON cha.challenge_id = vs2.challenge_id
GROUP BY c.contest_id, c.hacker_id, c.name
HAVING SUM(ss2.total_submissions) +
       SUM(ss2.total_accepted_submissions) +
       SUM(vs2.total_views) +
       SUM(vs2.total_unique_views) > 0
ORDER BY c.contest_id;
```
