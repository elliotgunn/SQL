The Blunder

```SELECT CEIL(AVG(Salary) - AVG(REPLACE(Salary, '0', '')))
FROM employees
```

