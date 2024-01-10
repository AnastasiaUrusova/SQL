# SQL

## Task 2
### Make a select statement which returns all person's names and person's ages from the city ‘[any]’.
```sql
SELECT name, age, address FROM person;
WHERE address = 'Kazan' 
```
![image](https://github.com/Nefyss/SQL/assets/113514047/73c9b3f2-b70d-4130-970d-6c40014e2e25)

## Task 3
### Make a select statement which returns names , ages for all women from the city ‘[any]. Yep, and sort result by name.
```sql
SELECT name, age, address, gender FROM person;
WHERE gender = 'female' and address = 'Kazan'
ORDER by name
```
![image](https://github.com/Nefyss/SQL/assets/113514047/07bdfd1b-bd81-4c1b-a804-62408d43857a)

## Task 4
### Make 2 syntax different select statements which return a list of pizzerias (pizzeria name and rating) with rating between 3.5 and 5 points (including limit points) and ordered by pizzeria rating.
```sql
SELECT name, rating FROM pizzeria;
WHERE rating >=3.5 and rating <=5
ORDER BY rating
```
![image](https://github.com/Nefyss/SQL/assets/113514047/5f148677-87ee-4ed8-98b7-38ee93c0b1e2)

## Task 5
### Make a select statement which returns the person's identifiers (without duplication) who visited pizzerias in a period from [any date period](including all days) or visited pizzeria with identifier 2. Also include ordering clause by person identifier in descending mode
```sql
SELECT DISTINCT (person_id) FROM person_visits;
WHERE visit_date >= '2022-01-05' and visit_date <='2022-01-06' or pizzeria_id = '2'
ORDER BY person_id DESC
```
![image](https://github.com/Nefyss/SQL/assets/113514047/b88b405f-6c52-4385-9edc-6273d0f6e0e1)

## Task 6
### Make a select statement which returns person's names (based on internal query in SELECT clause) who made orders for the menu with identifiers [3 dates and]. Don’t use JOIN statements in this query. Take a look at the pattern of internal query.
```sql
SELECT name FROM person;
WHERE id IN (SELECT person_id FROM person_order WHERE order_date = '2022-01-01' 
			 or order_date = '2022-01-09' or order_date = '2022-01-09')
```
![image](https://github.com/Nefyss/SQL/assets/113514047/7510887a-bb0f-407a-b5b9-aaaaad1e98ab)

## Task 7
### Make a select to person table that returning (true or false) if person_name == ‘[any]’ exists in table
```sql
SELECT EXISTS (
		SELECT 1 FROM person WHERE name = 'Anna'
	);
```
![image](https://github.com/Nefyss/SQL/assets/113514047/d660f06c-ef4b-424f-8df9-30b729a071bf)
