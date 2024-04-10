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
##262

```
WITH twb AS(
    SELECT request_at::date as day, status FROM trips
    WHERE client_id NOT IN (
        SELECT users_id FROM users WHERE banned = 'Yes'
    )
    AND driver_id NOT IN (
        SELECT users_id FROM users WHERE banned = 'Yes'
    )
)

SELECT 
    day AS "Day",
    ROUND (CAST(COUNT(*)FILTER (WHERE status LIKE 'cancelled%') AS NUMERIC)
    /
    CAST(COUNT(*) AS NUMERIC) ,2) AS "Cancellation Rate"
FROM twb
WHERE day BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY day
ORDER BY 1
```
