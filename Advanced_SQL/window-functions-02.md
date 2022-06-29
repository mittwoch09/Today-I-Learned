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
