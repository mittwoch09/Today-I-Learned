### Create sample table

```sql

DROP TABLE IF EXISTS T;
CREATE TABLE T (a int PRIMARY KEY,
                b text,
                c boolean,
                d int);

INSERT INTO T VALUES
  (1, 'x',  true, 10),
  (2, 'y',  true, 40),
  (3, 'x', false, 30),
  (4, 'y', false, 20),
  (5, 'x',  true, NULL);
```

|a|b|c    |d |
|-|-|-----|--|
|1|x|true |10|
|2|y|true |40|
|3|x|false|30|
|4|y|false|20|
|5|x|true |  |

### DISTINCT ON

<img width="500" alt="Screen Shot 2022-05-09 at 4 22 02 PM" src="https://user-images.githubusercontent.com/73784742/167369660-973f9248-5890-4bf4-8a47-f4a0f53ca5ea.png">

<img width="500" alt="Screen Shot 2022-05-09 at 4 22 29 PM" src="https://user-images.githubusercontent.com/73784742/167369718-509e449a-a6e6-465e-ad42-f5c9a0fc3aab.png">


```sql
SELECT DISTINCT ON (t.c) t.*
FROM T AS t
ORDER BY t.c, t.d ASC;
```

|a|b|c    |d |
|-|-|-----|--|
|4|y|false|20|
|1|x|true |10|

Keep the d-smallest row for each of the two false/true groups.

In absence of ORDER BY, we get any representative from the two groups.

### Ordered Aggregates

<img width="500" alt="Screen Shot 2022-05-09 at 4 54 47 PM" src="https://user-images.githubusercontent.com/73784742/167375660-0b188a97-7cf1-402f-8b93-83d06f281899.png">

```sql
SELECT string_agg(t.a :: text, ',' ORDER BY t.d) AS "all a"
FROM T AS t;
```

|all a    |
|---------|
|1,4,3,2,5|

Ordered aggregate (',' separates the aggregated string values)

### Filtered and Unique Aggregates

```sql
SELECT SUM(t.d) FILTER (WHERE t.c) AS picky,
       SUM(t.d) AS "don't care"
FROM   T As t;
```

|picky|don't care|
|-----|----------|
|   50|       100|

The query below implements the same filtered aggregration.

```sql
SELECT SUM(CASE WHEN t.c THEN t.d ELSE 0 END) AS picky,
       SUM(t.d) AS "don't care"
FROM   T As t;
```

|picky|don't care|
|-----|----------|
|   50|       100|

Simple pivoting

```sql
SELECT SUM(t.d) FILTER (WHERE t.b = 'x')            AS "∑d in region x",
       SUM(t.d) FILTER (WHERE t.b = 'y')            AS "∑d in region y",
       SUM(t.d) FILTER (WHERE t.b NOT IN ('x','y')) AS "∑d elsewhere"
FROM   T As t;
```

|d in region x|∑d in region y|∑d elsewhere|
|-------------|--------------|------------|
|           40|            60|            |

Unique aggregate

```sql
SELECT COUNT(DISTINCT t.c) AS "#distinct non-NULL",
       COUNT(t.c)          AS "#non-NULL"
FROM   T as t;
```

|distinct non-NULL|#non-NULL|
|------------------|---------|
|                 2|        5|
