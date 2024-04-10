SQL
## 175
###
```
SELECT firstName, lastName, city, state FROM Person p
LEFT JOIN Address a ON a.personID = p.personID
```

## 176

```
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

##177

```
CREATE OR REPLACE FUNCTION NthHighestSalary(N INT) RETURNS TABLE (Salary INT) AS $$
BEGIN
  RETURN QUERY (
  SELECT DISTINCT tab.salary FROM (
	    SELECT DENSE_RANK() OVER(ORDER BY e.salary desc) AS getNthHighestSalary, e.* FROM Employee e
    ) as tab
    WHERE getNthHighestSalary = N
  );
END;
$$ LANGUAGE plpgsql;
```

##178

```
SELECT s.score, DENSE_RANK() OVER(ORDER BY score desc) AS rank FROM Scores s
```
###181

```
SELECT a.Name AS Employee
FROM Employee a JOIN Employee b on a.ManagerId = b.Id
WHERE a.Salary > b.Salary
```

##182

```
SELECT email FROM person
GROUP BY email
HAVING COUNT(email) > 1
```

##Game Play Analysis I

```
SELECT player_id, min(event_date) AS first_login FROM activity
GROUP BY player_id
```

