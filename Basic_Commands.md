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
  <summary> Database Definition: [ CREATE ] : Table </summary>
<br><br>

| Command | Description |
| ----------- | ----------- |  
|**CREATE TABLE** table_name ( <br>  column_name_1 data_type (size) NULL/ NOT NULL , <br> column_name_2 data_type (size) NULL/ NOT NULL ,<br> column_name_3 data_type (size) NULL/ NOT NULL , <br>... ... ...<br>... ... ...<br>PRIMARY KEY(column_name/s) ,<br> CONSTRAINT fk_name FOREIGN KEY (Column_Name/s) REFERENCES referenced_table_name(referenced_column_Name/s) ON DELETE CASCADE ON UPDATE CASCADE , <br>... ... ...<br>... ... ...<br>); |  To Create a Table with Primary key and Foreign Keys.<br> <br>**For example:** <br>create table personal( <br>id int, <br>name varchar(50),<br>birth_date date, <br>phone varchar(12), <br>gender varchar(1));<br> <br><b><u>NOTE:</u> Each Table can have only one Primary Key which may consist of one or more than one Columns. But a table/relation may have multiple Foreign Key.In Case of, Foreign Key declaration, referenced Column have to be Primary Key in referenced table/relation.|  


#### Data Types:

There are three main data types:
- String
  - **char(size)** : fixed length string, column length can be 0 to 255
  - **varchar(size)** : variable length string, length can be 0 to 65535
  - **binary(size)** : fixed length string but only stores binary byte string, length 0 to 255, default 1
  - **varbinary(size)** : variable length string but only stores binary bytes string, length 0 to 65535, default 1
  - **tinytext** : holds a string with maximum length of 255 charachters
  - **text(size)** : holds a string with maximum length of 65535 bytes
  - **mediumtext** : holds a string with maximum length of 16,777,215 bytes
  - **longtext** : holds a string with maximum length of 4,294,967,295 characters
  - **tinyblob** : for BLOBS. max length : 255 bytes
  - **blob** : for BLOBS, max length : 65,535 bytes
  - **mediumblob** : for BLOBS, mex length : 16,777,215 bytes
  - **longblob** : for BLOBS, mex length : 4,294,967,295 bytes
  - **enum(values1, values2, ...)** : A string object that can have only one value, chosen from a list of possible values. We can list up to 65535 values in an enum list
  - **set(val1, val2, val3, ...)** : A string object that can have 0 or more values, chosen from a list of possible values.

- Numeric
  - **bit(size)** : bit value type. size 1 to 64. default size 1 
  - **tinyint(size)** : value range from -128 to  127. maximum size 255
  - **int(size)** : value ranges from 2147483648 to 2147483647. 
  - **integer(size)** : value ranges from 2147483648 to 2147483647
  - **smallint(size)** : A small integer. Signed range is from -32768 to 32767.
  - **mediumint(size)** : A medium integer. Signed range is from -8388608 to 8388607
  - **bigint(size)** : A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. 
  - **bool** : Zero is considered as false, nonzero values are considered as true.
  - **boolean** : Zero is considered as false, nonzero values are considered as true.
  - **float(p)** : A floating point number. MySQL uses the p value to determine whether to use FLOAT or DOUBLE for the resulting data type. If p is from 0 to 24, the data type becomes FLOAT(). If p is from 25 to 53, the data type becomes DOUBLE()
  - **double(size,d)** : A normal-size floating point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter
  - **decimal(size,d)** : An exact fixed-point number. The total number of digits is specified in size. The number of digits after the decimal point is specified in the d parameter. The maximum number for size is 65, but default 10. The maximum number for d is 30, default 0.
  - **dec(size,d)** : Same as DECIMAL(size,d)
- Date and Time
  - **date** 'yyyy-mm-dd', allowed '1000-01-01' to '9999-12-31'
  - **datetime(fsp)** 'yyyy-mm-dd hh:mm:ss', allowed '1000-01-01 00:00:00' to '9999-12-31 23:59:59'
    - Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time
  - **timestamp(fsp)** 'yyyy-mm-dd hh:mm:ss', allowed '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07'
  - **time(fsp)** 'hh:mm:ss', allowed '-838:59:59' to '838:59:59'
  - **year** a year in four-digit format, allowed 1901 to 2155 and 0000

</details>

<details>
<summary> Modifying Database : [ INSERT INTO ] : Table </summary>
  
<br><br>
  
| Command    | Description |
| ----------- | ----------- |  
|<b>INSERT INTO table_name VALUES <br>(value1,value2,value3,... ...),<br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...), <br>... ... ; <b>| TO add values for all the columns of the table.<br><br> No need to specify the column names in the SQL syntax. <br><br> But need to make sure the order of the values is in the same order as the columns in the table.|
|<b>INSERT INTO table_name <br>(column1, column2, column3,... ...) VALUES <br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...) , <br>(value1,value2,value3,... ...),<br> ... ... ; <b>|To insert Data Only in Specified Columns.|
  <br>

  <br> 
</details>

<details>
  <summary>Querying Data : [ SELECT | SELECT DISTINCT ] : Table </summary>

  The SELECT statement has the following clauses:
  - Select distinct rows using **DISTINCT** operator.
  - Sort rows using **ORDER BY** clause.
  - Filter rows using **WHERE** clause.
  - Select a subset of rows from a table using **LIMIT or FETCH** clause.
  - Group rows into groups using **GROUP BY** clause.
  - Filter groups using **HAVING** clause.
  - Join with other tables using joins such as **INNER JOIN**, **LEFT JOIN**, **FULL OUTER JOIN**, **CROSS JOIN** clauses.
  - Perform set operations using **UNION**, **INTERSECT**, and **EXCEPT**

In this section, we will focus on **SELECT** and **FROM** clause
  | Command    | Description |
  | ----------- | ----------- |  
  | **SELECT** select_list <br> **FROM** table_name | General statement for basic query <br> <b>Note : <b> Where clause is optional| 
  |**SELECT** column1, column2, ...<br> **FROM** table_name ;| Used to select data from a database.The data returned is stored in a result table.|
  |**SELECT** * <br> **FROM** table_name|To see all the fields available in the table|
  |**SELECT** **DISTINCT** column1, column2, ... <br>**FROM** table_name;<br> <br>**SELECT** **DISTINCT**(column_name) **FROM** table_name;| Used to return only distinct (different) values. <br> **NOTE** : Inside a table, a column often contains many duplicate values; and sometimes we only want to list the different (distinct) values. <br> **Note:** The **DISTINCT** keyword operates on column(s)|

</details>

<details>
  <summary>Querying Data : [ ORDER BY ] : Table </summary>
    
  | Command | Description |
  | --- | --- |  
  | **SELECT** column1, column2, ... <br>**FROM** table_name <br> **ORDER BY**  sort_expression1 **ASC/DESC** , <br> sort_expression2 **ASC/DESC** | The ORDER BY clause allows to sort rows returned by a SELECT clause in ascending or descending order based on a sort expression |  

  **Execution Order:**  
  PostgreSQL evaluates the clauses in the SELECT statement in the following order: ``FROM``, ```SELECT```, and ```ORDER BY```  

  **Note:** 
  - Use **ASC** to sort in **ascending** order.
  - Use **DESC** to sort in **descending** order.
  - If we leave it blank, **ORDER BY** uses **ASC** by **default**.

  **Example:**  
  Example - 01:
  ```PostgreSQL
  SELECT store_id, first_name, last_name 
  FROM customer
  ORDER BY store_id DESC, first_name ASC;
  ```
  Example - 02:
  ```PostgreSQL
  SELECT store_id, first_name, last_name 
  FROM customer
  ORDER BY store_id DESC;
  ```
</details>

<details>
  <summary>Filtering Data : [ WHERE ] : Table </summary>

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
  <summary>Filtering Data : [ LIMIT | OFFSET ] : Table </summary>  
  
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
</details>

<details>
  <summary> Operators [ Arithmetic | Comparison ] </summary>

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