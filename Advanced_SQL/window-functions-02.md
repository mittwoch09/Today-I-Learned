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
