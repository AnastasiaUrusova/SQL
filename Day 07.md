SQL
##Task 00
### Let’s make a simple aggregation, please write a SQL statement that returns person identifiers and corresponding number of visits in any pizzerias and sorting by count of visits in descending mode and sorting in `person_id` in ascending mode. Please take a look at the sample of data below.
```
SELECT pv.person_id, COUNT(pv.id) as count_of_visits 
FROM person_visits pv
GROUP BY 1
ORDER BY 2 DESC, 1;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/c61b825c-f7a7-46c2-b466-30e316ba45a6)

## Task 01
### Please change a SQL statement from Exercise 00 and return a person name (not identifier). Additional clause is  we need to see only top-4 persons with maximal visits in any pizzerias and sorted by a person name. Please take a look at the example of output data below.
```
SELECT p.name, COUNT(pv.id) as count_of_visits 
FROM person_visits pv
JOIN person p ON p.id = pv.person_id
GROUP BY 1
ORDER BY 2 DESC, 1 LIMIT 4;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/44a0693a-d93f-4762-b026-405ac51b4886)

## Task 02
### Please write a SQL statement to see 3 favorite restaurants by visits and by orders in one list (please add an action_type column with values ‘order’ or ‘visit’, it depends on data from the corresponding table). Please take a look at the sample of data below. The result should be sorted by action_type column in ascending mode and by count column in descending mode.
```
WITH vis AS (
	SELECT piz.name, COUNT(pv.id), 'visit' as action_type
	FROM person_visits pv
	JOIN pizzeria piz ON piz.id = pv.pizzeria_id
	GROUP BY 1
	ORDER BY 2 DESC, 1 LIMIT 3
), 
ord AS (
	SELECT piz.name, COUNT(po.id), 'order' as action_type
	FROM person_order po
	JOIN menu m ON m.id = po.menu_id
	JOIN pizzeria piz ON piz.id = m.pizzeria_id
	GROUP BY 1
	ORDER BY 2 DESC, 1 LIMIT 3)
SELECT * FROM vis
UNION ALL
SELECT * FROM ord
ORDER BY 3,  2 DESC;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/4d7fac1d-78bc-4fd9-8ce6-6b7089642876)

## Task 03
### Please write a SQL statement to see restaurants are grouping by visits and by orders and joined with each other by using restaurant name.  
You can use internal SQLs from Exercise 02 (restaurants by visits and by orders) without limitations of amount of rows.

Additionally, please add the next rules.
- calculate a sum of orders and visits for corresponding pizzeria (be aware, not all pizzeria keys are presented in both tables).
- sort results by `total_count` column in descending mode and by `name` in ascending mode.
Take a look at the data sample below.
```
WITH vis AS (
	SELECT piz.name, COUNT(pv.id)
	FROM person_visits pv
	JOIN pizzeria piz ON piz.id = pv.pizzeria_id
	GROUP BY 1
), 
ord AS (
	SELECT piz.name, COUNT(po.id)
	FROM person_order po
	JOIN menu m ON m.id = po.menu_id
	JOIN pizzeria piz ON piz.id = m.pizzeria_id
	GROUP BY 1
),
alll AS (
SELECT * FROM vis
UNION ALL
SELECT * FROM ord
)
SELECT a.name, SUM(a.count) as total_count 
FROM alll a
GROUP BY 1
ORDER BY 2 DESC, 1;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/eba1d7b7-d128-4582-8e9f-1639b7dbc631)

## Task 05
### Please write a simple SQL query that returns a list of unique person names who made orders in any pizzerias. The result should be sorted by person name. Please take a look at the sample below.
```
SELECT DISTINCT p.name FROM person_order po
JOIN person p ON p.id = po.person_id
ORDER BY 1;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/87d13733-be7e-4bbb-9289-5a74256d9a5b)

## Task 06
### Please write a SQL statement that returns the amount of orders, average of price, maximum and minimum prices for sold pizza by corresponding pizzeria restaurant. The result should be sorted by pizzeria name. Please take a look at the data sample below. 
Round your average price to 2 floating numbers.
```
SELECT 
	piz.name, 
	COUNT(po.id) as count_of_orders, 
	ROUND(AVG(m.price)::numeric, 2) as average_price, 
	MAX(m.price) as max_price, 
	MIN(m.price) as min_price 
	FROM person_order po
JOIN menu m ON m.id = po.menu_id
JOIN pizzeria piz ON piz.id = m.pizzeria_id
GROUP BY 1
ORDER BY 1;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/34a4bddb-7d9b-4e7c-a72a-e8f355713705)

## Task 07
### Please write a SQL statement that returns a common average rating (the output attribute name is global_rating) for all restaurants. Round your average rating to 4 floating numbers.

```
SELECT 
	ROUND(AVG(piz.rating)::numeric, 4) as global_rating
	FROM pizzeria piz
;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/df37a79b-81bb-413e-bdd6-58da6b624e34)

## Task 08
### We know about personal addresses from our data. Let’s imagine, that particular person visits pizzerias in his/her city only. Please write a SQL statement that returns address, pizzeria name and amount of persons’ orders. The result should be sorted by address and then by restaurant name. Please take a look at the sample of output data below.

```
SELECT p.address, piz.name, COUNT(po.id)
FROM person_order po
JOIN menu m ON m.id = po.menu_id
JOIN pizzeria piz ON piz.id = m.pizzeria_id
JOIN person p ON p.id = po.person_id
GROUP BY 1, 2
ORDER BY 1, 2;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/c4e27546-4ab7-419c-abe0-0c6cdf51ed83)

## Task 09
### Please write a SQL statement that returns aggregated information by person’s address , the result of “Maximal Age - (Minimal Age  / Maximal Age)” that is presented as a formula column, next one is average age per address and the result of comparison between formula and average columns (other words, if formula is greater than  average then True, otherwise False value).

```
WITH tab AS(
	SELECT 
	p.address,
	ROUND(MAX(p.age) - (MIN(p.age)/MAX(p.age))::numeric, 2) as formula,
	ROUND(AVG(p.age)::numeric, 2) as average
FROM person p
GROUP BY 1
ORDER BY 1, 2)

SELECT *, 
	CASE
		WHEN formula > average THEN 'True'
		WHEN formula < average THEN 'False'
	END
	FROM tab;
```
![image](https://github.com/AnastasiaUrusova/SQL/assets/112615007/90af76da-1a2f-4c71-9de3-94e3c64859cf)
