# MySQL Basics

<details>
  <summary> Section 01 : [ SHOW | CREATE | DROP | BACKUP | RESTORE | RENAME | COPY] : Database </summary>
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
  <summary> Section 02: [ CREATE ] : Table </summary>
<br><br>

| Command | Description |
| ----------- | ----------- |  
|**CREATE TABLE** table_name ( <br>  column_name_1 data_type (size) NULL/ NOT NULL , <br> column_name_2 data_type (size) NULL/ NOT NULL ,<br> column_name_3 data_type (size) NULL/ NOT NULL , <br>... ... ...<br>... ... ...<br>PRIMARY KEY(column_name/s) ,<br> CONSTRAINT fk_name FOREIGN KEY (Column_Name/s) REFERENCES referenced_table_name(referenced_column_Name/s) ON DELETE CASCADE ON UPDATE CASCADE , <br>... ... ...<br>... ... ...<br>); |  To Create a Table with Primary key and Foreign Keys.<br> <br>**For example:** <br>create table personal( <br>id int, <br>name varchar(50),<br>birth_date date, <br>phone varchar(12), <br>gender varchar(1));<br> <br><b><u>NOTE:</u> Each Table can have only one Primary Key which may consist of one or more than one Columns. But a table/relation may have multiple Foreign Key.In Case of, Foreign Key declaration, referenced Column have to be Primary Key in referenced table/relation.|  


#### Data Types in MySQL:
There are three main data types in MySQL:
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
<summary> Section 03 : [ INSERT INTO ] : Table </summary>
  
<br><br>
  
| Command    | Description |
| ----------- | ----------- |  
|<b>INSERT INTO table_name VALUES <br>(value1,value2,value3,... ...),<br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...), <br>... ... ; <b>| TO add values for all the columns of the table.<br><br> No need to specify the column names in the SQL syntax. <br><br> But need to make sure the order of the values is in the same order as the columns in the table.|
|<b>INSERT INTO table_name <br>(column1, column2, column3,... ...) VALUES <br>(value1,value2,value3,... ...), <br>(value1,value2,value3,... ...) , <br>(value1,value2,value3,... ...),<br> ... ... ; <b>|To insert Data Only in Specified Columns.|
  <br>

  <br> 
</details>