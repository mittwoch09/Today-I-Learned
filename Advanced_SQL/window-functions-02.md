## Beyond Aggregations: Window Function

<img width="500" alt="Screen Shot 2022-06-17 at 11 51 22 AM" src="https://user-images.githubusercontent.com/73784742/174221420-d53705d0-2312-4af1-95a3-10a1a4d16030.png">

### LAG/LEAD

<img width="500" alt="Screen Shot 2022-06-17 at 11 52 55 AM" src="https://user-images.githubusercontent.com/73784742/174221556-7cebbc6c-0132-4d20-bca1-52cccbfd73ad.png">

<img width="500" alt="Screen Shot 2022-06-17 at 11 53 27 AM" src="https://user-images.githubusercontent.com/73784742/174221606-0d311586-c08b-4a35-8291-e68880988cc1.png">

### EXERCISE

```sql
TABLE W;
```

|row|a|b|
|---|-|-|
|q1 |1|O|
|q2 |2|X|
|q3 |3|X|
|q4 |3|O|
|q5 |3|X|
|q6 |4|X|
|q7 |6|O|
|q8 |6|O|
|q9 |7|X|

```sql
SELECT w.row 							 AS "current row",
w.a 								 AS a,
w.b 								 AS "partition",
LAG (w.row, 1, 'no row') OVER win AS "lag",
LEAD(w.row, 1, 'no row') OVER win AS "lead"
FROM   W AS w
WINDOW win AS (PARTITION BY w.b ORDER BY w.a)
ORDER BY w.b, w.a;
```

|current row|a|partition|lag   |lead  |
|-----------|-|---------|------|------|
|q1         |1|O        |no row|q4    |
|q4         |3|O        |q1    |q8    |
|q8         |6|O        |q4    |q7    |
|q7         |6|O        |q8    |no row|
|q2         |2|X        |no row|q5    |
|q5         |3|X        |q2    |q3    |
|q3         |3|X        |q5    |q6    |
|q6         |4|X        |q3    |q9    |
|q9         |7|X        |q6    |no row|

```sql
TABLE map;
```

|x  |alt|
|---|---|
|  0|200|
| 10|200|
| 20|200|
| 30|300|
| 40|400|
| 50|400|
| 60|400|
| 70|200|
| 80|400|
| 90|700|
|100|800|
|110|700|
|120|500|

```sql
SELECT m.x, m.alt,
	   CASE sign(LEAD(m.alt, 1) OVER (ORDER BY m.x) - m.alt)
	   		WHEN -1 THEN '\'
	   		WHEN  0 THEN '-'
	   		WHEN  1 THEN '/'
	   				ELSE '?'
	   		END AS  climb,
	   LEAD(m.alt, 1) OVER (ORDER BY m.x) - m.alt AS "by [m]"
FROM   map AS m;
```

|x  |alt|climb|by [m]|
|---|---|-----|------|
|  0|200|-    |     0|
| 10|200|-    |     0|
| 20|200|/    |   100|
| 30|300|/    |   100|
| 40|400|-    |     0|
| 50|400|-    |     0|
| 60|400|\    |  -200|
| 70|200|/    |   200|
| 80|400|/    |   300|
| 90|700|/    |   100|
|100|800|\    |  -100|
|110|700|\    |  -200|
|120|500|?    |      |

### EXERCISE

<img width="500" alt="Screen Shot 2022-06-29 at 4 10 02 PM" src="https://user-images.githubusercontent.com/73784742/176385873-eecf7168-df8c-4c84-a217-2d5ee37865c3.png">

<img width="500" alt="Screen Shot 2022-06-29 at 4 11 01 PM" src="https://user-images.githubusercontent.com/73784742/176386100-a2f01104-fe62-4e25-9420-5c0102dbddb8.png">

```sql
DROP TABLE IF EXISTS log;
CREATE TABLE log(uid text NOT NULL,
                 ts  timestamp NOT NULL);

INSERT INTO log(uid, ts) VALUES
  ('Cop', '05-25-2020 07:25:12'),
  ('Cop', '05-25-2020 07:25:18'),
  ('Cop', '05-25-2020 07:25:21'),
  ('Spy', '05-25-2020 08:01:55'),
  ('Spy', '05-25-2020 08:05:07'),
  ('Spy', '05-25-2020 08:05:30'),
  ('Spy', '05-25-2020 08:05:53'),
  ('Spy', '05-25-2020 08:06:19'),
  ('Cop', '05-25-2020 08:06:30'),
  ('Cop', '05-25-2020 08:06:42'),
  ('Cop', '05-25-2020 18:32:07'),
  ('Cop', '05-25-2020 18:32:27'),
  ('Cop', '05-25-2020 18:32:44'),
  ('Cop', '05-25-2020 18:33:00'),
  ('Spy', '05-25-2020 22:20:06'),
  ('Spy', '05-25-2020 22:20:16');
  
@set inactivity = '30 seconds'
```

```sql
WITH
tagged(uid, ts, sos) AS (
  SELECT l.*,
         CASE WHEN l.ts >
                   LAG (l.ts, 1, '-infinity') OVER (PARTITION BY l.uid ORDER BY l.ts) + :inactivity
              THEN 1
              ELSE 0
          END AS sos
  FROM   log AS l
  ORDER BY l.uid, l.ts
),
sessionized(uid, ts, sos, session) AS (
  SELECT t.*,
         SUM (t.sos) OVER (PARTITION BY t.uid ORDER BY t.ts) AS session
  FROM   tagged AS t
  ORDER BY t,uid, t.ts
),
measured(uid, session, duration) AS (
  SELECT s.uid,
         s.session,
         MAX(s.ts) - MIN(s.ts) AS duration
  FROM   sessionized AS s
  GROUP BY s.uid, s.session
  ORDER BY s.uid, s.session
)
```

```sql
TABLE tagged;
```

|uid|ts                     |sos|
|---|-----------------------|---|
|Cop|2020-05-25 07:25:12.000|  1|
|Cop|2020-05-25 07:25:18.000|  0|
|Cop|2020-05-25 07:25:21.000|  0|
|Cop|2020-05-25 08:06:30.000|  1|
|Cop|2020-05-25 08:06:42.000|  0|
|Cop|2020-05-25 18:32:07.000|  1|
|Cop|2020-05-25 18:32:27.000|  0|
|Cop|2020-05-25 18:32:44.000|  0|
|Cop|2020-05-25 18:33:00.000|  0|
|Spy|2020-05-25 08:01:55.000|  1|
|Spy|2020-05-25 08:05:07.000|  1|
|Spy|2020-05-25 08:05:30.000|  0|
|Spy|2020-05-25 08:05:53.000|  0|
|Spy|2020-05-25 08:06:19.000|  0|
|Spy|2020-05-25 22:20:06.000|  1|
|Spy|2020-05-25 22:20:16.000|  0|

```sql
TABLE sessionized;
```

|uid|ts                     |sos|session|
|---|-----------------------|---|-------|
|Cop|2020-05-25 07:25:12.000|  1|      1|
|Cop|2020-05-25 07:25:18.000|  0|      1|
|Cop|2020-05-25 07:25:21.000|  0|      1|
|Cop|2020-05-25 08:06:30.000|  1|      2|
|Cop|2020-05-25 08:06:42.000|  0|      2|
|Cop|2020-05-25 18:32:07.000|  1|      3|
|Cop|2020-05-25 18:32:27.000|  0|      3|
|Cop|2020-05-25 18:32:44.000|  0|      3|
|Cop|2020-05-25 18:33:00.000|  0|      3|
|Spy|2020-05-25 08:01:55.000|  1|      1|
|Spy|2020-05-25 08:05:07.000|  1|      2|
|Spy|2020-05-25 08:05:30.000|  0|      2|
|Spy|2020-05-25 08:05:53.000|  0|      2|
|Spy|2020-05-25 08:06:19.000|  0|      2|
|Spy|2020-05-25 22:20:06.000|  1|      3|
|Spy|2020-05-25 22:20:16.000|  0|      3|

```sql
TABLE measured;
```

|uid|session|duration|
|---|-------|--------|
|Cop|      1|00:00:09|
|Cop|      2|00:00:12|
|Cop|      3|00:00:53|
|Spy|      1|00:00:00|
|Spy|      2|00:01:12|
|Spy|      3|00:00:10|

### FIRST_VALUE, LAST_VALUE, NTH_VALUE

<img width="500" alt="Screen Shot 2022-06-30 at 1 45 21 PM" src="https://user-images.githubusercontent.com/73784742/176601713-2444894b-366b-4316-a153-835f37ae7b7a.png">

<img width="500" alt="Screen Shot 2022-06-30 at 1 45 53 PM" src="https://user-images.githubusercontent.com/73784742/176601794-fef48ec7-5f0d-4803-b510-edde2a0f2c0b.png">

```sql
SELECT w.row                       AS "current row",
       array_agg(w.row)   OVER win AS "rows in frame",
       FIRST_VALUE(w.row) OVER win AS "first row",
       LAST_VALUE(w.row)  OVER win AS "last row",
       NTH_VALUE(w.row,2) OVER win AS "second row"
FROM   W AS w
WINDOW win AS (ORDER BY w.a ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING)
ORDER BY w.a, w.row;
```

|current row|rows in frame   |first row|last row|second row|
|-----------|----------------|---------|--------|----------|
|q1         |{q1,q2,q3}      |q1       |q3      |q2        |
|q2         |{q1,q2,q3,q4}   |q1       |q4      |q2        |
|q3         |{q1,q2,q3,q4,q5}|q1       |q5      |q2        |
|q4         |{q2,q3,q4,q5,q6}|q2       |q6      |q3        |
|q5         |{q3,q4,q5,q6,q7}|q3       |q7      |q4        |
|q6         |{q4,q5,q6,q7,q8}|q4       |q8      |q5        |
|q7         |{q5,q6,q7,q8,q9}|q5       |q9      |q6        |
|q8         |{q6,q7,q8,q9}   |q6       |q9      |q7        |
|q9         |{q7,q8,q9}      |q7       |q9      |q8        |

### EXERCISE

<img width="500" alt="Screen Shot 2022-06-30 at 2 11 58 PM" src="https://user-images.githubusercontent.com/73784742/176605366-2dfd6c20-f047-4bf8-b85a-607357647f4d.png">

<img width="500" alt="Screen Shot 2022-06-30 at 2 12 11 PM" src="https://user-images.githubusercontent.com/73784742/176605411-4f33fd05-fc03-4b2f-90ed-26c3c6665e5e.png">

```sql
DROP FUNCTION IF EXISTS slope(int,int,int,int);
CREATE FUNCTION slope(s1 int, s2 int, s3 int, s4 int) RETURNS text AS
$$
  SELECT string_agg((array['↗','→','↘'])[signs.s + 2], '' ORDER by signs.pos)
  FROM   unnest(array[s1,s2,s3,s4]) WITH ORDINALITY AS signs(s,pos)
$$
LANGUAGE SQL IMMUTABLE;
```

```sql
WITH
slopes(x, slope) AS (
  SELECT m.x,
         slope(sign(FIRST_VALUE(m.alt) OVER w - NTH_VALUE(m.alt,2) OVER w) :: int,
               sign(NTH_VALUE(m.alt,2) OVER w - m.alt)                     :: int,
               sign(m.alt                     - NTH_VALUE(m.alt,4) OVER w) :: int,
               sign(NTH_VALUE(m.alt,4) OVER w - LAST_VALUE(m.alt) OVER w)  :: int)
  FROM   map AS m                                                      --   ↑
  WINDOW w AS (ORDER BY m.x ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING)  -- some sign() calls may yield NULL
)
SELECT s.x,
       CASE WHEN s.slope SIMILAR TO '(↘↘|↘→|↗↘|→↘)(↗↗|↗→|↗↘|→↗)' THEN 'valley'
            WHEN s.slope SIMILAR TO '(↗↗|↗→|↘↗|→↗)(↘↘|↘→|↘↗|→↘)' THEN 'peak'
            ELSE '-'
       END AS feature
FROM   slopes AS s
ORDER BY s.x;
```

|x  |feature|
|---|-------|
|  0|-      |
| 10|-      |
| 20|-      |
| 30|-      |
| 40|-      |
| 50|peak   |
| 60|-      |
| 70|valley |
| 80|-      |
| 90|-      |
|100|peak   |
|110|-      |
|120|-      |

### Numbering and Ranking Rows

<img width="500" alt="Screen Shot 2022-06-30 at 2 41 08 PM" src="https://user-images.githubusercontent.com/73784742/176609791-ac039cd2-dd09-4976-a683-d65bf5de8949.png">

<img width="500" alt="Screen Shot 2022-06-30 at 2 41 51 PM" src="https://user-images.githubusercontent.com/73784742/176609938-b7383ff8-c173-43a5-91bc-7931f8526ca8.png">

```sql
SELECT w."row"               AS "current row",
       w.a,
       w.b,
       ROW_NUMBER() OVER win AS "ROW_NUMBER",
       DENSE_RANK() OVER win AS "DENSE_RANK",
       RANK()       OVER win AS "RANK"
FROM   W AS w
WINDOW win AS (PARTITION BY w.b ORDER BY w.a)
ORDER BY w.b, w.a;
```

|current row|a|b|ROW_NUMBER|DENSE_RANK|RANK|
|-----------|-|-|----------|----------|----|
|q1         |1|O|         1|         1|   1|
|q4         |3|O|         2|         2|   2|
|q8         |6|O|         3|         3|   3|
|q7         |6|O|         4|         3|   3|
|q2         |2|X|         1|         1|   1|
|q5         |3|X|         2|         2|   2|
|q3         |3|X|         3|         2|   2|
|q6         |4|X|         4|         3|   4|
|q9         |7|X|         5|         4|   5|

```sql
SELECT tallest.legs, tallest.species, tallest.height
FROM   (SELECT d.legs, d.species, d.height,
               RANK() OVER (PARTITION BY d.legs
                            ORDER BY d.height DESC) AS rank
        FROM   dinosaurs AS d
        WHERE  d.legs IS NOT NULL) AS tallest(legs,species,height,rank)
WHERE  tallest.rank <= 3
ORDER BY tallest.legs, tallest.height;
```

|legs|species      |height|
|----|-------------|------|
|   2|Spinosaurus  |   2.4|
|   2|Ceratosaurus |   4.0|
|   2|Tyrannosaurus|   7.0|
|   4|Diplodocus   |   3.6|
|   4|Brachiosaurus|   7.6|
|   4|Supersaurus  |  10.0|

### EXERCISE

<img width="500" alt="Screen Shot 2022-06-30 at 2 47 44 PM" src="https://user-images.githubusercontent.com/73784742/176610934-781ff2cd-f86d-4ed8-af2a-c1da5cf060e9.png">

```sql
DROP TABLE IF EXISTS citations;
CREATE TABLE citations(ref int PRIMARY KEY);

INSERT INTO citations VALUES
  (5), (2), (14), (3), (1), (42), (6), (10), (7), (13);

TABLE citations;
```

|ref|
|---|
|  5|
|  2|
| 14|
|  3|
|  1|
| 42|
|  6|
| 10|
|  7|
| 13|

```sql
WITH ranges(ref, range) AS (
  SELECT c.ref,
         c.ref - ROW_NUMBER() OVER (ORDER BY c.ref) AS range
    FROM citations AS c
),
output(range, first, last) AS (
  SELECT r.range, MIN(r.ref) AS first, MAX(r.ref) AS last
  FROM   ranges AS r
  GROUP BY r.range
)
SELECT string_agg(CASE o.last - o.first
                    WHEN 0 THEN o.first :: text
                    WHEN 1 THEN o.first || '&' || o.last
                    ELSE        o.first || '-' || o.last
                  END,
                  ','
                  ORDER BY o.range) AS citations
FROM   output AS o;
```

|citations          |
|-------------------|
|1-3,5-7,10,13&14,42|

<img width="500" alt="Screen Shot 2022-07-01 at 2 16 44 PM" src="https://user-images.githubusercontent.com/73784742/176835548-c992a70d-053e-43c9-9ab2-48381e4cf138.png">

```sql
SELECT w.row                   AS "current row",
       w.a,
       PERCENT_RANK() OVER win AS "PERCENT_RANK",
       CUME_DIST()    OVER win AS "CUME_DIST",
       NTILE(3)       OVER win AS "NTILE(3)"
FROM   W AS w
WINDOW win AS (ORDER BY w.a)
ORDER BY w.a;
```

|current row|a|PERCENT_RANK|CUME_DIST         |NTILE(3)|
|-----------|-|------------|------------------|--------|
|q1         |1|         0.0|0.1111111111111111|       1|
|q2         |2|       0.125|0.2222222222222222|       1|
|q3         |3|        0.25|0.5555555555555556|       1|
|q4         |3|        0.25|0.5555555555555556|       2|
|q5         |3|        0.25|0.5555555555555556|       2|
|q6         |4|       0.625|0.6666666666666666|       2|
|q7         |6|        0.75|0.8888888888888888|       3|
|q8         |6|        0.75|0.8888888888888888|       3|
|q9         |7|         1.0|               1.0|       3|

### EXERCISE

<img width="500" alt="Screen Shot 2022-07-01 at 2 21 44 PM" src="https://user-images.githubusercontent.com/73784742/176836252-1e53bbea-72d6-4b8f-805d-ca6595d4d76f.png">

```sql
DROP TABLE IF EXISTS experiment;
CREATE TABLE experiment (
  t int PRIMARY KEY,
  f float
);

@set N = 100

@set segments = 5

INSERT INTO experiment(t, f)
  SELECT t, random() * 40 AS f
  FROM   generate_series(0, :N-1) AS t;

TABLE experiment;
```

```sql
WITH
tiles(tile, t, f) AS (
  SELECT NTILE(:segments) OVER (ORDER BY e.t) AS tile, e.t, e.f
  FROM   experiment AS e
),
segments(t0, t1, f0, f1) AS (
  SELECT DISTINCT ON (t.tile) -- to prevent showing all partitions
         FIRST_VALUE(t.t) OVER segment AS t0, LAST_VALUE(t.t) OVER segment AS t1,
         FIRST_VALUE(t.f) OVER segment AS f0, LAST_VALUE(t.f) OVER segment AS f1
  FROM   tiles AS t
  WINDOW segment AS (PARTITION BY t.tile ORDER BY t.t ROWS BETWEEN UNBOUNDED PRECEDING
                                                               AND UNBOUNDED FOLLOWING)
)
SELECT s.t0, s.t1, (s.f1 - s.f0) / (s.t1 - s.t0) AS m, s.f0 AS b
FROM   segments AS s;
```

|t0|t1|m                   |b                 |
|--|--|--------------------|------------------|
| 0|19| -1.1098085975338428| 30.94152859670828|
|20|39|-0.23897775720213513|22.656110408113364|
|40|59|-0.25847593035979494|16.634270128197954|
|60|79|-0.49428511221407984| 23.22585433891959|
|80|99| -1.4040733736118152|36.544866584515034|

### SUMMARY

<img width="500" alt="Screen Shot 2022-07-01 at 2 31 55 PM" src="https://user-images.githubusercontent.com/73784742/176837573-ffbc1a51-1f63-4b7f-8af1-1c10a6b5242a.png">
