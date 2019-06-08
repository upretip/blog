## SQL Codes Snippets for quick reminders

To be good at writing SQL queries one just needs to practice a lot. However, the following resource is gathered for quick reference. Tried to add texts wherever possible to help with `ctrl+F`


Things to be careful about
- _`syntax` errors_: spell things correctly 

Basic selection; retrun first 10

```
SELECT * 
FROM sales_table
LIMIT 10 ;
```

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
SELECT  sales_person_id, buyer_id, sales_unit
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

Sorting 
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

```
SELECT (sale_price * sale_volume) as tot_rev  --as to alias the new column
FROM sales_table ;
```

Group Summary statistics


```
SELECT SUM(sale_volume)
FROM sales_table
GROUP BY region;
```

Filter after grouping and aggregation

HAVING function removes data from the entire group
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

