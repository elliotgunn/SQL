Using questions in this [guide](https://quip.com/2gwZArKuWk7W). 

# Self Join Problems
1. Find the month-over-month percentage change for monthly active users (MAU). Table: logins.

| user_id | date       |
|---------|------------|
| 1       | 2018-07-01 |
| 234     | 2018-07-02 |
| 3       | 2018-07-02 |
| 1       | 2018-07-02 |
| ...     | ...        |
| 234     | 2018-10-04 |

```
WITH new_table AS
(
  SELECT MONTH(date) month, COUNT(DISTINCT user_id) mau
  FROM logins
  GROUP BY month
)

SELECT a.month previous_month
       a.mau previous_mau
       b.month current_month
       b.mau current_mau
       ((b.mau - a.mau)/a.mau)*100 percent_change
FROM new_table a
JOIN new_table b 
ON a.month = b.month - 1
```

2. Write SQL such that we label each node as a “leaf”, “inner” or “Root” node.

```
node   parent
1       2
2       5
3       5
4       3
5       NULL 
```

Such that we get:
```
node    label  
1       Leaf
2       Inner
3       Inner
4       Leaf
5       Root
```

```
SELECT node, CASE
                WHEN parent is 'NULL' THEN 'Root'
                WHEN node NOT IN (SELECT parent FROM tree) THEN 'Leaf'
                ELSE 'Inner' 
                END AS label
FROM tree
```
