# MySQL

## Table of Contents
- [SQL Data Types](#sql-data-types)
- [SQL Queries](#sql-queries)
  - [Database and Table](#database-and-table)
  - [Insert to DB](#insert-to-db)
  - [Query DB](#query-db)
  - [Manipulate DB](#manipulate-db)
- [Primary Key and Foreign Key](#primary-key-and-foreign-key)

## SQL Data Types
- **`VARCHAR(n)`**
  - Variable-length strings where `var` means you only store what you entered (incl. extra 2 bytes to store a length up to 65,535).
  - `n` - maximum number of characters that can be stored.
  - `0 <= n <= 65,535`.
- **`CHAR(n)`**
  - `n` - the number of characters that will be stored.
  - `0 <= n <= 255`.
- **BLOB**
  - A Binary Large OBject that are treated as binary/byte strings.
- **DEC(m, d)**
  - `m` - the maximum number of total digits, including `d` (default 10).
    - `1 <= m <= 65`.
  - `d` - the number of digits to the right of the decimal point (default 0).
- **INT[(m)]**
  - `m` - the number of bits per value.
    - `1 <= m <= 64`.
- **DATE**
  - A date expressed as `'YYYY-MM-DD'`.
- **DATETIME**
  - A date and time expressed as `'YYYY-MM-DD HH:MM:SS'`.
- **TIMESTAMP**
  - A timestamp expressed as `'YYYY-MM-DD HH:MM:SS.fraction'`.
- **TIME**
  - A time expressed as `'HH:MM:SS[.fraction]'`.

## SQL Queries
### Database and Table
- **Create Database**
  ```sql
  CREATE DATABASE <db_name>;
  ```
- **Use Database**
  ```sql
  USE <db_name>;
  ```
- **Create Table**
  ```sql
  CREATE TABLE <table_name> (
    <field1> <field1Type> [NOT NULL] [DEFAULT <field1Default>],
    <field2> <field2Type> [NOT NULL] [DEFAULT <field2Default>]
  );
  ```
- **Check Table**
  ```sql
  DESC <table_name>;
  ```
- **Remove Table**
  ```sql
  DROP TABLE <table_name>;
  ```
### Insert to DB

- **Add Data to Table**
  ```sql
  INSERT INTO <table_name>
    (<field1>, <field2>) VALUES
    (<field1Data>, <field2Data>);
  ```
### Query DB
- **Query Table**
  ```sql
  SELECT * from <table_name>;
  ```
- **Conditional Query**
  ```sql
  SELECT * FROM <table_name> WHERE <condition>;
  ```
  - Ex: `SELECT * FROM user_table WHERE name = 'Kakamotobi';`.
- **Specific Column Query**
  ```sql
  SELECT <field1>, <field2> from <table_name>;
  ```
- **`AND`**
  ```sql
  SELECT <field1> from <table_name> where <condition1> AND <condition2>;
  ```
  - Ex: `SELECT name FROM user_table WHERE age < 3 AND height >= 200;`.
  - Ex: `` SELECT name FROM user_table WHERE name >= `K` AND name < `P`; ``.
- **`BETWEEN`**
  ```sql
  SELECT <field1> from <table_name> WHERE <field2> BETWEEN <condition>;
  ```
  - Ex: `SELECT name FROM user_table WHERE age BETWEEN 3 AND 5;`.
- **`OR`**
  ```sql
  SELECT <field1> from <table_name> WHERE <condition1> OR <condition2>;
  ```
  - Multiple `OR`s.
    ```sql
    SELECT <field1> from <table_name> WHERE <condition1> OR <condition2> OR <condition3>;
    ```
    - Alternative using **`IN`**
      ```sql
      SELECT <field1> from <table_name> WHERE <field2> IN (<condition1>, <condtion2>, <condition3>);
      ```
- **Finding `NULL` ft. `IS`**
  ```sql
  SELECT <field1> from <table_name> WHERE <field2> IS NULL;
  ```
- **`LIKE`**
  - Find a record using a substring.
  ```sql
  SELECT * FROM <table_name> WHERE <field1> LIKE '<substring>';
  ```
  - Ex: `` SELECT * FROM user_table WHERE name LIKE `%son`; ``.
    - Finds all users with names that end with "son".
    - _Note: `son%` for starting with "son"._
- **`NOT`**
  - Query for records that are NOT the provided conditions.
    ```sql
    SELECT <field1> from <table_name> WHERE <field2> NOT IN (<condition1>, <condition2>, <condition3>);
    ```
  - `NOT` + `BETWEEN` + `AND`.
    ```sql
    SELECT <field1> from <table_name> WHERE NOT <field2> BETWEEN <condition1> AND <condition2>;
    ```
  - `NOT` + `LIKE` + `AND`.
    ```sql
    SELECT <field1> from <table_name> WHERE NOT <field1> LIKE '<substring>' AND NOT <field2> LIKE '<substring>';
    ```
### Manipulate DB
- **`DELETE`**
  - Used to remove rows, not columns.
  - The number of rows to remove depends on the `WHERE`.
  - **Delete All Rows**
    ```sql
    DELETE FROM <table_name>;
    ```
  - **Delete Conditionally**
    ```sql
    DELETE FROM <table_name> WHERE <condition>;
    ```
- **Renewing a Record**
  - **`INSERT` and `DELETE` Combo**
    - First, add a new record.
      ```sql
      INSERT INTO <table_name> VALUES (<values>);
      ```
    - Then, delete the "stale"/old record.
      ```sql
      DELETE FROM <table_name> WHERE <field1> = <old_value>;
      ```
  - **`UPDATE`**
    - An alternative to the above approach.
    ```sql
    UPDATE <table_name> SET <field1> = <new_value> WHERE <field1> = <old_value>;
    ```
    - **Updating Columns**
      ```sql
      UPDATE <table_name> SET <column1> = <new_value>, <column2> = <new_value>;
      ```
      ```sql
      UPDATE <table_name> SET <column1> = <new_value> WHERE <column1> = <old_value>;
      ```
    - **Arithmetic Operations with `UPDATE`**
      ```sql
      UPDATE <table_name> SET <field1> = <field1> <arithmetic operation>;
      ```
      - Ex: `UPDATE user_table SET age = age + 1;`.

## Primary Key and Foreign Key
- Data between the primary key and foreign key corresponds to each other.
### Primary Key
- **The Primary Key is a column that uniquely identifies each row in a table.**
  - MySQL creates an index for columns that are primary keys for quicker lookups.
  - There can be multiple primary keys.
- Set the primary key when creating a table.
  ```sql
  CREATE TABLE <table_name> (<field1> <dataType> PRIMARY KEY, <field2> <dataType>, <field3> <dataType>)
  ```
- Set the primary key after creating a table.
  ```sql
  ALTER TABLE <table_name> ADD PRIMARY KEY (<fieldname>);
  ```
### Foreign Key
- **A Foreign Key is a primary key (hence, column) from another table that can be seen in a different table.**
- Foreign Keys allow us to link between two tables.
  - A foreign key can be used to get more related information from its original table (where the key is the primary key).
  - There can be multiple foreign keys.
- Set the foreign key when creating a table.
  ```sql
  CREATE TABLE <table_name> (<field1> <dataType> PRIMARY KEY, <field2> <dataType>, FOREIGN KEY(<field3>) REFERENCES <other_table>(<field3>));
  ```
- Set the foreign key after creating a table.
  ```sql
  ALTER TABLE <table_name> ADD FOREIGN KEY (<field1>) REFERENCES <other_table>(field1);
  ```

## Reference
[MySQL :: MySQL 8.0 Reference Manual :: 11 Data Types](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
