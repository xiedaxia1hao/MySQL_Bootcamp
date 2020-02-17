This note is derived from the Udemy Course: [The Ultimate MySQL Bootcamp: Go from SQL Beginner to Expert][https://www.udemy.com/course/the-ultimate-mysql-bootcamp-go-from-sql-beginner-to-expert/]. I just make it for my self-study and review purpose. For anyone who many needs the quick notes of SQL, please feel free to use this one, and I am happy to share it to the world : )



Onwards! 

---



# SQL NOTES

### Common-used Data Type

- `INT`: While numbers

- `DECIMAL`: requires two parameters, e.g. `DECIMAL(total_number_of_digits, digits_after_decimal)`

  - e.g. `DECIMAL(5,2)` -> 999.99 (5 digits long, 2 digits after decimal)
    - If in this case, we insert a value larger than 999.99, for examle, 2135987129857, then in the table it will store as 999.99, since 999.99 is the largest number the table can store.
    - If we insert 1.9999, then we will return 2.00. Since when we only allow two digits after decimal, it will automatically round up. 
  - total_number_of_digits ranges from 1-65, and digits_after_decimal range from 0-30

- `FLOAT` and `DOUBLE`: store larger numbers using less space -> comes at the csot of precision

  - `FLOAT`: 4 Bytes, ~7 digits precision
  - `DOUBLE`: 8 Bytes, ~15 digits precision
  - **Which do I use? : Always try to use `DECIMAL` unless we know precision doesn't matter, and `DOUBLE` will guarantee more precision**
  - `DECIMAL` vs. `FLOAT` and `DOUBLE`
    - `DECIMAL`: <u>fixed-point type</u> and calculations are <u>exact</u>
    - `FLOAT` and `DOUBLE`: <u>floating-point type</u> and calculations are <u>approximate</u>. 

- `VARCHAR` (between 1 and 255 characters): e.g. ``varchar(100)``, ``varchar(254)``

- `CHAR`: has a fixed length, and the length can be any value from 0 to 255. e.g. `CHAR(5)`, which means only 5 characters allowed. If exceeds 5 chars, it will truncate. If it's less than 5 chars, it will add up to 5 chars. **Always occupy the same space.** 

  - Note: When CHAR values are retrieved, trailing spaces are removed unless the PAD_CHAR_TO_FULL_LENGTH SQL mode is enabled. 
  - CHAR is faster for fixed length text. e.g. State Abbr: CA, NY; Sex: M/F

- `DATE`: stores data without time. **'YYYY-MM-DD' Format**.

- `TIME`: stores time but no date. **'HH:MM:SS' Format**

- `DATETIME`: stores a date AND time. **'YYYY-MM-DD HH:MM:SS' Format**

  ```mysql
  INSERT INTO people (name, birthdate, birthtime, birthdt)
  VALUES('Padma', '1983-11-11', '10:07:35', '1983-11-11 10:07:35');
  ```

- `CURDATE()`: gives current date. e.g. `SELECT CURDATE();`

- `CURTIME()`: gives current time. e.g. `SELECT CURTIME();`

- `NOW()`: gives current datetime. e.g. `SELECT NOW();`

- Formatting Dates:

  - **DATE_FORMAT() is more common**

  ```mysql
  SELECT name, birthdate ,DAY(birthdate), DAYNAME(birthdate), DAYOFYEAR(birthtime) FROM people;
  
  SELECT name, birthdate ,MONTH(birthdate), MONTHNAME(birthdate) FROM people;
  
  SELECT name, birthtime, MINUTE(birthtime) FROM people;
  
  SELECT DATE_FORMAT('2009-10-04 22:23:00', '%W %M %Y');
  -> 'Sunday October 2009'
  SELECT DATE_FORMAT('2009-10-04 22:23:00', '%W-%M-%Y');
  -> 'Sunday-October-2009'
  ```

- **DATEMATH**:

  ```mysql
  SELECT DATEDIFF('2007-12-31 23:59:59', '2007-12-30');
  -> 1
  
  SELECT DATE_ADD('2007-12-31 23:59:59', INTERVAL 1 DAY);
  
  SELECT '2007-12-31 23:59:59' + INTERVAL 2 DAY;
  ```

- **TIMESTAMPS**: add a timestamps on the table 

  ```
  What's the difference between DATETIME and TIMESTAMP?
  ------
  They both store datetime information, but there's a difference in the range, 
  TIMESTAMP has a smaller range. TIMESTAMP also takes up less space. 
  TIMESTAMP is used for things like meta-data about when something is created
  or updated.
  ```

### Initialize SQL server

- Start SQL Command Tool: ``mysql-ctl``, then we can see all options under this command: start, install, stop, status, etc.

  - Start SQL server: ``mysql-ctl start``
  - Stop SQL server: ``mysql-ctl stop``
- Enter into SQL shell: `` mysql-ctl cli``, which means comand line interface (cli)
- Quit the shell: ``EXIT``
- Show all databases in the server: ``SHOW DATABASES;``

### Create Databases and Tables

- Create Database: ``CREATE DATABASE <database name>;``, e.g. ``CREATE DATABASE hello_world_db;``

- Delete Database: ``DROP DATABASE <database name>;``, e.g. ``DROP DATABASE hello_world_db;``

- Using Database: ``USE <database name>;``, e.g. ``USE hello_world_db;``

- See which database we are currently working on: ``SELECT database();``

- Create Table Command:

  ```
  CREATE TABLE tablename
  (
  	column_name data_type,
  	column_name data_type
  );
  
  e.g. 
  
  CREATE TABLE cats
  (
  	name VARCHAR(100),
  	age INT
  );
  ```
  - How to know it works:?
    - Show current table in database: `` SHOW TABLES;``
  - Show header of this table: ``SHOW COLUMNS FROM <table name>;`` or ``DESC <table name>``, DESC is the abbr. for describe.
  - Deleting table: ``DROP TABLE <table name>;``, e.g. ``DROP TABLE cats;``

### Insert Data

- Inserting data: 

  ```
  INSERT INTO table_name(column_name)
  VALUES (data);
  
  e.g. 
  
  INSERT INTO cats(name, age)
  VALUES ('Blue', 9); // the order of parameters matters!
  ```

- Multiple insert:

  ```
  INSERT INTO cats(name, age)
  VALUES ('Javis', 9)
  , ('Peanut', 11)
  , ('Monkey', 12);
  ```

- How to check the work: `SELECT * FROM cats;`

- If there is a warning: `SHOW WARNINGS;`

- If we want to define a new cats2 table with NOT NULL constraints:

  ```
  CREATE TABLE cats2
  (
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL
  );
  ```

- How to set default values?  

  ```
  CREATE TABLE cats3
  (
  	name VARCHAR(100) DEFAULT 'nonamegiven',
  	age INT DEFAULT 99
  );
  ```

- Combine NULL and DEFAULT:

  ```
  CREATE TABLE cats4
  (
    name VARCHAR(20) NOT NULL DEFAULT 'unnamed',
    age INT NOT NULL DEFAULT 99
  );
  ```

- **Define a table with a PRIMARY KEY constrint**: 

  ```
  CREATE TABLE unique_cats(
  	cat_id INT NOT NULL, 
  	name VARCHAR(100),
  	age INT,
  	PRIMARY KEY(cat_id)
  );
	```
	
	<u>Another way to do so:</u> 
	
	```
  CREATE TABLE unique_cats(
  	cat_id INT NOT NULL PRIMARY KEY, 
  	name VARCHAR(100),
  	age INT,
	);
  ```
  However, the approach above is tedious since every time we need to insert an id and it's hard for us to manually remember those id：
  
  e.g.
  
  
  ```  
  INSERT INTO unique_cats(cat_id, name, age) VALUES(1, 'Skippy',2);
  INSERT INTO unique_cats(cat_id, name, age) VALUES(2, 'David',4);
  ```
  **So we try to add AUTO_INCREMENT into our code:**
  
  
  ```
  CREATE TABLE unique_cats2(
		cat_id INT NOT NULL AUTO_INCREMENT, 
  	name VARCHAR(100),
  	age INT,
  PRIMARY KEY(cat_id)
  );
  ```
```
  
  Now it is easier for us to do insertion since we do not need to remember cat's id:
  
```
  INSERT INTO unique_cats2(name, age) VALUES('Skippy', 3);
  INSERT INTO unique_cats2(name, age) VALUES('Skippy', 3);
  INSERT INTO unique_cats2(name, age) VALUES('Boloson', 2);
  INSERT INTO unique_cats2(name, age) VALUES('Nelsol', 7);
  ```
  



## CRUD commands (Create, Read, Update, Delete)

### SELECT statements:

- Give me all columns in a table: ```SELECT * FROM table_name```
  - e.g. ```SELECT name FROM cats;```, ```SELECT cat_id FROM cats;```
- If we need to select multiple columns: ```SELECT colA, colB FROM cats```
  - e.g. ```SELECT cat_id, name, age FROM cats;```

### WHERE statements: 

- To find a specific row from all rows: 
  - e.g. Select by age: ```SELECT * FROM cats WHERE age = 4;```
  - e.g. Select by name: ```SELECT * FROM cats WHERE name = "Egg";```
    - Normally, the SQL is case-insensitive, thus the statement above should be the same as the statement: ```SELECT * FROM cats WHERE name = "eGG";```

### Aliases:

- ```
  SELECT cat_id AS id, name FROM cats;
   
  SELECT name AS 'cat name', breed AS 'kitty breed' FROM cats;
   
  DESC cats;
  ```

### Update Data:

```UPDATE table_name SET the_thing_need_to_change WHERE selected_item```

- e.g. ```UPDATE cats SET breed = 'Shorthair' WHERE breed = 'Tabby';```
  - or: ```UPDATE cats SET age = 14 WHERE name = 'Misty';```

### Delete:

```DELETE FROM cats WHERE name = 'EGG'```

If we choose to run ```DELETE FROM cats```, then all entries in ```cats``` will be deleted. But the ```cats``` table still there! 

### SOURCE a .sql file

```	SOURCE test.sql;```

NOTE: the directory matters! 

e.g. ```Source /dir1/dir2/dir3/test.sql;```



## String Functions

**The string functions only change the query output, and they don't affect the actual data in the database.** 

### CONCAT

Before we use `CONCAT`, we have to make sure it is in the `SELECT ... FROM ...` clause, since the sql needs to know which table we are working at. 

- `CONCAT(a,b,c,d,e,f);`:

  - e.g. `CONCAT(author_fname, author_lname);`

    - NOTE: in this case the concat will not generate a blank space between author's first name and last name. To do so: `CONCAT(author_fname, ' ', author_lname);` 
    - OR `CONCAT_WS(' ', author_fname, author_lname);`
    - `CONCAT_WS(segment, colA, colB, colC)` represents CONCAT WITH SEGMENT;

  - Another way we can do is:

    ```
    SELECT
      CONCAT(author_fname, ' ', author_lname)
      AS 'full name'
    FROM books;
    ```

    or

    ```
    SELECT author_fname AS first, author_lname AS last, 
      CONCAT(author_fname, ', ', author_lname) AS full
    FROM books;
    ```

### SUBSTRING

Before we use `SUBSTRING`, we have to make sure it is in the `SELECT ... ` clause, since the sql needs to know which table we are working at. 

- `SELECT SUBSTRING('hello world', 1, 5);` will return **hello**, <u>since the index system in sql begins at 1, not 0!</u> 
- `SELECT SUBSTRING('hello world', 7)` will return the character starting from the index 6 till the end. In this case it will return **world**
- `SELECT SUBSTRING('hello world', -5)`will return the string starting from the last but 7th index till the end. In this case it will return **world**. 
  - `SELECT SUBSTRING('hello world', -2)` will return **ld**. 

We can also use `SUBSTR` as a shortcut for `SUBSTRING`. They do the same thing. 

### REPLACE

- `SELECT REPLACE(string, str_to_be_replaced, str_to_replaced);`

- ` SELECT REPLACE('HELLO WORLD', 'HELL', '1234');` will return **all** 'HELL' in the string to '1234'. In our case, it will return 1234O WORLD. 
- This function is **case-sensitive**: `SELECT REPLACE('Hello WOrld', 'o', '7');` will return HELL7 WOrld.  

### REVERSE

- `SELECT REVERSE('Hello World')` will return 'dlroW olleH'. 

### CHAR_LENGTH

Return the length of the given string.

- `SELECT CHAR_LENGTH('HELLO WORLD');` will return 11.

  - e.g. `SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;`

  - `SELECT CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long') FROM books;`

### UPPER() and LOWER()

Change A String's Case

- `SELECT UPPER('Hello World');`will return HELLO WORLD
- `SELECT LOWER('Hello World');`will return hello world
- `SELECT CONCAT ('MY FAVORITE BOOK IS ', UPPER(title)) FROM books;`

### DISTINCT

Remove the duplicate

- `SELECT DISTINCT author_lname FROM books;`

What if we want to distinct multiple entries? There are two ways:

- `SELECT DISTINCT CONCAT(author_fname, ' ', author_lname) FROM books;`
- `SELECT DISTINCT author_fname, author_lname FROM books; `

### ORDER BY

**By default, it is sorted in ascending order. If we want, we can also add `ASC` in the end. ** 

- `SELECT author_lname FROM books ORDER BY author_lname;`
- or `SELECT author_lname FROM books ORDER BY author_lname ASC;`

**If we want to sort it in descending order, add `DESC` in the end.**

- `SELECT author_lname FROM books ORDER BY author_lname DESC;`

There is also a short cut: 

- `SELECT title, author_fname, author_lname FROM books ORDER BY 2;`

  In this case, the 2 means the second argument in the statement, which is author_fname. 

If we need to sort different different columns:

- `SELECT title, author_fname, author_lname FROM books ORDER BY author_lname, author_fname;`

  In this case, we will sort the author_lname in ascending order first, then sort the author_fname in ascending order as well. 

### LIMIT

**Get the top K entries we needed! LIMIT comes last! ** 

- `SELECT title, released_year FROM books ORDER BY released_year LIMIT 10;`

  Get the top 10 latest released books from our database.

Another way to get the result:

- `SELECT title, released_year FROM books ORDER BY released_year LIMIT 0, 10;`

  0,10 represents that we start from the 1st elements (startpoint) and want to go for 10 rows (how many steps we take).

  **Here it is confusing since the first row in this statement is 0 instead of 1 in the string.** 

- If we want to go from the 5th entries, and go for the all rows in the table:

  - `SELECT title, released_year FROM books ORDER BY released_year LIMIT 4, 1385271589275983;`
  - 1385271589275983 just a random gigantic number that we know we will never reach

- Hence, `SELECT title, released_year FROM books ORDER BY released_year LIMIT 4, 100;` is also acceptable if our entries are less than 100 rows.

### LIKE

###### Better Search, wildcard

- `SELECT author_fname FROM books WHERE title LIKE '%da%';`
  - Here the % can represents anything (widecard), and can also contains zero characters
  - In this case, we are searching for author's first name which contains letters da in any places.
- If we change the code to: `SELECT author_fname FROM books WHERE title LIKE 'da%';`
  - We are trying to find author's first name which begins with letters da. 
- If we change the code to: `SELECT author_fname FROM books WHERE title LIKE '%da';`
  - We are trying to find author's first name which ends with letters da. 
- If we change the code to: `SELECT author_fname FROM books WHERE title LIKE 'da';`
  - <u>We are trying to find author's first name which is exactly the same as da.</u> 
- **NOTE: the LIKE statement is CASE-INSENSITIVE.** 



**Another type of wildcar: '' where we have 4 underscores. Each underscore represents exactly one entity.** 

- `SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '____'; `
  - This statement will give us the titles and stock quantities of books whose stock_quantity has exactly four digits. 



If we want to match a US phone number, for example (235) 234-0987, we can use `LIKE '(___)___-____';`



**What if we want to find an item which contains % or _ ?**

- We can use excape symbol: `WHERE title LIKE '%\%%'`, <u>we use a backslash to escape the %.</u>
- Or  `WHERE title LIKE '%\_%'`, <u>we use a backslash to escape _.</u> 



## Aggregation Function

### Count

- How many things are in the table: `SELECT COUNT(*) FROM books;` 
- How many author's first name are in the table: `SELECT COUNT(DISTINCT author_fname) FROM books;` 
  - Because we need to know the exact distinct number of author's name. 
- How many distinct author's are in the table: `SELECT COUNT(DISTINCT author_fname, author_lname) FROM books; `
- How many titles contain "the": `SELECT COUNT(*) FROM books WHERE title LIKE '%the%';`

### GROUP BY

`GROUP BY` summarizes or aggregates identical data into single rows 

It will return the super row that visually only contains one element each row. But inside each row, we will have several seperate rows. 

- `SELECT author_fname, author_lname, COUNT(*) FROM books GROUP BY author_lname, author_fname;`
  - it will give us how many  books each author has written

### MIN and MAX

- `SELECT MIN(released_year) FROM books` will return us the earliest year the book was released
- `SELECT MAX(pages) FROM books` will return the highest pages of book

**If we want to find the title of the longest books:** 

- If we try to use `SELECT MAX(pages), title FROM books` will not return the right answer.

  - **WHY?** 
    - This statement first finds the max pages in the pages column, and then find the first element in title pages -> simply combine them **(NOT CORRECT!)**

- Also, if we try to use `SELECT title, pages FROM books WHERE pages = MAX(pages);` won't work either. 

  - The reason for this problem actually lies in the behavior of how `WHERE` works in MySQL. The condition specified in the WHERE clause is checked for every row and finally the selected rows are aggreagated. Hence, for MySQL to be able to understand the condition `pages = MAX(pages)`, the value of MAX(pages) should already be known. This cretes a circular reference if an aggregate function is used inside the WHERE clause. 
  - To make it correct, use `SELECT title, pages FROM books WHERE pages = (SELECT MAX(pages) FROM books);`
    - This statement needs to run two quiries, which is slow.
    
    - How to make it faster? 
    
    - **`SELECT * FROM books ORDER BY pages DESC LIMIT 1;` will provide us with the result faster.** 
    
      

- If we want to find the year each author published their first book, we can use:

  ```mysql
  SELECT author_fname, author_lname, MIN(released_year)
  FROM books
  GROUP BY author_lname,author_fname;
  
  -- This statement will first group the author_lname and author_fname to a superrow. Then based on each superrow, we find the min released_year. 
  ```

  or we can also use:

  `SELECT author_fname, author_lname, released_year FROM books GROUP BY autho
  r_fname, author_lname ORDER BY released_year;`



### SUM

- Sum all the pages in the database: `SELECT SUM(pages) FROM books;`

**SUM with GROUP BY:**

- Sum all pages each author has written: `SELECT author_fname, author_lname, SUM(pages) FROM books GROUP BY author_fname, author_lname;`

### AVG

- Find the average pages of all books: ` SELECT AVG(pages) FROM books;`


**AVG with GROUP BY:**

- Find the average pages each author has written: `SELECT author_fname, author_lname, AVG(pages) FROM books GROUP BY author_fname, author_lname;`
- Calculate the average stock quantity for books released in the same year: `SELECT released-year, AVG(stock_quantity) FROM books GROUP BY released_year; `



## Logical Operators

- Not Equal: `!=` 

- Not Like: `NOT LIKE`

- Greater Than: `>` 

- Great Than or Equal to: `>=`

  - `SELECT 99 > 1;`

    - It will return 1, which means TRUE
    - If it is FALSE, it will return 0;
    - `SELECT 'a' = 'A';` will return TRUE. Since in SQL, it treats 'a' and 'A' the same element
  
- Logical AND: `&&` or `AND`

- Logical OR: `||` or `OR`

- BETWEEN (inclusive): `Between x AND y;` (We can not replace the AND by && in this case. Between and AND are paired together)

  - e.g. `SELECT title, released_year FROM books WHERE released_year BETWEEN 2004 AND 2015;` -> return us with books whose 2004 <= released_year <= 2015 

- NOT Between: 

  - e.g. `SELECT title, released_year FROM books WHERE released_year NOT BETWEEN 2004 AND 2015;` -> return us with books whose released_year < 2004 or  released_year > 2015

  - If we want to compare the date, we have to use `CAST()` to make sure that they are in the same data type. Otherwise, it will cause problems. 
    - How to use `CAST()`?
      - `SELECT CAST('1993-11-5' AS DATETIME);` will return us '1993-11-5 00:00:00'
  - IN and NOT IN: `IN`. `NOT IN` is just the opposite of `IN`

  ```mysql
  SELECT title, author_lname FROM books
  WHERE author_lname = 'Carver' OR
  			author_lname = 'Lahiri' OR 
  			author_lname = 'Smith';
  			
  # It is the same as: 
  
  SELECT title, author_lname FROM books
  WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');
  # (This version is much shorter!)
  
  SELECT title, released-year FROM books
  WHERE released_year NOT IN
  (2000, 2002, 2004, 2006, 2010, 2012, 2014, 2016);
  ```

-  CASE statement: `CASE`

  ```mysql
  SELECT title, released_year, 
  	CASE
  		WHEN released_year >= 2000 THEN 'Modern Lit'
  		ELSE '20th Century Lit'
  	END AS GRENRE
  FROM books;
  ```



## Relationship Basics

### One To Many

For example, each book has many reviews. 



- **Define Foreign Key**: 

  ```mysql
  CREATE TABLE orders(
  	...
  	customer_id INT,
  	FOREIGN KEY (customer_id) REFERENCES customers(id)
    # The cusomers here represents the other table and the id indicates the primary key of the customers table
  
  );
  ```



### JOINS

- **Cross Join**: simply multiply every row in the second table to the each row in the first table (basically meaningless): 

  - `SELECT * FROM cusomers, orders;`

- **Inner Join**: 

  ```mysql
  -- Implicit Inner Join
  SELECT first_name, last_name, order_date, amount FROM customers, orders
  WHERE customers.id = orders.customer_id;
  # add table names before the column we want to mention
  # to reduce the ambiguity
  
  -- Explicit Inner Join
  SELECT first_name, last_name, order_date, amount FROM customers
  INNER JOIN orders
  	ON customers.id = orders.customer_id; 
  	
  # 1. The order of the table matters! The table which comes first will appear on the left part of the joined table 
  # 2. We don't necessary need to write INNER JOIN as our keywords. Simply write JOIN will provide with with the correct result. 
  ```

  <img src="SQL_NOTES.assets/image-20200217100547900.png" alt="image-20200217100547900" style="zoom:25%;" />

- **Left Join**: 

  ```mysql
  SELECT * FROM customers
  LEFT JOIN orders
  	ON customers.id = orders.customer_id;
  	
  # We take everything from customers, and then match orders into customers. For some customers who don't have any orders, we will just put null. 
  
  # To avoid the null, we can use:
  
  SELECT
  	first_name,
  	last_name,
  	IFNULL(SUM(amount), 0) AS total_spent
  FROM customers
  LEFT JOIN orders
  	ON customers.id = orders.customer_id
  GROUP BY customers.id
  ORDER BY total_spent; 
  ```

  

  <img src="SQL_NOTES.assets/image-20200217101601034.png" alt="image-20200217101601034" style="zoom: 25%;" />

- **Right Join**:

  ```mysql
  SELECT * FROM customers
  RIGHT JOIN orders
  	ON customers.id = orders.customers_id
  ```

  

  <img src="SQL_NOTES.assets/image-20200217102837675.png" alt="image-20200217102837675" style="zoom:25%;" />

- **ON DELETE CASCADE**

  - If we want to delete a record in the user, however there are still some orders associate with this user. We cannot simply delete the user since MySQL will throw an error. 

  - Thus, in order to delete the item who has something associate with it, we need to use **ON DELETE CASCADE**

    ```mysql
    CREATE TABLE orders(
    	...
    	customer_id INT,
    	FOREIGN KEY (customer_id) 
      	REFERENCES customers(id)
      	ON DELETE CASCADE
      # When a customer is deleted, then orders associate with this user will also be deleted. 
    );
    ```

    

  

  











