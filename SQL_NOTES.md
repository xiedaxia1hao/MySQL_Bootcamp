This note is derived from the Udemy Course: [The Ultimate MySQL Bootcamp: Go from SQL Beginner to Expert][https://www.udemy.com/course/the-ultimate-mysql-bootcamp-go-from-sql-beginner-to-expert/]. I just make it for my self-study and review purpose. For anyone who many needs the quick notes of SQL, please feel free to use this one, and I am happy to share it to the world : )



Onwards! 

---





# SQL NOTES

### Common-used Data Type

- INT: 
- VARCHAR (between 1 and 255 characters): e.g. ``varchar(100)``, ``varchar(254)``

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













