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

<img width="565" alt="Screen Shot 2022-05-09 at 4 22 02 PM" src="https://user-images.githubusercontent.com/73784742/167369660-973f9248-5890-4bf4-8a47-f4a0f53ca5ea.png">

<img width="567" alt="Screen Shot 2022-05-09 at 4 22 29 PM" src="https://user-images.githubusercontent.com/73784742/167369718-509e449a-a6e6-465e-ad42-f5c9a0fc3aab.png">


```sql
SELECT DISTINCT ON (t.c) t.*
FROM   T AS t
ORDER BY t.c, t.d ASC;
```

|a|b|c    |d |
|-|-|-----|--|
|4|y|false|20|
|1|x|true |10|

Keep the d-smallest row for each of the two false/true groups.

In absence of ORDER BY, we get any representative from the two groups.
