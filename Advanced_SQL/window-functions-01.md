## Window Functions

<img width="500" alt="Screen Shot 2022-06-06 at 2 20 29 PM" src="https://user-images.githubusercontent.com/73784742/172106363-d28af86f-72f6-4db8-90c9-917192edb159.png">

### Window Frame Specifications (Variant: ROWS)

<img width="500" alt="Screen Shot 2022-06-06 at 2 33 21 PM" src="https://user-images.githubusercontent.com/73784742/172107984-d6442643-bee0-4908-a5d0-3d58cbf9f6a7.png">

**BETWEEN**                 **AND**
       **UNBOUNDEDE PRECEDING** **CURRENT ROW**  
       **n PRECEDING**          **n FOLLOWING**
       **CURRENT ROW**          **UNBOUNDED FOLLOWING**

### Window Frame Specifications (Variant: RANGE)

<img width="500" alt="Screen Shot 2022-06-06 at 2 45 14 PM" src="https://user-images.githubusercontent.com/73784742/172109702-5fd5c760-8999-4251-9f2d-83d6379ca7ce.png">

**distance measured in terms of |current row - i|**

### Window Frame Specifications (Variant: GROUPS)

<img width="500" alt="Screen Shot 2022-06-06 at 2 51 18 PM" src="https://user-images.githubusercontent.com/73784742/172110451-77ae4c5e-791e-4168-b486-e8fdaca4e3ca.png">

**distance measured in number of peer groups**

### Window Frame Specifications: Abbreviations

<img width="500" alt="Screen Shot 2022-06-06 at 3 57 34 PM" src="https://user-images.githubusercontent.com/73784742/172119893-5e159f18-7116-4f84-90f9-c3b889a28e30.png">

**OVER () = all rows form the window frame(for any current row)**

**OVER (ORDER BY A) = OVER (ORDER BY A RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)**

### WINDOW Clause: Name the Frame

<img width="500" alt="Screen Shot 2022-06-06 at 3 58 35 PM" src="https://user-images.githubusercontent.com/73784742/172120051-17271042-7ccd-4ee2-9a2a-2b2efef658f2.png">

```sql
DROP TABLE IF EXISTS W;
CREATE TABLE W (
	row text PRIMARY KEY,
	a   int,
	b   text
);

INSERT INTO W(row, a, b) VALUES
	('q1', 1, 'O'),
	('q2', 2, 'X'),
	('q3', 3, 'X'),
	('q4', 3, 'O'),
	('q5', 3, 'X'),
	('q6', 4, 'X'),
	('q7', 6, 'O'),
	('q8', 6, 'O'),
	('q9', 7, 'X');
```

```sql
SELECT w.row                     AS "current row",
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS ();
```

|current row|frame size|rows in frame               |
|-----------|----------|----------------------------|
|q1         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q2         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q3         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q4         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q5         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q6         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q7         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q8         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|
|q9         |         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|

```sql
SELECT w.row                        AS "current row",
	   w.a,
	   SUM(w.a)            OVER win AS "SUM (a)",
	   MAX(w.a)            OVER win AS "MAX (a)",
	   bool_and(w.b = 'O') OVER win AS "b=O"
FROM W AS w 
WINDOW win AS ();
```

|current row|a|SUM (a)|MAX (a)|b=O  |
|-----------|-|-------|-------|-----|
|q1         |1|     35|      7|false|
|q2         |2|     35|      7|false|
|q3         |3|     35|      7|false|
|q4         |3|     35|      7|false|
|q5         |3|     35|      7|false|
|q6         |4|     35|      7|false|
|q7         |6|     35|      7|false|
|q8         |6|     35|      7|false|
|q9         |7|     35|      7|false|

```sql
-- OVER (ORDER BY w.a ROWS UNBOUNDED PRECEDING AND CURRENT ROW)
SELECT w.row                     AS "current row",
	   w.a,
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS (ORDER BY w.a)
ORDER BY w.a, w.row;
```

|current row|a|frame size|rows in frame               |
|-----------|-|----------|----------------------------|
|q1         |1|         1|{q1}                        |
|q2         |2|         2|{q1,q2}                     |
|q3         |3|         5|{q1,q2,q3,q4,q5}            |
|q4         |3|         5|{q1,q2,q3,q4,q5}            |
|q5         |3|         5|{q1,q2,q3,q4,q5}            |
|q6         |4|         6|{q1,q2,q3,q4,q5,q6}         |
|q7         |6|         8|{q1,q2,q3,q4,q5,q6,q7,q8}   |
|q8         |6|         8|{q1,q2,q3,q4,q5,q6,q7,q8}   |
|q9         |7|         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|

```sql
SELECT w.row                     AS "current row",
	   w.a,
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS (ORDER BY w.a ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
ORDER BY w.a, w.row;
```

|current row|a|frame size|rows in frame               |
|-----------|-|----------|----------------------------|
|q1         |1|         1|{q1}                        |
|q2         |2|         2|{q1,q2}                     |
|q3         |3|         3|{q1,q2,q3}                  |
|q4         |3|         4|{q1,q2,q3,q4}               |
|q5         |3|         5|{q1,q2,q3,q4,q5}            |
|q6         |4|         6|{q1,q2,q3,q4,q5,q6}         |
|q7         |6|         7|{q1,q2,q3,q4,q5,q6,q7}      |
|q8         |6|         8|{q1,q2,q3,q4,q5,q6,q7,q8}   |
|q9         |7|         9|{q1,q2,q3,q4,q5,q6,q7,q8,q9}|

```sql
SELECT w.row                     AS "current row",
	   w.a,
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS (ORDER BY w.a ROWS BETWEEN 1 PRECEDING AND 2 FOLLOWING)
ORDER BY w.a;
```

|current row|a|frame size|rows in frame|
|-----------|-|----------|-------------|
|q1         |1|         3|{q1,q2,q3}   |
|q2         |2|         4|{q1,q2,q3,q4}|
|q3         |3|         4|{q2,q3,q4,q5}|
|q4         |3|         4|{q3,q4,q5,q6}|
|q5         |3|         4|{q4,q5,q6,q7}|
|q6         |4|         4|{q5,q6,q7,q8}|
|q7         |6|         4|{q6,q7,q8,q9}|
|q8         |6|         3|{q7,q8,q9}   |
|q9         |7|         2|{q8,q9}      |

```sql
SELECT w.row                             AS "current row",
	   w.a,
	   AVG(w.a) OVER win :: NUMERIC(4,2) AS "smoothed a"
FROM W AS w 
WINDOW win AS (ORDER BY w.a ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)
ORDER BY w.a;
```

|current row|a|smoothed a|
|-----------|-|----------|
|q1         |1|      1.50|
|q2         |2|      2.00|
|q3         |3|      2.67|
|q4         |3|      3.00|
|q5         |3|      3.33|
|q6         |4|      4.33|
|q7         |6|      5.33|
|q8         |6|      6.33|
|q9         |7|      6.50|

```sql
SELECT w.row                     AS "current row",
	   w.a,
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS (ORDER BY w.a RANGE BETWEEN CURRENT ROW AND 2 FOLLOWING)
ORDER BY w.a;
```

|current row|a|frame size|rows in frame   |
|-----------|-|----------|----------------|
|q1         |1|         5|{q1,q2,q3,q4,q5}|
|q2         |2|         5|{q2,q3,q4,q5,q6}|
|q3         |3|         4|{q3,q4,q5,q6}   |
|q4         |3|         4|{q3,q4,q5,q6}   |
|q5         |3|         4|{q3,q4,q5,q6}   |
|q6         |4|         3|{q6,q7,q8}      |
|q7         |6|         3|{q7,q8,q9}      |
|q8         |6|         3|{q7,q8,q9}      |
|q9         |7|         1|{q9}            |

```sql
SELECT w.row                     AS "current row",
	   w.a,
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM W AS w 
WINDOW win AS (ORDER BY w.a GROUPS BETWEEN CURRENT ROW AND 2 FOLLOWING)
ORDER BY w.a;
```

|current row|a|frame size|rows in frame      |
|-----------|-|----------|-------------------|
|q1         |1|         5|{q1,q2,q3,q4,q5}   |
|q2         |2|         5|{q2,q3,q4,q5,q6}   |
|q3         |3|         6|{q3,q4,q5,q6,q7,q8}|
|q4         |3|         6|{q3,q4,q5,q6,q7,q8}|
|q5         |3|         6|{q3,q4,q5,q6,q7,q8}|
|q6         |4|         4|{q6,q7,q8,q9}      |
|q7         |6|         3|{q7,q8,q9}         |
|q8         |6|         3|{q7,q8,q9}         |
|q9         |7|         1|{q9}               |
