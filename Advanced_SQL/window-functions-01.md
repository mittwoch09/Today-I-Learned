## Window Functions

<img width="500" alt="Screen Shot 2022-06-06 at 2 20 29 PM" src="https://user-images.githubusercontent.com/73784742/172106363-d28af86f-72f6-4db8-90c9-917192edb159.png">

### Window Frame Specifications (Variant: ROWS)

<img width="500" alt="Screen Shot 2022-06-06 at 2 33 21 PM" src="https://user-images.githubusercontent.com/73784742/172107984-d6442643-bee0-4908-a5d0-3d58cbf9f6a7.png">

### Window Frame Specifications (Variant: RANGE)

<img width="500" alt="Screen Shot 2022-06-06 at 2 45 14 PM" src="https://user-images.githubusercontent.com/73784742/172109702-5fd5c760-8999-4251-9f2d-83d6379ca7ce.png">

`distance measured in terms of |current row - i|`

### Window Frame Specifications (Variant: GROUPS)

<img width="500" alt="Screen Shot 2022-06-06 at 2 51 18 PM" src="https://user-images.githubusercontent.com/73784742/172110451-77ae4c5e-791e-4168-b486-e8fdaca4e3ca.png">

`distance measured in number of peer groups`

### Window Frame Specifications: Abbreviations

<img width="500" alt="Screen Shot 2022-06-06 at 3 57 34 PM" src="https://user-images.githubusercontent.com/73784742/172119893-5e159f18-7116-4f84-90f9-c3b889a28e30.png">

`OVER () = all rows form the window frame(for any current row)`

`OVER (ORDER BY A) = OVER (ORDER BY A RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)`

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

### EXCERCISE

```sql
DROP TABLE IF EXISTS sensors;
CREATE TABLE sensors (
	day     int PRIMARY KEY,
	weekday TEXT,
	temp    float,
	rain    float);
	
INSERT INTO sensors(day, weekday, temp, rain) VALUES
	(1,  'Thu', 13,   0),
	(2,  'Fri', 10, 800),
	(3,  'Sat', 12, 300),
	(4,  'Sun', 16, 100),
	(5,  'Mon', 20, 400),
	(6,  'Tue', 20,  80),
	(7,  'Wed', 18, 500),
	(8,  'Thu', 14,   0),
	(9,  'Fri', 10,   0),
	(10, 'Sat', 12, 500),
	(11, 'Sun', 14, 300),
	(12, 'Mon', 14, 800),
	(13, 'Tue', 16,   0),
	(14, 'Wed', 15,   0),
	(15, 'Thu', 18, 100),
	(16, 'Fri', 17, 100),
	(17, 'Sat', 15,   0),
	(18, 'Sun', 16, 300),
	(19, 'Mon', 16, 400),
	(20, 'Tue', 19, 200),
	(21, 'Wed', 19, 100),
	(22, 'Thu', 18,   0),
	(23, 'Fri', 17,   0),
	(24, 'Sat', 16, 200);
```

```sql
WITH
-- Collect weather data for each day (and two days prior)
three_day_sensors(day, weekday, temp, rain) AS (
	SELECT s.day, s.weekday,
		   MIN(s.temp) OVER three_days AS temp,
		   SUM(s.rain) OVER three_days AS rain
	FROM   sensors AS s
	WINDOW three_days AS (ORDER BY s.DAY ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
),
-- Derive sunny/gloomy conditions FROM aggregated sensor readings
weather(day, weekday, condition) AS (
	SELECT s.day, s.weekday,
		   CASE WHEN s.temp >= 15 AND s.rain <= 600
		   		THEN 'sunny'
		   		ELSE 'gloomy'
		   END AS condition
	FROM   three_day_sensors AS s
)
-- Calculate chance of fine weather on a weekday/weekend
SELECT w.weekday IN ('Sat', 'Sun') AS "weekend?",
	   (COUNT(*) FILTER (WHERE w.condition = 'sunny') * 100.0 /
	    COUNT(*)) :: int AS "% fine"
FROM    weather AS w
GROUP BY "weekend?";
```

|weekend?|% fine|
|--------|------|
|false   |    29|
|true    |    43|

### PARTITION BY: Window Frames Inside Partitions

<img width="500" alt="Screen Shot 2022-06-08 at 2 54 31 PM" src="https://user-images.githubusercontent.com/73784742/172551531-472d8788-fad8-4198-acbc-9c691e5bb109.png">

```sql
SELECT w.row                     AS "current row",
	   w.a,
	   w.b                       AS "partition",
	   COUNT(*)         OVER win AS "frame size",
	   array_agg(w.row) OVER win AS "rows in frame"
FROM   W AS w 
WINDOW win AS (PARTITION BY w.b ORDER BY w.a ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
ORDER BY w.b, w.a, w.row;
```

|current row|a|partition|frame size|rows in frame   |
|-----------|-|---------|----------|----------------|
|q1         |1|O        |         1|{q1}            |
|q4         |3|O        |         2|{q1,q4}         |
|q7         |6|O        |         4|{q1,q4,q8,q7}   |
|q8         |6|O        |         3|{q1,q4,q8}      |
|q2         |2|X        |         1|{q2}            |
|q3         |3|X        |         3|{q2,q5,q3}      |
|q5         |3|X        |         2|{q2,q5}         |
|q6         |4|X        |         4|{q2,q5,q3,q6}   |
|q9         |7|X        |         5|{q2,q5,q3,q6,q9}|

```sql
SELECT w.row             AS "current row",
	   w.a,
	   w.b               AS "partition",
	   SUM(w.a) OVER win AS "SUM(a) so far"
FROM W AS w 
WINDOW win AS (PARTITION BY w.b ORDER BY w.a ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
ORDER BY w.b;
```

|current row|a|partition|SUM(a) so far|
|-----------|-|---------|-------------|
|q1         |1|O        |            1|
|q4         |3|O        |            4|
|q7         |6|O        |           10|
|q8         |6|O        |           16|
|q2         |2|X        |            2|
|q3         |3|X        |            5|
|q5         |3|X        |            8|
|q6         |4|X        |           12|
|q9         |7|X        |           19|

### EXERCISE

<img width="500" alt="Screen Shot 2022-06-13 at 4 35 18 PM" src="https://user-images.githubusercontent.com/73784742/173313545-0a934c09-b2b5-4365-b8ce-bb6ed58f5b13.png">

```sql
DROP TABLE IF EXISTS map;
CREATE TABLE map (
	x   int NOT NULL PRIMARY KEY,
	alt int NOT NULL
);

@set p0 = 0

INSERT INTO map(x, alt) VALUES
	(  0, 200),
	( 10, 200),
	( 20, 200),
	( 30, 300),
	( 40, 400),
	( 50, 400),
	( 60, 400),
	( 70, 200),
	( 80, 400),
	( 90, 700),
	(100, 800),
	(110, 700),
	(120, 500);
```

```sql
WITH
-- Location (x and altitude) of observer p0
p0(x, alt) AS (
	SELECT :p0 AS x, m.alt
	FROM   MAP AS m
	WHERE  m.x = :p0
),
-- Angles from view point of p0 (facing right)
angles(x, angle) AS (
	SELECT m.x,
		   degrees(atan((m.alt - p0.alt) / abs(p0.x - m.x))) AS angle
    FROM   map AS m, p0
    WHERE  m.x > p0.x
),
-- Max angle scan from p0
max_scan(x, max_angle) AS (
	SELECT a.x,
		   MAX(a.angle) OVER (ORDER BY abs(p0.x - a.x)) AS max_angle
	FROM   angles AS a, p0
),
-- Visibility from p0
visibility(x, "visible?") AS (
	SELECT m.x, a.angle >= m.max_angle AS "visible?"
	FROM   angles AS a, max_scan AS m
	WHERE  a.x = m.x
)
TABLE visibility
ORDER BY x;
```

|x  |visible?|
|---|--------|
| 10|true    |
| 20|true    |
| 30|true    |
| 40|true    |
| 50|false   |
| 60|false   |
| 70|false   |
| 80|false   |
| 90|true    |
|100|true    |
|110|false   |
|120|false   |

<img width="500" alt="Screen Shot 2022-06-13 at 4 37 44 PM" src="https://user-images.githubusercontent.com/73784742/173313999-2dbe1be8-1493-4b81-a58a-a44eb5a00aca.png">

```sql
@set p0 = 90

WITH
-- Location (x and altitude) of observer p0
p0(x, alt) AS (
	SELECT :p0 AS x, m.alt
	FROM   MAP AS m
	WHERE  m.x = :p0
),
-- Angles from view point of p0 (facing left and right)
angles(x, angle) AS (
	SELECT m.x,
		   degrees(atan((m.alt - p0.alt) / abs(p0.x - m.x))) AS angle
    FROM   map AS m, p0
    WHERE  m.x <> p0.x
),
-- Max angle scan from p0
max_scan(x, max_angle) AS (
	SELECT a.x,
		   MAX(a.angle) OVER (PARTITION BY sign(p0.x - a.x) ORDER BY abs(p0.x - a.x)) AS max_angle
	FROM   angles AS a, p0
),
-- Visibility from p0
visibility(x, "visible?") AS (
	SELECT m.x, a.angle >= m.max_angle AS "visible?"
	FROM   angles AS a, max_scan AS m
	WHERE  a.x = m.x
)
TABLE visibility
ORDER BY x;
```

|x  |visible?|
|---|--------|
|  0|true    |
| 10|true    |
| 20|false   |
| 30|true    |
| 40|true    |
| 50|true    |
| 60|true    |
| 70|true    |
| 80|true    |
|100|true    |
|110|false   |
|120|false   |
