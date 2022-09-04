---
title: SQL Tutorial
date: 2022-04-19 12:22:08
description: This blog contains a detailed tutorial on SQL
tags: 
 - sql
 - data_science
 - bigquery
icon: material/emoticon-happy
status: Inprogress
external_references: 
 - https://count.co/sql-resources
urls: 
aliases: 
---

This tutorial is designed for people who want to answer questions with data. SQL is used for accessing, cleaning, and analyzing data that's stored in databases. It's very easy to learn, yet it's employed by the world's largest companies to solve incredibly challenging problems.

> Why should I learn SQL when I already know Pandas : [[Big Query Vs Tensorflow Transform]]

## Generic SQL Select statement

The Generic sql select statement is made up of 4 elemental clauses  

```sql
select
  -- (some columns)
from
  -- (some tables)
  -- (a subquery)
  -- (a JOIN clause)
where
  -- (A 'predicate' expression to be used as a filter)
group by
  -- (some columns)
```

> Code snippets will be written in the Google BigQuery Standard SQL syntax

---

### From Clause

The sources defined in the FROM clause determines the set of rows that the SELECT statement can operate on hence it is the first part of the query to be executed.

#### Table as input

```sql
with my_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])) 
-- ^ The SELECT above is just defining some data to use
select
  *
from
  my_table
```

#### Subquery as input

```sql
select
  *
from
  (select * from unnest([
    struct('a' as strings, 1 as numbers),
    struct('b' as strings, 2 as numbers),
    struct('c' as strings, 3 as numbers)
  ])) 
```

> Instead of referring to a table by name, here we are refer the result of a SELECT statement  which is a tables

Above query can also be written using a WITH clause and multiple nested subqueries

```sql
with my_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])) 
select
  *
from
  (
    select * from (
      select * from (
        select * from my_table
      )
    )
  )
```

#### Join clause as input

To select rows from multiple tables, first the JOIN clause is used to  merge the tables together and the result of this clause is used as input to the FROM keyword

```sql
with table_1 as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])),
table_2 as (select * from unnest([
  struct('a' as strings_2, 4 as numbers_2),
  struct('b' as strings_2, 5 as numbers_2),
  struct('c' as strings_2, 6 as numbers_2)
]))
select
  numbers,
  numbers_2
from 
  table_1
  left join table_2 on table_1.strings = table_2.strings_2
```

Deciphering the above SELECT statement :
- We want the *numbers* and *numbers_2* columns (the database is smart enough to know which tables those columns come from)
- FROM *table_1* and *table_2*
- The rows of *table_2* should be associated with those from *table_1* by comparing the columns *strings* and *strings_2*

---
### Join Clause

JOIN clause is used in a SELECT statement to retrieve rows from multiple tables at the same time. It tells the database exactly how it should match those tables together. Following are the 5 Types of JOIN clause used in SQL 

#### Left Join
```sql
with left_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])),
right_table as (select * from unnest([
  struct('a' as strings, 4 as numbers),
  struct('b' as strings, 5 as numbers)
]))

select
  *
from
  left_table
left join
  right_table on left_table.strings = right_table.strings
```

Result of this query is :

- All columns from *table_1* and *table_2*
- All rows from *table_1*
- All rows from *table_2* where *column_2* matches the column *column_1* from *table_1*
- The table on the left of the join clause keeps all of its rows, even if there isn't a matching row in the table on the right. If there is no matching row in the table on the right, the columns from the right will be filled with NULLs.

#### Right Join
```sql
with left_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers)
])),
right_table as (select * from unnest([
  struct('a' as strings, 4 as numbers),
  struct('b' as strings, 5 as numbers),
  struct('c' as strings, 6 as numbers)
]))

select
  *
from
  left_table
right join
  right_table on left_table.strings = right_table.strings
```

Result of this query is :

- All columns from *table_1* and *table_2*
- All rows from *table_2*
- All rows from *table_1* where *column_1* matches the column *column_2* from *table_2*
- The table on the right of the join clause keeps all of its rows, even if there isn't a matching row in the table on the left. If there is no matching row in the table on the left, the columns from the left will be filled with NULLs.

#### Inner Join
```sql
with left_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])),
right_table as (select * from unnest([
  struct('a' as strings, 4 as numbers),
  struct('b' as strings, 5 as numbers),
  struct('d' as strings, 6 as numbers)
]))

select
  *
from
  left_table
inner join
  right_table on left_table.strings = right_table.strings
```

Result of this query is :

- All columns from *table_1* and *table_2*
- All rows from both tables where the values in *column_1* and *column_2* both match and exist
- Only the rows which match from both tables will be included in the result. The values 'c' and 'd' won't appear in the output, as those values don't exist in both tables.

#### Outer Join
```sql
with left_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers),
  struct('c' as strings, 3 as numbers)
])),
right_table as (select * from unnest([
  struct('a' as strings, 4 as numbers),
  struct('b' as strings, 5 as numbers),
  struct('d' as strings, 6 as numbers)
]))

select
  *
from
  left_table
full outer join
  right_table on left_table.strings = right_table.strings
```

Result of this query is :

- All columns from table_1 and table_2
- All rows from both tables
- All rows will be included in the result, even if they don't exist in one of the tables. If there is no matching row in one of the tables, the missing columns will be filled with NULLs. Values 'c' and 'd' will appear in the output, even though they only exist in one table each.

#### Cross Join
```sql
with left_table as (select * from unnest([
  struct('a' as strings, 1 as numbers),
  struct('b' as strings, 2 as numbers)
])),
right_table as (select * from unnest([
  struct('a' as strings, 4 as numbers),
  struct('b' as strings, 5 as numbers),
  struct('c' as strings, 6 as numbers)
]))

select
  *
from
  left_table
cross join
  right_table
```

Result of this query is :

- All columns from *table_1* and *table_2*
- All possible combinations of rows from both tables
- Number of rows returned is the product of the number of rows of the two tables. Number of rows in the output would be  2 x 3 = 6

---
### Where Clause

The WHERE clause is followed by a predicate expression, an expression which returns true or false.  For a given row, if the predicate returns true then, that row is included in the output of the query else that row is excluded.
#### Predicate Expression
To create a predicate expression we can use the following comparitives

- Inequality
- Equality
- IS NOT NULL
- IS NULL
- IN
- BETWEEN
- AND / OR

```sql
with my_table as (select * from unnest([null,1,2,3,4,null,5]) as numbers)
select numbers from my_table
where
  --- One of the below statements
  numbers > 2
  numbers = 2
  numbers > (select 1+1)
  numbers is null
  numbers is not null
  numbers in (1,2)
  numbers between 2 and 4 -- Note - these limits are inclusive
  numbers > 2 and (numbers < 3 or numbers > 4)
```

Predicate Expression can also contain Functions 

```sql
with my_table as (select * from unnest(['a', 'b', 'ab', 'aa', 'bb', 'cc']) as strings)
select strings from my_table
where
  -- Predicate expressions can contain functions
  (strings like 'a%' or strings like 'b%') and length(strings) = 2
```

---
### Date and Time 

> Date and time functionality and syntax differs between databases. In this article the code snippets are written in the Google BigQuery Standard SQL syntax

#### Basic Datetime data types
- DATE : calendar date
- DATETIME : calendar date and time
- TIMESTAMP : a particular moment in time, default timezone UTC 
- TIME : a time as seen on a watch

```sql
select
  date('2020-01-01') as date,
  datetime(2020, 1, 1, 0, 0, 0) as datetime,
  timestamp('2020-01-01T00:00:00.000Z') as timestamp,
  time(0, 0, 0) as time
```


#### Basic Functions

- Cast is used to change datatype of a variable explicitly from one data type to another

```sql
select
  -- string -> date
  cast('2020-01-01' as date) as date, 
  -- string -> datetime,
  cast('2020-01-01' as datetime) as datetime,
  -- string -> timestamp,
  cast('2020-01-01' as timestamp) as timestamp,
  -- string -> datetime -> date. 
  cast(cast('2020-01-01 00:00:00' as datetime) as date) as date_2,
```

- parse_date is used to convert a string to Date

```sql
select
parse_date('%Y-%m-%d', '2020-01-01') as iso_date
```

- timestamp_seconds and date_from_unix_date are used to convert a number to Date

```sql
select
  -- This is the number of seconds between the start of 1970 and 2020
  -- This function returns a timestamp
  timestamp_seconds(1577836800) as timestamp,
  -- This is the number of days between the start of 1970 and 2020
  -- This function returns a date
  date_from_unix_date(18262) as date,
```

- format_date is used to format dates and times

```sql
with my_date as (
  select
    date('2020-01-01') as date,
)
select
  format_date('%Y', date) as year_only,
  format_date('%Y-%m', date) as year_month,
  format_date('%b-%d', date) as month_day,
from 
  my_date
```

- date_add and date_sub are used in arithmetic operations in date time

```sql
with my_date as (
  select
    date('2020-01-01') as date,
)
select
  -- This is special BigQuery syntax for intervals
  date_add(date, interval 1 day) as tomorrow,
  date_sub(date, interval 1 day) as yesterday,
  date_add(date, interval 10 year) as ten_years_away,
from 
  my_date
```


