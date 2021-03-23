---
layout: post
title: Adventures in PostgreSQL - Extract Epoch
---

Recently I needed to create a database query that compares the timestamps of several rows of data. It took me substantial detective work and googling, so I figured I'd document the solution in a blog post.

PostgreSQL provides various functions for working with `date` and `timestamp` datatypes. The `EXTRACT` method allows you to select a certain attribute from a piece of data.

<br/>
```sql
EXTRACT(field FROM source)
```

<br/>
From the documentation:
> The extract function retrieves subfields such as year or hour from date/time values. source must be a value expression of type timestamp, time, or interval. (Expressions of type date will be cast to timestamp and can therefore be used as well.) field is an identifier or string that selects what field to extract from the source value. The extract function returns values of type double precision.

<br/>
<br/>
Example:

```sql
SELECT EXTRACT(DAY FROM TIMESTAMP '2001-02-16 20:38:40');
Result: 16
```
<br/>
For my specific use case, I wanted to find the difference between timestamps, so I needed to use the `EPOCH` field. Extracting the epoch from a `date` or `timestamp` returns the number of seconds since `1970-01-01 00:00:00-00` (the Unix Epoch time).

Example:

```sql
SELECT EXTRACT(EPOCH FROM TIMESTAMP WITH TIME ZONE '2001-02-16 20:38:40-08');
Result: 982384720
```
<br/>
Specifically, I needed to determine (given a set of criteria) whether at least two records had been created 10 minutes apart from each other.

I ended up writing a subquery that compared the absolute differences of the timestamps. Here is an abbreviated version of the final query:

```sql
SELECT *
FROM fruits x
WHERE EXISTS (
  SELECT 0
  FROM fruits y
  WHERE ABS((EXTRACT (EPOCH from x.time))-(EXTRACT (EPOCH from y.time))) < 600
  AND ABS((EXTRACT (EPOCH from x.time))-(EXTRACT (EPOCH from y.time))) > 0
)
```

This query works well, but shoot me an email if you have other efficient and elegant solutions.

---
## SOURCES
<a href="https://www.postgresql.org/docs/8.1/functions-datetime.html" target="_blank">PostgreSQL</a>
