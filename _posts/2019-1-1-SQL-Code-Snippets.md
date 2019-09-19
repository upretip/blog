## SQL Codes Snippets for quick reminders

To be good at writing SQL queries one just needs to practice a lot. However, the following resource is gathered for quick reference. Tried to add texts wherever possible to help with `ctrl+F`


Things to be careful about
- _`syntax` errors_: spell things correctly 
- _`appearance`_: Write sql commands in uppercase, new line if multiple columns in select statement. One line per function.

Basic selection; retrun first 10

SELECT  

```
SELECT * 
FROM sales_table
LIMIT 10 ;
```


WHERE 

Filter using single criteria

```
SELECT sales_person_id
FROM sales_table
WHERE sales_revenue > 100 ;
```

Filtering using multiple criteria

 <!-- Comparison Operators  

    operators | uses  
    ----------|-------  
    =      |  equals
    != , <>|  not equal -->


```
SELECT  sales_person_id, 
        buyer_id, 
        sales_unit
FROM sales_table
WHERE sales_price BETWEEN 100 AND 120
AND buyer_last_name LIKE %ik%  
AND buyer_first_name <> 'Sam';
```

Grouping using AND OR  
- Wildcard characters `%` to match multiple character and `_` for single

```
SELECT * 
FROM sales_table
WHERE (region = "East") 
AND (sale_price >50 OR sale_volume < 90);
```
ORDER BY   

Sorting using order by after selecting/aggregating
```
SELECT * 
FROM sales_table
WHERE (region = "East") 
AND (sale_price >50 OR sale_volume < 90)
ORDER BY sale_price DESC;
```

### Aggregating and Summarizing

Functions include
- COUNT
- DISTINCT
- SUM
- AVG
- MIN
- MAX
- TOTAL

Counting the number of rows

```
SELECT count(*)
FROM sales_table;
```

Minimum/Maximum, average, etc. value from a column
```
SELECT min(sale_price), max(sale_volume), AVG(sale_units)
FROM sales_table;
```

Sum of column as integer

```
SELECT SUM(total)
FROM sales_table;
```

Sum of column as float:
```
SELECT TOTAL(total)
FROM sales_table ;
```

Select unique values in column

```
SELECT DISTINCT(buyer_last_name)
FROM sales_table ;
```

Perform mathematical operation between two columns

Note:  
Use CAST to get two columns in same data type. Also, in order to avoid NULL while doing operations, use COALESCE(column_name, 0) to convert NULL to zero. 

```
SELECT (sale_price * sale_volume) as tot_rev  --as to alias the new column
FROM sales_table ;
```

GROUP BY  

Group Summary statistics


```
SELECT SUM(sale_volume)
FROM sales_table
GROUP BY region;
```
HAVING   

Filter after grouping and aggregation. HAVING function removes data from the entire group
```
SELECT AVG(sale_volume)/AVG(days) as sale_per_day
FROM sales_table
GROUP BY region
HAVING region LIKE 'e' ; -- note this is filtering using the new column 
```

Rounding 

```
SELECT ROUND(AVG(sale_price), 2) as rounded_avg_sale_price
FROM sales_table;
```

Change data type using CAST

```
SELECT CAST(sale_price as varchar(10))
FROM sales_table;
```

CASE WHEN  

Control flow to check whether certain conditions are met, if so follow the commands. Can use it on select or order by 

```
SELECT col_1
CASE 
    WHEN col_1 < 10 THEN  "Small"
    WHEN col_1 >=10 and col_1 <50 "Medium"
    ELSE "Large"
END as new_col
FROM table_1;
```

More things to add:
- Character functions (LOWER, UPPER, INITCAP, LPAD, RPAD, LTRIM, RTRIM, TRIM, SUBSTR, INSTR, LENGTH, CONCAT, REPLACE, TRANSLATE, SOUNDEX)
- Number functions (ABS, MOD, REMAINDER, TRUNC, FLOOR, CEIL, round)
- MISC functions 
    - NVL - replace null value
    - COALESCE - replace null value with multiple substitution expressions
    - NVL2 - null and not null substitution replacement
    - LNNVL - return opposite condition
    - NANVL - retrun substitution value in case of NAN
    - DECODE - substituion based on if-else-then
    - CASE - check for different cases eg. case when  cond then return val
- Date functions ( so many date time, interval functions and datatype)
    - TO_CHAR
    - TO_DATE
    - SYSDATE
    - CURRENT_DATE
    <!-- - DATE
    
    YYYY
    YEAR
    RR
    MM
    MON
    MONTH
    Month
    DD
    DAY
    DY
    D
    DDD
    DL
    HH or HH12
    HH24
    MI
    SS
    SSSSS
    AM or PM
    TS
    WW
    W
    Q
    -->  
- Date Search    
- Date time math (ROUND, EXTRACT, ADD_MONTHS, MONTHS_BETWEEN, LAST_DAY, NEXT_DAY, TRUNC) 
- Timezone and time-stamp (TIMESTAMP, TIMESTAMP WITH TIME ZONE, TIMESTAMP WITH LOCAL TIME ZONE)

### Join multiple tables

Concepts addressed:
- Table Aliases - can give the input tables aliases to save typing
- Can use ON or USING keywords 
    - USING - if the column names are same. Do not have to use the equality syntax
    - ON - can use if the column names are same or not. Need to prefix column with table name or alias. If selecting the join column, the select clause needs to be prefixed too.
- Cartesian Product or CROSS JOIN - literally what it sounds like: many-many relationship. In SQL, all combinations of given join values in two tables

(INNER) JOIN -  Keeps only common rows

```
SELECT course_id, s.section_no, c.description, 
    s.location, s.instructor_id
FROM course c JOIN section s
USING (course_id)

-- in the second line two tables, courses and sections are aliased
```

OUTER JOIN - Keeps rows incuding those that are not shared commonly
```
SELECT course_id, s.section_no, c.description, 
    s.location, s.instructor_id
FROM course c OUTER JOIN section s
ON course_id
```

NATURAL JOIN - Join tables based on all common columns. 
No need to specify ON or USING.   
If any more than one columns are common, values in all common columns must match 
```
SELECT course_no, s.section_no, c.description, 
    s.location, s.instructor_id
FROM course c NATURAL JOIN section s
```
Joining three or more tables  
The steps are similar to joining two tables but needs more repetitions. Queries can be optimized for performance if good undrestanding of database indexing.

```
SELECT course_id, s.section_no, c.description, 
    s.location, s.instructor_id, i.first_name, i.last_name
FROM course c JOIN section s
USING (course_id)
JOIN instructor i
USING (instructor_id)

```

### Complex Queries

_Subqueries_

In SQL, subqueries are used instead of  placeholder variables. The subqueries may get very complex, so it is important to understand how SQL performs computation


Readable SQL Queries/Subqueire:

- Capitalize SQL function names (SELECT, CASE WHEN, SUM, AVG, INNER JOIN, etc.)
- Indent columns in select statements and put each column in new line
- Put each SQL statement in a new line
- Use indentation to make subqueries look logically separate
- Use understandable aliases and indexing

Aliasing Subqueries

```
WITH [Alias] as [subquery]
SELECT * from [Alias]
```

Can nest multiple named subqueries at once too

```
WITH [subquery1] AS
    (SELECT * from Table1),
    [subquery2] AS
    (SELECT * from Subquery1 where ...),
    [subquery3] AS
    (SELECT * from Subquery2 Where ....)
```

Creating permanent view to use it in the future

```
CREATE VIEW dbname.viewname AS
    SELECT [columns] from dbname.table;
```
Drop the view similar to drop table

```
DROP VIEW dbname.viewname
```

UNION, INTERSECT and EXCEPT


To perform these operation, the two resulting columns must be of compatible data type and have same number of columns. Compatible data type example (float and int) but not (text and int)

```
SELECT * FROM table.A
UNION
SELECT * FROM table.B

SELECT * FROM table.A
INTERSECT
SELECT * FROM table.B

SELECT * FROM table.A
EXCEPT
SELECT * FROM table.B


```
SQLITE SCHEMA

SCHEMA of a table

```
PRAGMA TABLE_INFO(table_name)
```

```
SELECT
    name,
    type
FROM sqlite_master
WHERE type IN ("table","view");
```

Create Database/ Drop database

```
CREATE DATABASE [dbname] OWNER [username]
DROP DATABASE [dbname]
```

INDEX

- When there are two possible indexes available, SQLite tries to estimate which index will result in better performance. However, SQLite is not good estimating and will often end up picking an index at random.
- Use a multi-column index when data satisfying multiple conditions, in multiple columns, is to be retrieved.
- When creating a multi-column index, the first column in the parentheses becomes the primary key for the index.
- A covering index contains all the information necessary to answer a query.
- Covering indexes don't apply just to multi-column indexes.


```
CREATE INDEX  IF NOT EXISTS [index_name] on [table_name](column_name)
CREATE INDEX IF NOT EXISTS [multi_index_name] on [table_name](column1, column2)
```

Explain query plan

```
EXPLAIN QUERY PLAN SELECT * FROM [table_name]
```
before and after indexing a column



# SQLite Normalization Create and modify database

Start the command line with sqlite3 command and dbname as argument. Use ";" to end query

sqlite has some dot commands that can be used to turn on and off some features eg `.headers on`, `.mode column`, `.help`, `.tables` `.shell [command 'ls' ,etc.]`, `.quit `

```
$> sqlite3 chinook.db
$> .headers on
$> .mode column
$> select * fom student;
$> .shell clear
```

- create new table   
`CREATE TABLE [table_name] ([col1] [coltype], [col2] [coltype],...);`

- create table with primary, foreign key  
`CREATE TABLE [table_name] ([col1][coltype], [col2][coltype],...,PRIMARY KEY (colx, coly), FOREIGN KEY [colm] REFERENCES [another table]([another column]));`

- inserting values   
`INSERT INTO [table_name]{(columns)} VALUES (value1, value2, ...) `  

- Delete rows of data   
`DELETE FROM [table_name] {WHERE [expression]}`

- Adding columns to existing table  
`ALTER TABLE [table_name] ADD COlUMN [column1] [coltype], [column2][coltype],...;`

- Changing values in a table  
`UPDATE [table_name] SET [column_name] = [value] {WHERE [expression]}`


# PostgreSQL in command line

Things to know. Default port 5432, Default user 'postgres'. Don't forget `;` to execute query

Installation:  
- [Download and install](http://www.bigsql.org/postgresql/installers.jsp) 

POSTGRES special commands start with backslash `\`
Run on console
```
>$ psql -U postgres  --start psql with default user
>$ \l
>$ \dt
>$ \du
>$ \dp
>$ \q --quit console
```

Create roles etc. Use comma ',' for multiple permissions.
Later create groups add user to group etc. 

```
CREATE ROLE [username] WITH [options {SUPERUSER CREATEDB LOGIN PASSWORD 'password'}]
GRANT [PERMISSION {ALL PRiVILEGES SELECT INSERT UPDATE DELETE }] ON [tablename] TO [username]
```

Revoke permissions

```
REVOKE [PERMISSION {SELECT UPDATE INSERT DELETE}] on [tablename] FROM  [username]
REVOKE ALL PRIVILEGES on [table] from [username]
```