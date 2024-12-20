# PostgreSQL Basics

<details>
  <summary> Managing Database : [ SHOW | CREATE | DROP | BACKUP | RESTORE | RENAME | COPY ] : Database </summary>
<br><br>  

| Command    | Description |  
| ----------- | ----------- |  
|**SHOW DATABASES;**   | To see the list of all the databases on the sql server.      |  
|**CREATE DATABASE database_name ;**  |  To create a new database.|  
|**DROP DATABASE database_name ;** | To drop the entire database |  
|**BACKUP DATABASE ;** |🤷 |  
|**RESTORE DATABASE ;** |🤷 |  
|**RENAME DATABASE ;** |🤷 |   
|**COPY DATABASE ;** |🤷 |   

<br>  

</details>

<details>
  <summary>Execution Order : [ SELECT with all clause] </summary>

## Execution Order
PostgreSQL evaluates the select statements with all clause as follows:   
``FROM`` -> ``WHERE`` -> ``GROUP BY`` -> ``HAVING`` -> ``SELECT`` -> ``DISTINCT`` -> ``ORDER BY`` -> ``LIMIT``
</details>
<details>
  <summary>Querying Data : [ SELECT | SELECT DISTINCT | Column Alias ] : Table </summary>

## SELECT 
The **SELECT** statement has the following clauses:
  - Select distinct rows using **DISTINCT** operator.
  - Sort rows using **ORDER BY** clause.
  - Filter rows using **WHERE** clause.
  - Select a subset of rows from a table using **LIMIT or FETCH** clause.
  - Group rows into groups using **GROUP BY** clause.
  - Filter groups using **HAVING** clause.
  - Join with other tables using joins such as **INNER JOIN**, **LEFT JOIN**, **FULL OUTER JOIN**, **CROSS JOIN** clauses.
  - Perform set operations using **UNION**, **INTERSECT**, and **EXCEPT** 

In this section, we will focus on **SELECT** and **FROM** clause.

  | Command    | Description |
  | ----------- | ----------- |  
  | **SELECT** select_list <br> **FROM** table_name | General statement for basic query <br> <b>Note : <b> Where clause is optional| 
  | **SELECT** **DISTINCT** column1 <br>**FROM** table_name; | Removes duplicate rows from a result set |
  | **SELECT** **DISTINCT** column1, column2 <br>**FROM** table_name; | Removes duplicate rows from a result set. <br>It uses the combination of values in both column1 and column2 columns for evaluating the duplicate. |
  | **SELECT** column_name **AS** alias_name <br> **FROM** table_name <br><br> Or, <br><br> **SELECT** column_name alias_name <br> **FROM** table_name | The column_name is assigned an alias alias_name. <br>The **AS** keyword is optional so we can omit it like later command.<br>Both command will work as same |
  | **SELECT** expression **AS** alias_name <br>**FROM** table_name; | |

**Note:**  
- select list that can be a column or a list of columns in a table from which we want to retrieve data. If we specify a list of columns, we need to place a comma (,) between two columns to separate them. If we want to select data from all the columns of the table, we can use an asterisk (*) shorthand instead of specifying all the column names. The select list may also contain expressions or literal values.
- The FROM clause is optional. If we are not querying data from any table, we can omit the FROM clause in the SELECT statement.
- The **DISTINCT** keyword operates on column(s)
- If a column alias contains one or more spaces, we need to surround it with double quotes. ( ```column_name AS "alias name"```)

**Execution Order:**  
PostgreSQL evaluates the FROM clause before the SELECT clause in the SELECT statement: FROM -> SELECT

**Examples:**  
Find the first names of all customers from the customer table:
```PostgreSQL
SELECT first_name
FROM customer;
```
Retrieve first name, last name, and email of customers:
```PostgreSQL
SELECT first_name, last_name, email
FROM customer;
```
Retrieve data from all columns of the customer table:
```PostgreSQL
SELECT *
FROM customer;
```
To find distinct values of all columns in a table:
```PostgreSQL
SELECT DISTINCT *
FROM table_name;
```
Retrieve first name, last name of customers where the last need to be shown as surname:
```PostgreSQL
SELECT first_name, last_name AS surname
FROM customer;
```
Retrieve full_name of customers using first name, last name:
```PostgreSQL
SELECT first_name || ' ' || last_name AS full_name
FROM customer;
```
Or, 
```PostgreSQL
SELECT first_name || ' ' || last_name AS "full name"
FROM customer;
```
</details>

<details>
  <summary>Querying Data : [ ORDER BY ] : Table </summary>
    
  | Command | Description |
  | --- | --- |  
  | **SELECT** select_list <br>**FROM** table_name <br> **ORDER BY**  <br>sort_expression1 **ASC/DESC** , <br> sort_expression2 **ASC/DESC** | The ORDER BY clause allows to sort rows returned by a SELECT clause in ascending or descending order based on a sort expression | 
  | **ORDER BY** sort_expresssion [ASC or DESC] [NULLS FIRST or NULLS LAST] | In the database world, NULL is a marker that indicates the missing data or the data is unknown at the time of recording. When we sort rows that contain NULL, we can specify the order of NULL with other non-null values by using the NULLS FIRST or NULLS LAST option of the ORDER BY clause <br> <br> The **NULLS FIRST** option places NULL before other non-null values and the **NULL LAST** option places NULL after other non-null values. |

  **Execution Order:**  
  PostgreSQL evaluates the clauses in the SELECT statement in the following order: ``FROM``, ```SELECT```, and ```ORDER BY```  

  **Note:** 
  - Use **ASC** to sort in **ascending** order.
  - Use **DESC** to sort in **descending** order.
  - If we leave it blank, **ORDER BY** uses **ASC** by **default**.
  - By default, **ASC** use NULLS LAST
  - By default, **DESCSC** use NULLS FIRST

  **Example:**   
  Sort customers by their first names in ascending order:
  ```PostgreSQL
  SELECT first_name, last_name 
  FROM customer
  ORDER BY first_name ASC;
  ```
  OR, 
   ```PostgreSQL
  SELECT first_name, last_name 
  FROM customer
  ORDER BY first_name;
  ```
  
  ```PostgreSQL
  SELECT store_id, first_name, last_name 
  FROM customer
  ORDER BY store_id DESC, first_name ASC;
  ```
  Find store_id, first name and last name of all the customer ordered in descended value of store_id:
  ```PostgreSQL
  SELECT store_id, first_name, last_name 
  FROM customer
  ORDER BY store_id DESC;
  ```
  
  ```PostgreSQL
  SELECT num 
  FROM sort_demo 
  ORDER BY num NULLS FIRST;
  ```

  ```PostgreSQL
  SELECT num 
  FROM sort_demo 
  ORDER BY num DESC NULLS LAST;
  ```
</details>

<details>
  <summary>Filtering Data : [ WHERE ] : Table </summary>

## WHERE
  | Command | Description |
  | ----- | ----- | 
  | **SELECT** select_list <br> **FROM** table_name <br>**WHERE** condition; | In this syntax, we place the **WHERE** clause right after the FROM clause of the SELECT statement.<br>The **WHERE** clause uses the condition to filter the rows returned from the SELECT clause. <br>The condition is a boolean expression that evaluates to true, false, or unknown. <br>The query returns only rows that satisfy the condition in the WHERE clause.|

  **Execution Order**  
  PostgreSQL evaluates the WHERE clause after the FROM clause but before the SELECT and ORDER BY clause. i.e., from -> where -> select -> order by

  **Example**  
  To find customers with the first name is Jamie:
  ```PostgreSQL
  SELECT customer_id, first_name
  FROM customer
  WHERE first_name = 'Jamie';
  ```

  To find customers whose first name and last names are Jamie and rice:
  ```PostgreSQL
  SELECT customer_id, first_name, last_name
  FROM customer
  WHERE first_name = 'Jamie' AND last_name = 'rice';
  ```
  To find the customers with first names having any of the following Ann, Anne, and Annie:
  ```PostgreSQL
  SELECT customer_id, first_name, last_name
  FROM customer
  WHERE first_name IN ('Ann', 'Anne', 'Annie');
  ```
  Find customers whose first names start with Br and last names are not Motley:
  ```PostgreSQL
  SELECT customer_id, first_name, last_name
  FROM customer
  WHERE first_name like 'Br%'
      AND last_name <> 'Motley';
  ```
</details>

<details>
  <summary>Filtering Data : [ AND | OR ] : Table </summary>

## AND & OR
AND operator – combine two boolean expressions and return true if both expressions evaluate to true. <br>
OR operator – combine two boolean expressions and return false if either expression evaluates to false.

In PostgreSQL, a boolean value can have one of three values: true, false, and null.

PostgreSQL uses ``true``, ``'t'``, ``'true'``, ``'y'``, ``'yes'``, ``'1'`` to represent true and ``false``, ``'f'``, ``'false'``, ``'n'``, ``'no'``, and ``'0'`` to represent false.

A boolean expression is an expression that evaluates to a boolean value.

For example, the expression 1=1 is a boolean expression that evaluates to true:
```PostgreSQL
SELECT 1 = 1 AS result;
```
**Explanation of AND operator:**  
The basic syntax of the AND operator:
```Postgresql
expression1 AND expression2
```
In this syntax, expression1 and expression2 are boolean expressions that evaluate to true, false, or null.

The AND operator returns true only if both expressions are true. It returns false if one of the expressions is false. Otherwise, it returns null.

The following table shows the results of the AND operator when combining true, false, and null.
| true | false | null | ANDed Result |
| --- | --- | --- | --- |
| true | false | null | true |
| false | false | false | false |
| null | false | null | null |

Find the films that have a length greater than 180 and a rental rate less than 1:
```Postgresql
SELECT title, length, rental_rate
FROM film
WHERE length > 180 AND rental_rate < 1;
```
**Explanation of OR Operator:**
The basic syntax of the OR operator:
```Postgresql
expression1 OR expression2
```
In this syntax, expression1 and expression2 are boolean expressions that evaluate to ``true``, ``false``, or ``null``.

The OR operator returns true only if any of the expressions is true. It returns false if both expressions are false. Otherwise, it returns null.

The following table shows the results of the AND operator when combining true, false, and null.
| true | false | null | ANDed Result |
| --- | --- | --- | --- |
| true | true | true | true |
| true | false | null | false |
| true | null | null | null |

Find the films that have a rental rate is 0.99 or 2.99
```PostgreSQL
SELECT title, rental_rate
FROM film
WHERE rental_rate = 0.99 OR rental_rate = 2.99;
```
</details>
<details>
  <summary>Filtering Data : [ LIMIT | OFFSET | FETCH ] : Table </summary>  


## LIMIT & OFFSET 
  | Command | Description |
  | --- | --- |
  | **SELECT** select_list<br>**FROM** table_name <br> **ORDER BY** sort_expression <br> **LIMIT** row_count | The statement returns row_count rows generated by the query. <br> If the row_count is zero, the query returns an empty set. <br> If the row_count is NULL, the query returns the same result set as it does not have the LIMIT clause. |
  | **SELECT** select_list<br>**FROM** table_name <br> **ORDER BY** sort_expression <br> **LIMIT** row_count <br> **OFFSET** row_to_skip; | The statement first skips row_to_skip rows before returning row_count rows generated by the query. <br> If the row_to_skip is zero, the statement will work like it doesn’t have the OFFSET clause. <br> It’s important to note that PostgreSQL evaluates the OFFSET clause before the LIMIT clause. |  

  **Note**
  - PostgreSQL stores rows in a table in an unspecified order, therefore, when we use the LIMIT clause, we should always use the ORDER BY clause to control the row order.
  - If we don’t use the ORDER BY clause, we may get a result set with the rows in an unspecified order.

  **Examples**  
  The following statement uses the **LIMIT** clause to get the first five films sorted by film_id:
  ```PostgreSQL
  SELECT film_id, title, release_year 
  FROM film 
  ORDER BY film_id 
  LIMIT 5;
  ```
  To retrieve 4 films starting from the fourth one ordered by film_id, we can use both LIMIT and OFFSET clauses as follows:
  ```PostgreSQL
  SELECT film_id, title, release_year 
  FROM film 
  ORDER BY film_id 
  LIMIT 4
  OFFSET 3;
  ```

## FETCH
- To skip a certain number of rows and retrieve a specific number of rows, we often use the ``LIMIT`` clause in the ``SELECT`` statement. However, the LIMIT clause is not a ``SQL`` standard.
- To conform with the SQL standard, PostgreSQL supports the ``FETCH`` clause to skip a certain number of rows and then fetch a specific number of rows.

The basic syntax of FETCH clause:
```PostgreSQL
OFFSET row_to_skip { ROW | ROWS }
FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
```
In this syntax:
- First, specify the number of rows to skip (``row_to_skip``) after the ``OFFSET`` keyword. The start is an integer that is zero or positive. It defaults to 0, meaning the query will skip no rows.
  - If the ``row_to_skip`` is higher than the number of rows in the table, the query will return no rows.
- Second, provide the number of rows to retrieve (``row_count``) in the ``FETCH`` clause. The ``row_count`` must be an integer 1 or greater. The ``row_count`` defaults to 1.

**Note:**  
- The ``ROW`` is the synonym for ``ROWS``, ``FIRST`` is the synonym for ``NEXT`` so we can use them interchangeably.
- Because the table stores the rows in an unspecified order, we should always use the ``FETCH`` clause with the ``ORDER BY`` clause to make the order of rows consistent.
- ``OFFSET`` clause must come before the ``FETCH`` clause in SQL:2008. However, ``OFFSET`` and ``FETCH`` clauses can appear in any order in PostgreSQL.

Select first film sorted by titles in ascending order:
```PostgreSQL
SELECT film_id, title
FROM film
ORDER BY title
FETCH FIRST ROW ONLY;
```
Select first five films sorted by titles in ascending order:
```PostgreSQL
SELECT film_id, title
FROM film
ORDER BY title
FETCH FIRST 5 ROWS ONLY;
```
Find the next five films after the first five films sorted by titles:
```PostgreSQL
SELECT film_id, title
FROM film
ORDER BY title
OFFSET 5 ROWS
FETCH FIRST 5 ROWS ONLY;
```
</details>

<details>
  <summary>Filtering Data : [ IN | BETWEEN ] : Table </summary>

### `IN Operator`
The ``IN`` operator allows to check whether a value matches any value in a list of values.

The basic syntax of the ``IN`` operator:

```PostgreSQL
value IN (value1,value2,...)
```
The ``IN`` operator returns ``true`` if the value is equal to any value in the list such as ``value1``, ``value2``, ...

Retrieve information about the film with id 1, 2, and 3:
```PostgreSQL
SELECT film_id, title
FROM film
WHERE film_id IN (1,2,3);
```
Find the actors who have the last name in the list 'Allen', 'Chase', and 'Davis':
```PostgreSQL
SELECT first_name, last_name
FROM actor
WHERE last_name IN ('Allen', 'Chase', 'Davis' )
ORDER BY last_name;
```
Find payments whose payment dates are in a list of dates: 2007-02-15 and 2007-02-16:
```PostgreSQL
SELECT payment_id, amount, DATE(payment_date)
FROM payment
WHERE payment_date::date IN ('2007-02-15', '2007-02-16');
```

### `BETWEEN Operator`
The `BETWEEN` operator allows to check if a value falls within a range of values.
| Command | Description |
| --- | --- | 
| value ``BETWEEN`` low ``AND`` high | If the ``value`` is greater than or equal to the ``low`` value and less than or equal to the ``high`` value, the ``BETWEEN`` operator returns true; <br> Otherwise, it returns false. <br> The syntax can be re-written as ``value`` >= low ``AND`` ``value`` <= high | 

**Note:**  
- When using BETWEEN operator with dates that also include timestamp information, we need to pay careful attention to using BETWEEN versus <=, >= comparison operators, due to the fact that a datetime starts at 0:00. Later on we will study more specific methods for datetime information types.
Retrieve payments with payment_id is between 17503 and 17505:
```PostgreSQL
SELECT payment_id, amount
FROM payment
WHERE payment_id BETWEEN 17503 AND 17505;
```
Find payments with payment_id is not the range between 17503 and 17505:
```PostgreSQL
SELECT payment_id, amount
FROM payment
WHERE payment_id NOT BETWEEN 17503 AND 17505;
```
Find payments whose payment dates are between 2007-02-15 and 2007-02-20 and amount more than 10:
```PostgreSQL
SELECT payment_id, amount, payment_date
FROM payment
WHERE payment_date BETWEEN '2007-02-15' AND '2007-02-20'
  AND amount > 10
ORDER BY payment_date;
```
</details>
<details>
  <summary>Filtering Data : [ LIKE | ILIKE] : Table </summary>

## LIKE Operator
The ``LIKE`` (and ``ILIKE``) operator allows us to perform pattern matching against string data with the use of wildcard characters:
- Percent % 
  - Matches any sequence of characters
- Underscore _
  - Matches any single character
**Syntax**  
```PostgresSQL
value LIKE pattern
value NOT LIKE pattern
value ILIKE pattern
VALUE NOT ILIKE pattern
```
**Note:**
- ``LIKE`` is case-sensitive
- ``ILIKE`` is case-insensitive  

**Some Pattern**
- "a%" = words ``start`` with a
- "%a" = words ``end`` with a
- "%test%"= words that have ``test`` in any position
- "_r%" = words that have "r" in the 2nd position from beginning
- "a_%" = words start with 'a' and it at least have 1 char after 'a'.
- "a_ _%" = words start with 'a' and it at least have 2 char after 'a'.
- "a%o"= Strings that start with "a" and ends with "o"
- [_ _ _] matches any string of exactly three characters.
- [_ _ _ %] matches any string of at least three characters.

Find customers whose first names contain the string ``er`` :
```PostgreSQL
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '%er%'
ORDER BY first_name;
```

### What if we want to match the character % or _ itself:
Then, The solution is use ESCAPE option.

For example, a column of table contains info like:  
The rents are now 10% higher than last month    


To match the % or _ itself, ESCAPE should be used.

(Read from o)

</details>

<details>
  <summary>Filtering Data : [ IS NULL ] : Table </summary>

## NULL
NULL means missing information or not applicable. NULL is not a value, therefore, we cannot compare it with other values like numbers or strings.

The comparison of NULL with a value will always result in NULL. Additionally, NULL is not equal to NULL so the following expression returns NULL:
```PostgreSQL
SELECT null = null AS result;
```

## IS NULL
To check if a value is NULL or not, we cannot use the equal to (``=``) or not equal to (``<>``) operators.`Instead, we use ``IS NULL`` or ``IS NOT NULL`` operator
```PostgreSQL
value IS NULL
---------------
value IS NOT NULL
```
- The IS ``NULL`` operator returns true if the ``value`` is NULL or false otherwise.
- The ``IS NOT NULL`` operator returns true if the ``value`` is not NULL or false otherwise.

Find the addresses from the address table that the address2 column contains NULL:
```PostgreSQL
SELECT address, address2
FROM address
WHERE address2 IS NULL;
```
Retrieve the address that has the address2 not NULL:
```PostgreSQL
SELECT address, address2
FROM address
WHERE address2 IS NOT NULL;
```
</details>

<details>
  <summary>Joining Tables : [ TABLE ALIAS | INNER JOIN | FULL OUTER JOIN | LEFT JOIN | RIGHT JOIN | SELF JOIN | CROSS JOIN | NATURAL JOIN] </summary>

## Table Alias
A table alias is a feature in SQL that allows to assign a temporary name to a table during the execution of a query.
| Command | Description | 
| --- | --- | 
| table_name AS alias_name | It will renamed the name of the table "table_name" to "alias_name" |

**Note:**  
- The AS keyword is optional, meaning that we can omit it like this: ```table_name alias_name```

To retrieve five titles from the ``film`` table:
```PostgreSQL
SELECT f.title
FROM film AS f
ORDER BY f.title
LIMIT 5;
```
## Inner Join 

| Command | Description |
| --- | --- | 
| **SELECT** select_list <br>**FROM** TableA **INNER JOIN** TableB <br>**ON** TableA.column_name = TableB.column_name; | Inner join produces only the set of records that match in both TableA and TableB based on the column name. |

![inner_join](images/inner_join.png)  
**Note:**   
- To make the query shorter, we can use table aliases:
  ```PostgreSQL
  SELECT
    select_list
  FROM
    table1 t1 
      INNER JOIN table2 t2 ON t1.column_name = t2.column_name;
  ```
- If the columns for matching share the same name, we can use the USING syntax:
  ```PostgreSQL
  SELECT
    select_list
  FROM
    table1 t1 INNER JOIN table2 t2 USING(column_name);
  ```
**Example:**  
A customer walks in and is a huge fan of the actor "Nick Wahlberg" and wants to know which movies he is in. <br>
Get a list of all the movies "Nick Wahlberg" has been in.
```PostgreSQL
select
	title, first_name, last_name 
from 
	film_actor
		inner join actor on	actor.actor_id = film_actor.actor_id
		inner join film on film.film_id = film_actor.film_id
where
	first_name = 'Nick'
	and last_name = 'Wahlberg';
```
California sales tax laws have changed and we need to alert our customers to this through email. <br> 
What are the emails of the customers who live in California?
```PostgreSQL
select district, email
from customer inner join address
	on customer.address_id = address.address_id
where district = 'California';
```
## Full Outer Join

| Command | Description |
| --- | --- | 
| **SELECT** select_list <br>**FROM** TableA **FULL OUTER JOIN** TableB <br>**ON** TableA.column_name = TableB.column_name;  | Full outer join produces the set of all records in Table A and Table B,<br>with matching records from both sides where available.<br>If there is no match, the missing side will contain null. |
![full_outer_join](images/full_outer_join.png) 

To produce the set of records unique to Table A and Table B, we perform the same full outer join, then exclude the records we don't want from both sides via a where clause.
```PostgreSQL
SELECT * FROM TableA
FULL OUTER JOIN TableB
ON TableA.name = TableB.name
WHERE TableA.id IS null
OR TableB.id IS null
```
![unique](images/unique_join.png)  

```PostgreSQL
CREATE TABLE departments (
  department_id serial PRIMARY KEY,
  department_name VARCHAR (255) NOT NULL
);
CREATE TABLE employees (
  employee_id serial PRIMARY KEY,
  employee_name VARCHAR (255),
  department_id INTEGER
);
```
Based on this, find the department that does not have any employees:
```PostgreSQL
select
  department_id
from
  departments dept
    full outer join employees emp on dept.department_id = emp.department_id
where
  emp.employee_name is null
```

## Left Outer Join

| Command | Description |
| --- | --- | 
| **SELECT** select_list <br>**FROM** TableA **LEFT OUTER JOIN** TableB <br>**ON** TableA.column_name = TableB.column_name;  | Left outer join produces a complete set of records from Table A,<br> with the matching records (where available) in Table B.<br>If there is no match, the right side will contain null. |
![left_outer_join](images/left_outer_join.png)  

**Note:**
- If the columns for joining two tables have the same name, we can use the USING syntax:
  ```PostgreSQL
  SELECT
    select_list
  FROM
    table1
      LEFT JOIN table2 USING (column_name);
  ------------------------------------------------------------
  SELECT
  f.film_id,
  f.title,
  i.inventory_id
  FROM
    film f
      LEFT JOIN inventory i USING (film_id)
  ORDER BY
    i.inventory_id;
  ```

To produce the set of records only in Table A, but not in Table B, we perform the same left outer join, then exclude the records we don't want from the right side via a where clause.
```PostgreSQL
SELECT * FROM TableA
LEFT OUTER JOIN TableB
ON TableA.name = TableB.name
WHERE TableB.id IS null
```
![left_outer_left](images/left_outer_join2.png) 

Identify the films that are not present in the inventory:
```PostgreSQL
SELECT
  f.film_id,
  f.title,
  i.inventory_id
FROM
  film f
  LEFT JOIN inventory i USING (film_id)
WHERE
  i.film_id IS NULL
ORDER BY
  f.title;
```

## Right Outer Join

| Command | Description |
| --- | --- | 
| **SELECT** select_list <br>**FROM** TableA **RIGHT OUTER JOIN** TableB <br>**ON** TableA.column_name = TableB.column_name;  | Right outer join produces a complete set of records from Table B(the right table),<br> with the matching records (where available) in Table A.<br>If there is no match, the left side will contain null. |
![right_outer_join](images/right_outer_join.jpg)  

**Note:**
- If the columns for joining two tables have the same name, we can use the USING syntax:
  ```PostgreSQL
  SELECT
    select_list
  FROM
    table1
      RIGHT JOIN table2 USING (column_name);
  ------------------------------------------------------------
  SELECT
  f.film_id,
  f.title,
  i.inventory_id
  FROM
    film f
      RIGHT JOIN inventory i USING (film_id)
  ORDER BY
    i.inventory_id;
  ```

To produce the set of records only in Table B, but not in Table A, we perform the same right outer join, then exclude the records we don't want from the left side via a where clause.
```PostgreSQL
SELECT * FROM TableA
RIGHT OUTER JOIN TableB
ON TableA.name = TableB.name
WHERE TableA.id IS null
```
![right_outer_right](images/right_outer_join2.jpg) 

## Self Join
A self-join is a regular join that joins a table to itself. In practice, we typically use a self-join to query hierarchical data or to compare rows within the same table.

Syntax:  
```PostgreSQL
SELECT
  select_list
FROM
  table_name t1
    INNER JOIN table_name t2 ON join_predicate;
-------------------------------------------------alternative
SELECT
  select_list
FROM
  table_name t1
    LEFT JOIN table_name t2 ON join_predicate;
```

Find all pairs of films that have the same length:
```PostgreSQL
select
  f1.title,
  f2.title,
  f1.length
from 
  film f1
    inner join film f2 on f1.length = f2.length and f1.film_id > f2.film_id
```

## Cross Join
A cross-join allows to join two tables by combining each row from the first table with every row from the second table, resulting in a complete combination of all rows.

**Note**
- A cross-join produces the cartesian product of rows in two tables.
- If table1 has ``n`` rows and table2 has ``m`` rows, the CROSS JOIN will return a result set that has ``nxm`` rows.

**Syntax:**  
```PostgreSQL
SELECT
  select_list
FROM
  table1
    CROSS JOIN table2;
--------------------alternative
SELECT
  select_list
FROM
  table1,table2;
```

The query ```SELECT * FROM T1 CROSS JOIN T2;``` will produce the following output:  
![cross_join](images/cross_join.jpeg)

## Natural Join
A natural join is a join that creates an implicit join based on the same column names in the joined tables.
The syntax for natural join:
```PostgreSQL
SELECT select_list
FROM table1
NATURAL [INNER, LEFT, RIGHT] JOIN table2;
```

In this syntax:
- First, specify columns from the tables from which we want to retrieve data in the select_list in the SELECT clause.
- Second, provide the main table (table1) from which we want to retrieve data.
- Third, specify the table (table2) that we want to join with the main table, in the NATURAL JOIN clause.

**Note**
- A natural join can be an inner join, left join, or right join. If we do not specify an explicit join, PostgreSQL will use the INNER JOIN by default.
- The convenience of the NATURAL JOIN is that it does not require to specify the condition in the join clause because it uses an implicit condition based on the equality of the common columns.

The equivalent of NATURAL JOIN is as follows:
```PostgreSQL
SELECT select_list
FROM table1
[INNER, LEFT, RIGHT] JOIN table2
   ON table1.column_name = table2.column;
```
**Inner Join**
```PostgreSQL
SELECT select_list
FROM table1
NATURAL INNER JOIN table2;
------------------------------equivalent to
SELECT select_list
FROM table1
INNER JOIN table2 USING (column_name);
```

**Left Join**
```PostgreSQL
SELECT select_list
FROM table1
NATURAL LEFT JOIN table2;
------------------------------equivalent to
SELECT select_list
FROM table1
LEFT JOIN table2 USING (column_name);
```

**Right Join**
```PostgreSQL
SELECT select_list
FROM table1
NATURAL RIGHT JOIN table2;
------------------------------equivalent to
SELECT select_list
FROM table1
RIGHT JOIN table2 USING (column_name);
```

</details>
<details>
  <summary>Aggregate Functions [ COUNT | MAX | MIN | SUM | AVG ] : Table </summary>

  | Function | Command | Description |
  | --- | --- | --- |
  |**COUNT()** | **COUNT(column_name)** or **COUNT(*)** <br> <br>**SELECT** **COUNT**(column_name) <br> **FROM** table_name;| Returns the number of records returned by a select query.<br>**Note:** NULL values are not counted.|
  |**AVG()** | **SELECT** **AVG**(column_name) <br> **FROM** table_name ; | Return the average value for the given column.|
  |**MIN()** | **SELECT** **MIN**(column_name) <br> **FROM** table_name ;| Returns the minimum value from the records.|
  |**MAX()** | **SELECT** **MAX**(column_name) <br> **FROM** table_name ;| Returns the maximum value from the records.|
  |**SUM()** | **SELECT** **SUM**(column_name) <br> **FROM** table_name | Returns the total sum of the specified column |

Calculate the average replacement cost of all films:
```PostgreSQL
SELECT ROUND(AVG(replacement_cost),2) AS avg_replacement_cost
FROM film
```
Count number of films:
```PostgreSQL
SELECT COUNT(*)
FROM film
```
Find maximum replacement cost of films:
```PostgreSQL
SELECT MAX(replacement_cost) AS max_replacement_cost
FROM film
```
Find the films that have the maximum replacement cost:
```PostgreSQL
SELECT film_id, title
FROM film
WHERE
  replacement_cost = (
    SELECT MAX(replacement_cost)
    FROM film
  )
ORDER BY title
```
Find minimum replacement cost of films:
```PostgreSQL
SELECT MIN(replacement_cost) AS min_replacement_cost
FROM film
```
Find the total length of films grouped by film’s rating:
```PostgreSQL
SELECT rating, SUM(length)
FROM film
GROUP BY rating
ORDER BY rating;
```
</details>

<details>
  <summary> Operators [ Logical | Arithmetic | Comparison ] </summary>

### Logical Operators (AND, OR)

### Arithmetic Operators(+,-,*,/,%)

  | Command    | Description |
  | ----------- | ----------- |
  | + | Addition |
  | - | Subtraction |
  | * | Multiplication |
  | / | Division |
  | % | Modulo |
  
### Comparison Operators(=, >, <, >=, <=, <>)

  | Command    | Description |
  | ----------- | ----------- |
  | = | Equal |
  | > | Greater than |
  | < | Less than	|
  | >= | Greater than or equal	|
  | <= |	Less than or equal |  
  | <>|	Not equal.<br>**Note**: In some versions of SQL this operator may be written as !=	|

</details>



<details>
  <summary>Grouping Data : [ GROUP BY | HAVING ] </summary>

## GROUP BY
- Divide rows of a result set into groups and optionally apply an aggregate function to each group.

The basic syntax of the ``GROUP BY`` clause:
```PostgreSQL
SELECT
  column1,
  column2,
  ...,
  aggregate_function(column3)
FROM
  table_name
GROUP BY
  column1,
  column2,
  ...;
```
In this syntax,
- First, select the columns that we want to group such as ``column1`` and ``column2``, and column that we want to apply an aggregate function (``column3``).
- Second, list the columns that we want to group in the ``GROUP BY`` clause.

**Execution Order:** PostgreSQL evaluates the ``GROUP BY`` clause after the ``FROM`` and ``WHERE`` clauses and before the ``HAVING`` ``SELECT``, ``DISTINCT``, ``ORDER BY`` and ``LIMIT`` clauses.

**Example:**  
Retrieve the customer_id from the payment table:
```PostgreSQL
SELECT customer_id
FROM customer
GROUP BY customer_id
ORDER BY customer_id;
```
Retrieve the total payment paid by each customer:
```PostgreSQL
SELECT customer_id, SUM(amount)
FROM payment
GROUP BY customer_id
ORDER BY customer_id;
```
Retrieve the total payment for each customer and display the customer name and amount:
```PostgreSQL
SELECT first_name || ' ' || last_name AS full_name, SUM(amount)
FROM
  payment
    INNER JOIN customer ON payment.customer_id = customer.customer_id
GROUP BY full_name
ORDER BY SUM(amount) DESC;

```
Count the number of payments processed by each staff:
```PostgreSQL
SELECT staff_id, COUNT(*)
FROM
  payment
GROUP BY staff_id;
```

## HAVING
HAVING clause specifies a search condition for a group or an aggregate.

The basic syntax is as follows:
```PostgreSQL
SELECT 
  column1,
  aggregate_function(column2)
FROM
  table_name
GROUP BY 
  column1
HAVING
  condition;
```
In this syntax:
- First, the ``GROUP BY`` clause groups rows into groups by the values in the ``column1``.
- Then, the ``HAVING`` clause filters the groups based on the ``condition``.

**Note:**
- Besides the GROUP BY clause, we can also include other clauses such as JOIN and LIMIT in the statement that uses the HAVING clause.

### HAVING vs WHERE
- The ``WHERE`` clause filters the rows based on a specified condition whereas the ``HAVING`` clause filter groups of rows according to a specified condition.
- In other words, we can apply the condition in the ``WHERE`` clause to the rows while we need to apply the condition in the ``HAVING`` clause to the groups of rows.

Find the customers who have been spending more than 200:
```PostgreSQL
SELECT
  customer_id, SUM(amount)
FROM 
  payment
GROUP BY 
  customer_id
HAVING SUM(amount) > 200
```

Find the stores that has more than 300 customers:
```PostgreSQL
SELECT
  store_id, COUNT(customer_id)
FROM
  customer
GROUP BY
  store_id
HAVING COUNT(customer_id) > 300
```
We are launching a platinum service for our most loyal customers. We will assign platinum status to customers that have had 40 or more transaction payments. What customer_ids are eligible for platinum status?
```PostgreSQL
SELECT
  customer_id, COUNT(amount)
FROM
  payment
GROUP BY
  customer_id
HAVING COUNT(amount) >= 40
```
What are the customer ids of customers who have spent more than $100 in payment transactions with our staff_id member 2?
```PostgreSQL
SELECT
  customer_id, staff_id, SUM(amount)
FROM
  payment
WHERE 
  staff_id = 2
GROUP BY
  customer_id
HAVING SUM(amount) > 100
```
</details>

<details>
  <summary>Grouping Data : [ GROUPING SETS | CUBE | ROLLUP ] </summary>

## GROUPING SETS
The ``GROUPING SETS`` feature allows users to generate result sets that are equivalent to those produced by the ``UNION ALL`` of multiple ``GROUP BY`` clauses. This feature is highly useful for creating complex reports with multiple levels of aggregation in a single query.

The basic syntax is as follows:
```PostgreSQL
SELECT
    column1,
    column2,
    aggregate_function(column3)
FROM
    table_name
GROUP BY
    GROUPING SETS (
        (column1, column2),
        (column1),
        (column2),
        ()
);
```
A table with following properties:
```PostgreSQL
CREATE TABLE sales (
    brand VARCHAR NOT NULL,
    segment VARCHAR NOT NULL,
    quantity INT NOT NULL,
    PRIMARY KEY (brand, segment)
);
```
Let's think of the following scenario:
- return the number of products sold by brand and segment
- return the number of products sold by a brand
- return the number of products sold by segment
- return the number of products sold for all brands and segments.

We can find each of the result by running ``group by`` statements separately. What if we need to run them in a single query? Then may be we think to combine them using ``UNION ALL``. But it raises performances issues. Then the solution comes with ``GROUPING SETS`` clause.
Let's see both commands:
```PostgreSQL
SELECT brand, segment, SUM (quantity)
FROM sales
GROUP BY brand, segment

UNION ALL

SELECT brand, NULL, SUM (quantity)
FROM sales
GROUP BY brand

UNION ALL

SELECT NULL, segment, SUM (quantity)
FROM sales
GROUP BY segment

UNION ALL

SELECT NULL, NULL, SUM (quantity)
FROM sales;

---------------------------------------equivalent to 
SELECT brand, segment, SUM (quantity)
FROM sales
GROUP BY
    GROUPING SETS (
        (brand, segment),
        (brand),
        (segment),
        ()
    );
``` 
**To Do:** Read more about grouping function

## CUBE
PostgreSQL ``CUBE`` is a subclause of the ``GROUP BY`` clause. The ``CUBE`` allows to generate multiple grouping sets.

Syntax:
```PostgreSQL
SELECT c1, c2, c3, aggregate (c4)
FROM table_name
GROUP BY
    CUBE (c1, c2, c3);
```
Here, CUBE(c1,c2,c3) is equivalent to the following:
```PostgreSQL
CUBE (c1, c2, c3);
-------------------------equivalent to
GROUPING SETS (
    (c1,c2,c3),
    (c1,c2),
    (c1,c3),
    (c2,c3),
    (c1),
    (c2),
    (c3),
    ()
 )
```
The above example can be run as follows:
```PostgreSQL
SELECT brand, segment, SUM (quantity)
FROM sales
GROUP BY
    CUBE (brand, segment)
```

## ROLLUP
- The PostgreSQL ``ROLLUP`` is a subclause of the ``GROUP BY`` clause that offers a shorthand for defining multiple grouping sets.
- Different from the ``CUBE`` subclause, ``ROLLUP`` does not generate all possible grouping sets based on the specified columns. It just makes a subset of those.
For example, the ``CUBE (c1,c2,c3)`` makes all eight possible grouping sets:
```PostgreSQL
(c1, c2, c3)
(c1, c2)
(c2, c3)
(c1,c3)
(c1)
(c2)
(c3)
()
```
However, the ``ROLLUP(c1,c2,c3)`` generates only four grouping sets, assuming the hierarchy c1 > c2 > c3 as follows:
```PostgreSQL
(c1, c2, c3)
(c1, c2)
(c1)
()
```
</details>

<details>
  <summary>Set Operations : [ Union | INTERSECT | EXCEPT] </summary>

## UNION
The ``UNION`` operator allows to combine the result sets of two or more ``SELECT`` statements into a single result set.

The basic syntax of ``UNION`` is as follows:
```PostgreSQL
SELECT select_list
FROM A
UNION
SELECT select_list
FROM B
```
In this syntax,
- The number and the order of the columns in the select list of both queries must be the same
- The data types of the columns in select lists of the queries must be compatible

**Note:**  
- The UNION operator removes all duplicate rows from the combined data set.
- To retain the duplicate rows, we need to use the UNION ALL instead
The syntax of the ``UNION ALL`` operator:
```PostgreSQL
SELECT select_list
FROM A
UNION ALL
SELECT select_list
FROM B
```

### Examples
```PostgreSQL
CREATE TABLE top_rated_films(
  title VARCHAR NOT NULL,
  release_year SMALLINT
);

CREATE TABLE most_popular_films(
  title VARCHAR NOT NULL,
  release_year SMALLINT
);

INSERT INTO top_rated_films(title, release_year)
VALUES
   ('The Shawshank Redemption', 1994),
   ('The Godfather', 1972),
   ('The Dark Knight', 2008),
   ('12 Angry Men', 1957);

INSERT INTO most_popular_films(title, release_year)
VALUES
  ('An American Pickle', 2020),
  ('The Godfather', 1972),
  ('The Dark Knight', 2008),
  ('Greyhound', 2020);
```

Retrieve all the films those are most popular or top rated films:
```PostgreSQL
------------------------------with duplicates
SELECT * FROM top_rated_films
UNION
SELECT * FROM most_popular_films
------------------------------without duplicates
SELECT * FROM top_rated_films
UNION ALL
SELECT * FROM most_popular_films
```
Retrieve all the films those are most popular or top rated films sorted by title:
```PostgreSQL
------------------------------with duplicates
SELECT * FROM top_rated_films
UNION
SELECT * FROM most_popular_films
ORDER BY title
------------------------------without duplicates
SELECT * FROM top_rated_films
UNION ALL
SELECT * FROM most_popular_films
ORDER BY title
```
**Note:**
- ORDER BY in UNION must be placed at the last

## INTERSECT
``INTERSECT`` operator combines result sets of two ``SELECT`` statements into a single result set. The ``INTERSECT`` operator returns a result set containing rows available in both results sets.

The basic syntax of ``INTERSECT`` is as follows:
```PostgreSQL
SELECT select_list
FROM A
INTERSECT
SELECT select_list
FROM B
-------------------with order by
SELECT select_list
FROM A
INTERSECT
SELECT select_list
FROM B
ORDER BY sort_expression
```
In this syntax,
- The number and the order of the columns in the select list of both queries must be the same
- The data types of the columns in select lists of the queries must be compatible

Retrieve all the films those are most popular and top rated films:
```PostgreSQL
SELECT * FROM top_rated_films
INTERSECT
SELECT * FROM most_popular_films
```
Retrieve all the films those are most popular and top rated films sorted by title:
```PostgreSQL
SELECT * FROM top_rated_films
INTERSECT
SELECT * FROM most_popular_films
ORDER BY title
```
## EXCEPT
The ``EXCEPT`` operator returns rows by comparing the result sets of two or more queries. The ``EXCEPT`` operator returns distinct rows from the first (left) query that are not in the second (right) query. 

The basic syntax of ``EXCEPT`` is as follows:
```PostgreSQL
SELECT select_list
FROM A
EXCEPT
SELECT select_list
FROM B
-------------------with order by
SELECT select_list
FROM A
EXCEPT
SELECT select_list
FROM B
ORDER BY sort_expression
```
In this syntax,
- The number and the order of the columns in the select list of both queries must be the same
- The data types of the columns in select lists of the queries must be compatible

Find the top-rated films that are not popular:
```PostgreSQL
SELECT * FROM top_rated_films
EXCEPT
SELECT * FROm most_popular_films
```

</details>

<details>
  <summary>Sub-query : [ sub-query | ANY/SOME | ALL | EXISTS ]</summary>

## Sub-query
A sub-query is a query nested within another query. A sub-query is also known as an inner query or nested query. A sub-query can be useful for retrieving data that will be used by the main query as a condition for further data selection.

The basic syntax of the sub-query is as follows:
```PostgreSQL
SELECT
  select_list
FROM 
  table1
WHERE 
  columnA operator (
    SELECT 
      columnB
    FROM 
      table2
    WHERE
      condition
  );
```
In this syntax: 
- the sub-query is enclosed within parentheses and is executed first
- The main query will use the result of the sub-query to filter data in the ``WHERE`` clause.

Retrieve the all the cities of the country United States:
```PostgreSQL
SELECT
  city
FROM 
  city
WHERE 
  country_id = (
    SELECT 
      country_id
    FROM
      country
    WHERE
      country = 'United States';
  )
ORDER BY
  city;
```
Find the titles of the film with the category Action:
```PostgreSQL
SELECT
  film_id, title
FROM 
  film
WHERE 
  film_id IN (
    SELECT 
      film_id
    FROM
      film_category
	  	  inner join category on film_category.category_id = category.category_id
	  WHERE category.name = 'Action'
  )
ORDER BY
	film_id;
```

```PostgreSQL
SELECT 
	film_id, title, length, rating
FROM film AS f
WHERE length > (
	SELECT  AVG(length)
	FROM film
	WHERE rating = f.rating
)
```
## ANY/SOME
The PostgreSQL ANY operator compares a value with a set of values returned by a sub-query. It is commonly used in combination with comparison operators such as =, <, >, <=, >=, and <>.  

The basic syntax for ANY is as follows:
```PostgreSQL
expression operator ANY(sub-query)
```
In this syntax:
- ``expression`` is a value that we want to compare.
- ``operator`` is a comparison operator including =, <, >, <=, >=, and <>.
- ``sub-query`` is a sub-query that returns a set of values to compare against. It must return exactly one column.

**Note**
- The ``ANY`` operator returns ``true`` if the comparison returns ``true`` for at least one of the values in the set, and ``false`` otherwise.
- If the sub-query returns an empty set, the result of ``ANY`` comparison is always ``true``.


**Note:**
- ``SOME`` is a synonym for ``ANY``, which means that we can use them interchangeably.

## ALL
``ALL`` operator allows we to compare a value with all values in a set returned by a sub-query.
```PostgreSQL
expression operator ALL(sub-query)
```
In this syntax:
- The ``ALL`` operator must be preceded by a comparison operator such as equal (=), not equal (<>), greater than (>), greater than or equal to (>=), less than (<), and less than or equal to (<=).
- The ``ALL`` operator must be followed by a sub-query which also must be surrounded by the parentheses.

**Note:**
- If the sub-query returns a non-empty result set, the ALL operator works as follows:
  - value > ALL (sub-query) returns true if the value is greater than the biggest value returned by the sub-query
  - value >= ALL (sub-query) returns true if the value is greater than or equal to the biggest value returned by the sub-query.
  - value < ALL (sub-query) returns true if the value is less than the smallest value returned by the sub-query.
  - value <= ALL (sub-query) returns true if the value is less than or equal to the smallest value returned by the sub-query.
  - value = ALL (sub-query) returns true if the value equals every value returned by the sub-query.
  - value != ALL (sub-query) returns true if the value does not equal any value returned by the sub-query.
- If the sub-query returns no row, then the ALL operator always evaluates to true.


## EXISTS
The EXISTS operator is a boolean operator that checks the existence of rows in a sub-query.

Here’s the basic syntax of the EXISTS operator:
```PostgreSQL
EXISTS (sub-query)
```
```PostgreSQL
SELECT
  select_list
FROM
  table1
WHERE
  EXISTS(
    SELECT
      select_list
    FROM
      table2
    WHERE
      condition
  );
```
**Note:**
- If the sub-query returns at least one row, the EXISTS operator returns true. If the sub-query returns no row, the EXISTS returns false.
- if the sub-query returns NULL, the EXISTS operator returns true.
- The result of EXISTS operator depends on whether any row is returned by the sub-query, and not on the row contents. Therefore, columns that appear in the select_list of the sub-query are not important.

Check if the payment value is zero exists in the payment table:
```PostgreSQL
SELECT 
  EXISTS (
    SELECT 1
    FROM payment
    WHERE amount = 0
  );
```
Find customers who have paid at least one rental with an amount greater than 11:
```PostgreSQL
SELECT first_name, last_name
FROM customer AS c
WHERE EXISTS(
  SELECT 1
  FROM payment AS p
  WHERE c.customer_id = p.customer_id
    AND amount > 11
)
ORDER BY
  first_name,
  last_name;
```
Find customers who have not made any payment more than 11.
```PostgreSQL
SELECT first_name, last_name
FROM customer AS c
WHERE NOT EXISTS(
  SELECT 1
  FROM payment AS p
  WHERE c.customer_id = p.customer_id
    AND amount > 11
)
ORDER BY
  first_name,
  last_name;
```
</details>

<details>
  <summary>Common Table Expressions : [ CTE ]</summary>

## CTE
- A common table expression (CTE) allows to create a temporary result set within a query.
- A CTE helps to enhance the readability of a complex query by breaking it down into smaller and more reusable parts

The basic syntax is as follows:
```PostgreSQL
WITH cte_name (column1, column2, ...) AS (
    -- CTE query
    SELECT ...
)
-- Main query using the CTE
SELECT ...
FROM cte_name;
```
The following example uses a common table expression (CTE) to select the title and length of films in the 'Action' category and returns all the columns of the CTE:
```PostgreSQL
WITH action_films AS (
  SELECT
    f.title,
    f.length
  FROM
    film f
    INNER JOIN film_category fc USING (film_id)
    INNER JOIN category c USING(category_id)
  WHERE
    c.name = 'Action'
)
SELECT * FROM action_films;
```
The following example uses multiple CTEs to calculate various statistics related to films and customers:
```PostgreSQL
WITH film_stats AS (
    -- CTE 1: Calculate film statistics
    SELECT
        AVG(rental_rate) AS avg_rental_rate,
        MAX(length) AS max_length,
        MIN(length) AS min_length
    FROM film
),
customer_stats AS (
    -- CTE 2: Calculate customer statistics
    SELECT
        COUNT(DISTINCT customer_id) AS total_customers,
        SUM(amount) AS total_payments
    FROM payment
)
-- Main query using the CTEs
SELECT
    ROUND((SELECT avg_rental_rate FROM film_stats), 2) AS avg_film_rental_rate,
    (SELECT max_length FROM film_stats) AS max_film_length,
    (SELECT min_length FROM film_stats) AS min_film_length,
    (SELECT total_customers FROM customer_stats) AS total_customers,
    (SELECT total_payments FROM customer_stats) AS total_payments;
```
**TO Do:** Read more about ``Recursive CTE``
</details>

<details>
<summary> Modifying Data : [ INSERT INTO ] : Table </summary>
  
## INSERT INTO
  
| Command    | Description |
| ----------- | ----------- |  
| **INSERT INTO** table_name <br>VALUES (value1,value2,value3,... ...),<br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...), <br>... ... ; | To add values for all the columns of the table.<br><br> No need to specify the column names in the SQL syntax. <br><br> But need to make sure the order of the values is in the same order as the columns in the table.|
| **INSERT INTO** <br> table_name (column1, column2, column3,... ...) VALUES <br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...) , <br>(value1,value2,value3,... ...),<br> ... ... ;|To insert Data Only in Specified Columns.|
| **INSERT INTO** <br> table1(column1, column2, …)<br> **VALUES** (value1, value2, …) <br>**RETURNING** *; | The ``INSERT`` statement has an optional ``RETURNING`` clause that returns the information of the inserted row. |

### A table with following properties:
```PostgreSQL
CREATE TABLE links (
  id SERIAL PRIMARY KEY,
  url VARCHAR(255) NOT NULL,
  name VARCHAR(255) NOT NULL,
  description VARCHAR (255),
  last_update DATE
);
```
Insert the following information with url: https://github.com/RuhulAminSharif/SQL-Commands/ and name : SQL Commands
```PostgreSQL
INSERT INTO links(url, name)
VALUES ('https://github.com/RuhulAminSharif/SQL-Commands/',  'SQL Commands')
```
Insert the following information with url : http://www.oreilly.com, and name: O'Reilly Media
```PostgreSQL
INSERT INTO links(url, name)
VALUES ('http://www.oreilly.com',  'O''Reilly Media')
```
Another insertion example:
```PostgreSQL
INSERT INTO links (url, name, last_update)
VALUES('https://www.google.com','Google','2013-06-01');
```
### A table with following properties:
```PostgreSQL
CREATE TABLE contacts (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(384) NOT NULL UNIQUE
);
```
Inserting multiple rows:
```PostgreSQL
INSERT INTO contacts (first_name, last_name, email)
VALUES
    ('John', 'Doe', 'johndoe@gmail.com'),
    ('Jane', 'Smith', 'janesmith@gmail.com'),
```

</details>

<details>
  <summary>Modifying Data : [ UPDATE | UPDATE JOIN ] : Table</summary>

## UPDATE
- The PostgreSQL UPDATE statement allows to update data in one or more columns of one or more rows in a table.  

Basic Syntax:
```PostgreSQL
UPDATE table_name
SET column1 = value1,
    column2 = value2,
    ...
WHERE condition;
```
In this syntax:
- First, specify the name of the table that we want to update data after the UPDATE keyword.
- Second, specify columns and their new values after SET keyword. The columns that do not appear in the SET clause retain their original values.
- Third, determine which rows to update in the condition of the WHERE clause. (optional clause)
Some examples:
```PostgreSQL
UPDATE account
SET last_login = CURRENT_TIMESTAMP
    WHERE last_login IS NULL;
```
Reset everything:
```PostgreSQL
UPDATE account
SET last_login = CURRENT_TIMESTAMP
```
Set based on another column
```PostgreSQL
UPDATE account
SET last_login = created_on
```
Returning affected rows:
```PostgreSQL
UPDATE account
SET last_login = created_on
RETURNING account_id,last_login
```

## UPDATE JOIN
Basic syntax:
```PostgreSQL
UPDATE table1
SET table1.c1 = new_value
FROM table2
WHERE table1.c2 = table2.c2;
```
See the following two tables:  
![no alt text](db_design/image.png)
**Task:** You have to calculate the net price of every product based on the discount of the product segment
```PostgreSQL
UPDATE product
SET net_price = price - price * discount
FROM product_segment
WHERE product.segment_id = product_segment.id;
```
</details>

<details>
  <summary>Modifying Data : [ DELETE | DELETE JOIN ] : Table </summary>

## DELETE 
The PostgreSQL ``DELETE`` statement allows to delete one or more rows from a table.  

The basic syntax of DELETE statement is as follows:
```PostgreSQL
DELETE FROM table_name
WHERE condition;
```
In this syntax:
- First, specify the name (table_name) of the table from which we want to delete data after the DELETE FROM keywords.
- Second, specify a condition in the WHERE clause to determine which rows to delete.

**Note:**
- The WHERE clause is optional. If we omit the WHERE clause, the DELETE statement will delete all rows in the table.
- The DELETE statement returns the number of rows deleted. It returns zero if the DELETE statement did not delete any row.


To return the deleted row(s) to the client, we use the RETURNING clause as follows:
```PostgreSQL
DELETE FROM table_name
WHERE condition
RETURNING (select_list | *)
```
A table with following properties:
```PostgreSQL
CREATE TABLE todos (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    completed BOOLEAN NOT NULL DEFAULT false
);
```
to delete one row with the id 1 from the todos table:
```PostgreSQL
DELETE FROM todos
WHERE id = 1;
```
To delete the rows with the completed status true:
```PostgreSQL
DELETE FROM todos
WHERE completed = true;
```
To delete all the rows:
```PostgreSQL
DELETE FROM todos
```

## DELETE JOIN
The basic syntax:
```PostgreSQL
DELETE FROM table1
USING table2
WHERE condition
RETURNING returning_columns;
```
In this syntax:
- First, specify the name of the table (table1) from which we want to delete data after the DELETE FROM keywords
- Second, provide a table (table2) to join with the main table after the USING keyword.
- Third, define a condition in the WHERE clause for joining two tables.
- Finally, return the deleted rows in the RETURNING clause. The RETURNING clause is optional.

For example, the following statement uses the DELETE statement with the USING clause to delete data from t1 that has the same id as t2:
```PostgreSQL
DELETE FROM t1
USING t2
WHERE t1.id = t2.id
```
Consider the following two tables:
```PostgreSQL
CREATE TABLE member(
   id SERIAL PRIMARY KEY,
   first_name VARCHAR(50) NOT NULL,
   last_name VARCHAR(50) NOT NULL,
   phone VARCHAR(15) NOT NULL
);


CREATE TABLE denylist(
    phone VARCHAR(15) PRIMARY KEY
);
```
Delete the records from member tables whose phone numbers are in the denylist:
```PostgreSQL
DELETE FROM member
USING denylist
WHERE member.phone = denylist.phone
-------------------------------------alternative statement
DELETE FROM member
WHERE phone IN (
  SELECT phone
  FROM denylist
)
```
</details>

<details>
  <summary>Modifying Data : [ UPSERT ] : INSERT ON CONFLICT </summary>

The basic syntax of the INSERT...ON CONFLICT statement:
```PostgreSQL
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...)
ON CONFLICT (conflict_column)
DO NOTHING | DO UPDATE SET column1 = value1, column2 = value2, ...;
```
In this syntax:
- table_name: This is the name of the table into which we want to insert data.
- (column1, column2, ...): The list of columns we want to insert values into the table.
- VALUES(value1, value2, ...): The values we want to insert into the specified columns (column1, column2, ...).
- ON CONFLICT (conflict_column): This clause specifies the conflict target, which is the unique constraint or unique index that may cause a conflict.
- DO NOTHING: This instructs PostgreSQL to do nothing when a conflict occurs.
- DO UPDATE: This performs an update if a conflict occurs.
- SET column = value1, column = value2, ...: This list of the columns to be updated and their corresponding values in case of conflict.

**Working of INSERT...ON CONFLICT**
- First, the ON CONFLICT clause identifies the conflict target which is usually a unique constraint (or a unique index). If the data that we insert violates the constraint, a conflict occurs.
- Second, the DO UPDATE instructs PostgreSQL to update existing rows or do nothing rather than aborting the entire operation when a conflict occurs.
- Third, the SET clause defines the columns and values to update. We can use new values or reference the values we attempted to insert using the EXCLUDED keyword.

Example:
```PostgreSQL
CREATE TABLE inventory(
   id INT PRIMARY KEY,
   name VARCHAR(255) NOT NULL,
   price DECIMAL(10,2) NOT NULL,
   quantity INT NOT NULL
);
-----------------------------------
INSERT INTO inventory (id, name, price, quantity)
VALUES (1, 'A', 16.99, 120)
ON CONFLICT(id)
DO UPDATE SET
  price = EXCLUDED.price,
  quantity = EXCLUDED.quantity;
```
</details>

<details>
  <summary>Transactions : [ BEGIN | COMMIT | ROLLBACK ] </summary>

## What is a database transaction?
- A database transaction is a single unit of work that consists of one or more operations.
- A classical example of a transaction is a bank transfer from one account to another. A complete transaction must ensure a balance between the sender and receiver accounts.
- This implies that if the sender account transfers X amount, the receiver receives exactly X amount, neither more nor less.
- A PostgreSQL transaction is atomic, consistent, isolated, and durable. These properties are often referred to collectively as ACID:
  - **Atomicity** guarantees that the transaction is completed in an all-or-nothing manner.
  - **Consistency** ensures that changes to data written to the database are valid and adhere to predefined rules.
  - **Isolation** determines how the integrity of a transaction is visible to other transactions.
  - **Durability** ensures that transactions that have been committed are permanently stored in the database.

To begin a transaction:
```PostgreSQL
BEGIN TRANSACTION;
------------------OR
BEGIN WORK;
------------------OR
BEGIN;
```
Example:
```PostgreSQL
BEGIN;

INSERT/UPDATE/DELETE commands
```
**To commit a transaction:**  
To permanently apply the change to the database, we commit the transaction by using the COMMIT WORK statement:
```PostgreSQL
COMMIT TRANSACTION;
--------------------OR
COMMIT WORK;
--------------------OR
COMMIT;
```
Example:
```PostgreSQL
BEGIN;

INSERT/UPDATE/DELETE commnad(s)

COMMIT;
```
**Roll back a transaction**  
If we want to undo the changes to the database, we can use the ROLLBACK statement:
```PostgreSQL
ROLLBACK TRANSACTION;
-------------------------OR
ROLLBACK WORK;
-------------------------OR
ROLLBACK;
```
Example: 

```PostgreSQL
-- start a transaction
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id =  1;

-- rollback the changes
ROLLBACK;
```
</details>

<details>
  <summary> Managing Tables : [ CREATE TABLE | SELECT INTO | CREATE TABLE AS ] </summary>
<br><br>

## CREATE TABLE
The basic syntax of the ``create`` table statement is as follows:
```PostgreSQL
CREATE TABLE [IF NOT EXISTS] table_name (
   column1 datatype(length) column_constraint,
   column2 datatype(length) column_constraint,
   ...
   table_constraints
);
```
In this syntax:  
- First, specify the name of the table that we want to create after the ``CREATE TABLE`` keywords. The table name must be unique in a schema. If we create a table with a name that already exists, we'll get an error.
  - A schema is a named collection of database objects including tables. If you create a table without a schema, it defaults to public.
- Second, use the IF NOT EXISTS option to create a new table only if it does not exist. When we use the IF NOT EXISTS option and the table already exists, PostgreSQL will issue a notice instead of an error.
- Third, specify table columns separated by commas. Each column definition consists of the column name, data type, size, and constraint.
  - The constraint of a column specifies a rule that is applied to data within a column to ensure data integrity. The column constraints include ``primary key``, ``foreign key``, ``not null``, ``unique``, ``check``, and ``default``.
    - For example, the ``NOT NULL`` constraint ensures that the values in a column cannot be NULL. 
- Finally, specify constraints for the table including ``primary key``, ``foreign key``, and ``check`` constraints.
  - A table constraint is a rule that is applied to the data within the table to maintain data integrity.

**Note:** Some column constraints can be defined as table constraints such as primary key, foreign key, unique, and check constraints.   

### Constraints
PostgreSQL includes the following column constraints:
- NOT NULL– ensures that the values in a column cannot be NULL.
- UNIQUE – ensures the values in a column are unique across the rows within the same table.
- PRIMARY KEY – a primary key column uniquely identifies rows in a table. A table can have one and only one primary key. The primary key constraint allows to define the primary key of a table.
- CHECK – ensures the data must satisfy a boolean expression. For example, the value in the price column must be zero or positive.
- FOREIGN KEY – ensures that the values in a column or a group of columns from a table exist in a column or group of columns in another table. Unlike the primary key, a table can have many foreign keys.
 
**Note:** Table constraints are similar to column constraints except that we can include more than one column in the table constraint.

**Example:**
We will create a new table called accounts in the dvdrental sample database.  
The accounts table has the following columns:
- user_id – primary key
- username – unique and not null
- password – not null
- email – unique and not null
- created_at – not null
- last_login – null
```PostgreSQL
CREATE TABLE accounts (
  user_id SERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  password VARCHAR(50) NOT NULL,
  email VARCHAR(50) UNIQUE NOT NULL,
  created_at TIMESTAMP NOT NULL,
  last_login TIMESTAMP 
);
```
## SELECT INTO
The PostgreSQL ``SELECT INTO`` statement creates a new table and inserts data returned from a query into the table.  

The basic syntax is as follows: 
```PostgreSQL
SELECT select_list
INTO [ TEMPORARY | TEMP ] [ TABLE ] new_table_name
FROM table_name
WHERE search_condition;
```
Here,  
- To create a new table with the structure and data derived from a result set, we specify the new table name after the INTO keyword.
- The TEMP or TEMPORARY keyword is optional; it allows to create a temporary table instead.
- The TABLE keyword is optional, which enhances the clarity of the statement.
- The WHERE clause allows to specify a condition that determines which rows from the original tables should be filled into the new table.
- Besides the WHERE clause, we can use other clauses in the SELECT statement for the SELECT INTO statement such as INNER JOIN, LEFT JOIN, GROUP BY, and HAVING.

**Note:** we cannot use the SELECT INTO statement in PL/pgSQL because it interprets the INTO clause differently. In this case, we can use the CREATE TABLE AS statement which provides more functionality than the SELECT INTO statement.

**Example:**  
Create a new table called film_r that contains films with the rating R and rental duration 5 days from the film table.
```PostgreSQL
SELECT film_id, title, rental_rate
INTO TABLE film_r
FROM film
WHERE rating  = 'R' AND rental_duration = 5
ORDER BY title
```
Create a temporary table named short_film that contains films whose lengths are under 60 minutes:
```PostgreSQL
SELECT film_id, title, length
INTO TEMP TABLE short_film
FROM film
WHERE length < 60 
ORDER BY title
```
## CREATE TABLE AS
The CREATE TABLE AS statement creates a new table and fills it with the data returned by a query.  

The following shows the syntax:
```PostgreSQL
CREATE TABLE new_table_name
AS query;
```
For the creation of temporary table:
```PostgreSQL
CREATE TEMP TABLE new_table_name
AS query;
```
The columns of the new table will have the names and data types associated with the output columns of the SELECT clause.  

If we want the table columns to have different names, then we can specify the new table columns after the new table name:
```PostgreSQL
CREATE TABLE new_table_name ( column_name_list )
AS query;
```
To avoid an error by create a new table that already exists, we can use the ``IF NOT EXISTS`` options as follows:
```PostgreSQL
CREATE TABLE IF NOT EXISTS new_table_name
AS query;
```
Create a new table that contains the films whose category is 1:
```PostgreSQL
CREATE TABLE IF NOT EXISTS action_film
AS (
  SELECT film_id, title, release_year, length, rating
  FROM film INNER JOIN film_category ON film.film_id = film_category.film_id
  WHERE category_id = 1

);
```

</details>

<details>
  <summary>Managing Tables : [ Auto Increment - SERIAL ] </summary>

## SERIAL pseudo-type
Used to create auto-increment column in a table.

The basic syntax is as follows:
```PostgreSQL
CREATE TABLE table_name (
  id SERIAL
);
```
which is equivalent to:
```PostgreSQL
CREATE SEQUENCE table_name_id_seq;

CREATE TABLE table_name (
  id integer NOT NUL DEFAULT nextval('table_nam_id_seq)
);

ALTER SEQUENCE table_name_id_seq
OWNED BY table_name.id;
```
PostgreSQL provides three serial pseudo types:
- ``SMALLSERIAL``  
- ``SERIAL``  
- ``BIGSERIAL`` 

Creates `fruits` table with the id column as the SERIAL column:  
```PostgreSQL
CREATE TABLE fruits (
  id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL
)
```
</details>

<details>
  <summary>Managing Tables : [ ALTER TABLE ] </summary>

## ALTER TABLE
Used to change the structure of an existing table.  
The basic syntax:
```PostgreSQL
ALTER TABLE table_name action;
```
PostgreSQL provides the following actions:
### Add a column
To add a column:
```PostgreSQL
ALTER TABLE table_name
ADD COLUMN column_name datatype column_constraint;
```
When we add a new column to the table, PostgreSQL appends it at the end of the table. PostgreSQL has no option to specify the position of the new column in the table.   

To add multiple column:
```PostgreSQL
ALTER TABLE table_name
ADD COLUMN column_name1 datatype column_constraint;
ADD COLUMN column_name2 datatype column_constraint;
ADD COLUMN column_name3 datatype column_constraint;
... ... ... ... ... ... ... ...
ADD COLUMN column_namen datatype column_constraint;
```
### Drop a column
```PostgreSQL
ALTER TABLE table_name
DROP COLUMN column_name;
``` 
If the column that we want to remove is used in other database objects such as views, triggers, and stored procedures, we cannot drop the column because other objects depend on it.   
To drop the column and all of its dependent objects:
```PostgreSQL
ALTER TABLE table_name
DROP COLUMN column_name CASCADE;
```
To avoid error when column does not exist, use the following:
```PostgreSQL
ALTER TABLE table_name
DROP COLUMN IF EXISTS column_name;
```
### Change column datatype
To change the datatype of a column:
```PostgreSQL
ALTER TABLE table_name
ALTER COLUMN column_name
SET DATA TYPE new_data_type;
```
### Rename a column
To rename a column:
```PostgreSQL
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;
``` 
### Add/Change constraint to a column
#### Set/Drop default value for the column
    ```PostgreSQL
    ---------------------------------Set default value
    ALTER TABLE table_name
    ALTER COLUMN column_name
    SET DEFAULT value;
    ---------------------------------Drop default value
    ALTER TABLE table_name
    ALTER COLUMN column_name
    DROP DEFAULT;
    ``` 
#### To change ``NOT NULL`` constraint:
    ```PostgreSQL
    ----------------------------------Set NOT NULL
    ALTER TABLE table_name
    ALTER COLUMN column_name
    SET NOT NULL;
    ----------------------------------Drop NOT NULL
    ALTER TABLE table_name
    ALTER COLUMN column_name
    DROP NOT NULL;
    ``` 
### Add a constraint to a table:
  ```PostgreSQL
  ALTER TABLE table_name
  ADD CONSTRAINT constraint_name constraint_definition
  ```
### Rename a table
To rename a table:
```PostgreSQL
ALTER TABLE table_name
RENAME TO new_table_name
``` 
To avoid error if the table does not exist:
```PostgreSQL
ALTER TABLE IF EXISTS table_name
RENAME to new_table_name
```
### Drop a table
To drop a table:
```PostgreSQL
DROP TABLE IF EXISTS table_name [CASCADE | RESTRICT]
```
- Use the CASCADE option to remove the table and its dependent objects.
- Use the RESTRICT option rejects the removal if there is any object depending on the table. The RESTRICT option is the default if we don’t explicitly specify it in the DROP TABLE statement.
### Truncate a table
To remove all data from a table:
```PostgreSQL
DROP TABLE table_name;
```
To remove all data if table has foreign key references:
```PostgreSQL
DROP TABLE table_name CASCADE;
```
To remove all data with resetting the values of identity column:
```PostgreSQL
TRUNCATE TABLE table_name
RESTART IDENTITY;
```
### Copy a table
To copy a table completely, including both table structure and data:
```PostgreSQL
CREATE TABLE new_table AS TABLE existing_table;
```
To copy a table structure with no data:
```PostgreSQL
CREATE TABLE new_table AS TABLE existing_table WITH NO DATA;
```
To copy a table structure with partial data:
```PostgreSQL
CREATE TABLE new_table AS
SELECT *
FROM existing_table
WHERE condition;
```
</details>

<details>
  <summary>Constraints : [ PRIMARY KEY | FOREIGN KEY | CHECK | UNIQUE | NOT NULL | DEFAULT ] </summary>

## PRIMARY KEY
- A primary key is a column or a group of columns used to uniquely identify a row in a table. The column that participates in the primary key is known as the primary key column.
- A table can have zero or one primary key. It cannot have more than one primary key.
- Technically, a primary key constraint is the combination of a not-null constraint and a UNIQUE constraint.

To define primary key for a table while creating it:
```PostgreSQL
CREATE TABLE table_name (
  column1 datatype PRIMARY KEY,
  column2 datatype, 
  ... ... ...
);
-----------------------------------if the PRIMARY KEY consists of more that one column
CREATE TABLE table_name (
  column1 datatype,
  column2 data_type,
  column3 data_type,
  ... ... ... 
  PRIMARY KEY(column1, column2, ...)
)
```
To add a primary key to an existing table:
```PostgreSQL
ALTER TABLE table_name
ADD PRIMARY KEY ( column1, column2, ...);
```
- Default name for the primary key constraint: table-name_pkey
- To assign name for the PRIMARY KEY:
  ```PostgreSQL
  CONSTRAINT constraint_name
  PRIMARY KEY(column1, column2, ...)
  ``` 
To drop a primary key:
```PostgreSQL
ALTER TABLE table_name
DROP CONSTRAINT primary_key_constraint_name;
```
## FOREIGN KEY
- A foreign key is a column or a group of columns in a table that uniquely identifies a row in another table.
- A foreign key establishes a link between the data in two tables by referencing the primary key or a unique constraint of the referenced table.
- The table containing a foreign key is referred to as the referencing table or child table. Conversely, the table referenced by a foreign key is known as the referenced table or parent table.
- The main purpose of foreign keys is to maintain referential integrity in a relational database, ensuring that relationships between the parent and child tables are valid.
  - For example, a foreign key prevents the insertion of values that do not have corresponding values in the referenced table.

The basic syntax is as follows:
```PostgreSQL
[ CONSTRAINT fk_name]
  FOREIGN KEY(fk_columns)
  REFERENCES parent_table(parent_key_columns)
  [ON DELETE delete_action]
  [ON UPDATE update_action]
```
In this syntax:
- First, specify the name for the foreign key constraint after the CONSTRAINT keyword. The CONSTRAINT clause is optional. If we omit it, PostgreSQL will assign an auto-generated name.
- Second, specify one or more foreign key columns in parentheses after the FOREIGN KEY keywords.
- Third, specify the parent table and parent key columns referenced by the foreign key columns in the REFERENCES clause.
- Finally, specify the desired delete and update actions in the ON DELETE and ON UPDATE clauses.
  - The delete and update actions determine the behaviors when the primary key in the parent table is deleted and updated.

PostgreSQL supports the following actions:
- SET NULL
- SET DEFAULT
- RESTRICT
- NO ACTION 
- CASCADE
To add a foreign key constraint to an existing table:
```PostgreSQL
ALTER TABLE child_table
ADD CONSTRAINT constraint_name
FOREIGN KEY (fk_columns)
REFERENCES parent_table(parent_key_columns)
[ ON DELETE delete_action]
[ ON UPDATE update_action]
```
## CHECK 
- A ``CHECK`` constraint ensures that values in a column or a group of columns meet a specific condition.
- A check constraint allows to enforce data integrity rules at the database level.
- A check constraint uses a boolean expression to evaluate the values, ensuring that only valid data is inserted or updated in a table.
The basic syntax is as follows:
```PostgreSQL
-----------------------------------if check condition involve one column
CREATE TABLE table_name (
  column1 data_type,
  column2 data_type CHECK(condition),
  ... ... ... 
);
-----------------------------------if check condition involve more than one column
CREATE TABLE table_name (
  column1 data_type,
  ... ... ...
  CONSTRAINT constraint_name CHECK(condition)
);
```
Here, constraint_name is optional. If we omit this, PostgreSQL will assign a default name using the following format:
```PostgreSQL
{table}_{column}_check
```
To add check constraint to tables:
```PostgreSQL
ALTER TABLE table_name
ADD CONSTRAINT constraint_name CHECK (condition);
```
To remove check constraint:
```PostgreSQL
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
```
## UNIQUE
- Used to ensure that values stored in a column or a group of columns are unique across the whole table.
```PostgreSQL
CREATE TABLE table_name (
  column1 data_type,
  column2 data_type UNIQUE,
  ... ... ...
);
--------------------------Or, 
CREATE TABLE table_name (
  column1 data_type,
  column2 data_type,
  ... ... ...
  
  UNIQUE(columns)
);
```
## NOT NULL
- NULL represents unknown or missing information. NULL is not the same as an empty string or the number zero.
- To check if a value is NULL or not, use ``IS NULL`` boolean operator
  - For example, to check if the email_address is NULL or not:
    ```PostgreSQL
    email_address IS NULL;
    ``` 

To add ``NOT NULL`` constraint to a column:
```PostgreSQL
CREATE TABLE table_name(
   ...
   column_name data_type NOT NULL,
   ...
);
```
To add ``NOT NULL`` constraint to an existing column:
```PostgreSQL
ALTER TABLE table_name
ALTER COLUMN column_name SET NOT NULL;
```
Using CHECK to force a column to accept NOT NULL values:
```PostgreSQL
CHECK(column IS NOT NULL);
```
## DEFAULT 
To add default value:
```PostgreSQL
CREATE TABLE table_name(
  column1 data_type,
  column2 data_type DEFAULT default_value,
  column3 data_type,
  ... ... ...
);
```
To add default to an existing column;
```PostgreSQL
ALTER TABLE table_name
ALTER COLUMN column_name
SET DEFAULT default_value;
```
To remove default value from a column:
```PostgreSQL
ALTER TABLE table_name
ALTER COLUMN column_name
DROP DEFAULT;
```
</details>

<details>
  <summary>Conditional Expressions & Operators : [ CASE | COALESCE | NULLIF | CAST ]</summary>

## CASE 
- The PostgreSQL CASE expression is the same as IF/ELSE statement in other programming languages. It allows to add if-else logic to the query to form a powerful query.
- Since CASE is an expression, we can use it in any place where we would use an expression such as SELECT, WHERE, GROUP BY, and HAVING clauses.
- The CASE expression has two forms:
  - General
  - Simple
### General CASE expression:
```PostgreSQL
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  [WHEN ... ]
  [ELSE else_result]
END
```
In this syntax,
- each condition (``condition1``, ``condition2…``) is a boolean expression that returns either ``true`` or ``false``.
- When a condition evaluates to ``false``, the ``CASE`` expression evaluates the next condition from top to bottom until it finds a condition that evaluates to ``true``.
- If a condition evaluates to ``true``, the ``CASE`` expression returns the corresponding result that follows the condition.
  - For example, if the ``condition2`` evaluates to ``true``, the ``CASE`` expression returns the ``result2``. Also, it immediately stops evaluating the remaining expressions. 
- If all conditions are ``false``, the ``CASE`` expression returns the result (``else_result``) that follows the ``ELSE`` keyword. If we omit the ``ELSE`` clause, the ``CASE`` expression returns ``NULL``.

Label the films by their lengths based on the following logic:
- If the length is less than 50 minutes, the film is short.
- If the length is greater than 50 minutes and less than or equal to 120 minutes, the film is medium.
- If the length is greater than 120 minutes, the film is long.
```PostgreSQL
SELECT
  title,
  length,
  CASE
    WHEN length > 0 and length <= 50 THEN 'SHORT'
    WHEN length > 50 and length <= 120 THEN 'MEDIUM'
    WHEN length > 120 THEN 'LONG'
  END AS duration
FROM
  film
ORDER BY
  title;
```
Assign price segments to films with the following logic:
- If the rental rate is 0.99, the film is economic.
- If the rental rate is 1.99, the film is mass.
- If the rental rate is 4.99, the film is premium.
And count the number of films that belong to economy, mass, and premium
```PostgreSQL
SELECT 
  SUM (
    CASE
      WHEN rental_rate == 0.99 THEN 1 ELSE 0 
    END
  ) AS "Economy",
  SUM (
    CASE
      WHEN rental_rate == 1.99 THEN 1 ELSE 0 
    END
  ) AS "Mass",
  SUM (
    CASE
      WHEN rental_rate == 4.99 THEN 1 ELSE 0 
    END
  ) AS "Premium"
FROM
  film;
```
### Simple CASE expression
```PostgreSQL
CASE expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  [WHEN ...]
  ELSE else_result
END
```
- The CASE first evaluates the expression and compares the result with each value( value1, value2, …) in the WHEN clauses sequentially until it finds the match.
- Once the result of the expression equals a value (value1, value2, etc.) in a WHEN clause, the CASE returns the corresponding result in the THEN clause.
- If CASE does not find any matches, it returns the else_result in that follows the ELSE, or NULL value if the ELSE is not available.
Adding rating description:
```PostgreSQL
SELECT title,
       rating,
       CASE rating
           WHEN 'G' THEN 'General Audiences'
           WHEN 'PG' THEN 'Parental Guidance Suggested'
           WHEN 'PG-13' THEN 'Parents Strongly Cautioned'
           WHEN 'R' THEN 'Restricted'
           WHEN 'NC-17' THEN 'Adults Only'
       END rating_description
FROM film
ORDER BY title;
```
## COALESCE
- The COALESCE() function accepts a list of arguments and returns the first non-null argument.
The basic syntax is as follows:
```PostgreSQL
COALESCE(argument1, argument2,...);
```
- The COALESCE() function evaluates arguments from left to right until it finds the first non-null argument. All the remaining arguments from the first non-null argument are not evaluated.

Consider the following table:
```PostgreSQL
CREATE TABLE items (
  id SERIAL PRIMARY KEY,
  product VARCHAR (100) NOT NULL,
  price NUMERIC NOT NULL,
  discount NUMERIC
);
```
Here, discount column accepts NULL values. If we calculate net price as ( price - discount ), then it will return NULL if discount column has NULL values. To solve this issue, we can use COALESCE function as follows:
```PostgreSQL
SELECT
  product,
  ( price - COALESCE(discount, 0) ) AS net_price
FROM items;
```
This query can also executed using CASE statement as follows:
```PostgreSQL
SELECT
  product,
  ( price - ( CASE WHEN discount IS NULL THEN 0 ELSE discount END) ) AS net_price
FROM items;
```
## NULLIF
The basic syntax:
```PostgreSQL
NULLIF(argument1, argument2);
```
The ``NULLIF`` function returns a null value if argument1 equals to argument2, otherwise, it returns argument1.
## CAST
To convert a value of one type into another, we can use ``CAST() `` function and cast operator (``::``).   

Using CAST() function:
```PostgreSQL
CAST( value AS target_type)
```
In this syntax:
- First, provide a value that we want to convert. It can be a constant, a table column, or an expression.
- Then, specify the target data type to which we want to convert the value.
Using cast operator(::):
```PostgreSQL
value::target_type
```
In this syntax:
- value is a value that we want to convert.
- target_type specifies the target type that we want to cast the value to.

Both the process will raise error, if it can't cast to desired type;


Example:
```PostgreSQL
SELECT '2019-06-15 14:30:20'::timestamp;
```
</details>