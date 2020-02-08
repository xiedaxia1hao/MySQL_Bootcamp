This note is derived from the Udemy Course: [The Ultimate MySQL Bootcamp: Go from SQL Beginner to Expert][https://www.udemy.com/course/the-ultimate-mysql-bootcamp-go-from-sql-beginner-to-expert/]. I just make it for my self-study and review purpose. For anyone who many needs the quick notes of SQL, please feel free to use this one, and I am happy to share it to the world : )



Onwards! 

---





# SQL NOTES



- Start SQL Command Tool: ``mysql-ctl``, then we can see all options under this command: start, install, stop, status, etc.

  - Start SQL server: ``mysql-ctl start``
  - Stop SQL server: ``mysql-ctl stop``

- Enter into SQL shell: `` mysql-ctl cli``, which means comand line interface (cli)

- Quit the shell: ``EXIT``

- Show all databases in the server: ``SHOW DATABASES;``

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
  - 



# Common-used Data Type

- INT: 
- VARCHAR (between 1 and 255 characters): e.g. ``varchar(100)``, ``varchar(254)``