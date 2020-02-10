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
  
  Now it is easier for us to do insertion since we do not need to remember cat's id:
  
  ```
  INSERT INTO unique_cats2(name, age) VALUES('Skippy', 3);
  INSERT INTO unique_cats2(name, age) VALUES('Skippy', 3);
  INSERT INTO unique_cats2(name, age) VALUES('Boloson', 2);
  INSERT INTO unique_cats2(name, age) VALUES('Nelsol', 7);
  ```
  



### CRUD commands (Create, Read, Update, Delete)

