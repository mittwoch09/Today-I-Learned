## Table-Generating Functions

<img align="left" width="500" alt="Screen Shot 2022-06-01 at 1 56 55 PM" src="https://user-images.githubusercontent.com/73784742/171337423-985c9a2f-421a-48a3-8d53-10a48a838a9d.png">

```sql
-- 1, 3, 5, 7, 9 (since 1 <= i <= 10)
SELECT i
FROM generate_series(1,10,2) AS i;
```

|i|
|-|
|1|
|3|
|5|
|7|
|9|

```sql
-- 0, -0.1, ..., -1.0
SELECT i
FROM generate_series(0,-2,0.1) AS i;
```

|i   |
|----|
|   0|
|-0.1|
|-0.2|
|-0.3|
|-0.4|
|-0.5|
|-0.6|
|-0.7|
|-0.8|
|-0.9|
|-1.0|

```sql
-- generate subscripts for word/character access

SELECT i, xs[i]
FROM (VALUES (string_to_array('Star Wars', ' '))) AS _(xs),
    generate_subscripts(xs, 1) AS i;
```

|i|xs  |
|-|----|
|1|Star|
|2|Wars|

## A Bridge Between Arrays and Tables: unnest & array_agg

<img align="left" width="500" alt="Screen Shot 2022-06-01 at 1 56 10 PM" src="https://user-images.githubusercontent.com/73784742/171337336-886e57fb-7883-4469-9d85-b1cab95f96d4.png">

```sql
SELECT starwars.*
FROM   unnest(ARRAY[4,5,1,2,3,6,7,8,9],              -- episodes
			  ARRAY['A New Hope',                    -- know episode titles
			  		'The Empire Strikes Back',
			  		'The Phantom Menace',
			  		'Attack of the Clones',
			  		'Revenge of the Sith',
			  		'Return of the Jedi',
			  		'The Force Awakens',
			  		'The Last Jedi',
			  		'The Rise of Skywalker'])
	   WITH ORDINALITY AS starwars(episode,film,watch)
 ORDER BY watch;
 ```

|episode|film                   |watch|
|-------|-----------------------|-----|
|      4|A New Hope             |    1|
|      5|The Empire Strikes Back|    2|
|      1|The Phantom Menace     |    3|
|      2|Attack of the Clones   |    4|
|      3|Revenge of the Sith    |    5|
|      6|Return of the Jedi     |    6|
|      7|The Force Awakens      |    7|
|      8|The Last Jedi          |    8|
|      9|The Rise of Skywalker  |    9|

## User-Defined SQL Functions (UDFs)

<img align="left" width="500" alt="Screen Shot 2022-06-01 at 2 25 03 PM" src="https://user-images.githubusercontent.com/73784742/171340982-c614b36b-1dd3-49d8-a7c0-159fe714ab04.png">

<img align="left" width="500" alt="Screen Shot 2022-06-01 at 2 26 50 PM" src="https://user-images.githubusercontent.com/73784742/171341255-559d5e4b-4de4-420e-aa90-5f4adcf1f284.png">

### Example UDF: Issue Unique ID, Write Protocol

```sql
DROP TABLE IF EXISTS issue;
CREATE TABLE issue (
	id     int GENERATED ALWAYS AS IDENTITY,
	"when" timestamp);

DROP FUNCTION IF EXISTS new_ID(text);
CREATE FUNCTION new_ID(prefix text) RETURNS TEXT AS
$$
	INSERT INTO issue(id, "when") VALUES
	  (DEFAULT, 'now'::timestamp)
	RETURNING prefix || id::TEXT
$$
LANGUAGE SQL VOLATILE;
```

```sql
SELECT new_ID('csutomer') AS customer, d.species
FROM   dinosaurs AS d
WHERE  d.legs = 2;
```

|customer |species      |
|---------|-------------|
|csutomer1|Ceratosaurus |
|csutomer2|Deinonychus  |
|csutomer3|Microvenator |
|csutomer4|Plateosaurus |
|csutomer5|Spinosaurus  |
|csutomer6|Tyrannosaurus|
|csutomer7|Velociraptor |

```sql
TABLE issue;
```

|id|when                   |
|--|-----------------------|
| 1|2022-06-01 14:21:19.438|
| 2|2022-06-01 14:21:19.438|
| 3|2022-06-01 14:21:19.438|
| 4|2022-06-01 14:21:19.438|
| 5|2022-06-01 14:21:19.438|
| 6|2022-06-01 14:21:19.438|
| 7|2022-06-01 14:21:19.438|

### Example Table-Generating UDF: Flatten a 2D-Array

```sql
CREATE OR REPLACE FUNCTION unnest2(xss anyarray)
  RETURNS SETOF anyelement AS
$$
SELECT xss[i][j]
FROM   generate_subscripts(xss,1) _(i),
	   generate_subscripts(xss,2) __(j)
ORDER BY j, i
$$
LANGUAGE SQL IMMUTABLE;
```

```sql
SELECT t.*
FROM   unnest2(ARRAY[ARRAY['a','b','c'],
					 ARRAY['d','d','f'],
					 ARRAY['x','y','z']])
		WITH ORDINALITY AS t(elem,pos);
```

|elem|pos|
|----|---|
|a   |  1|
|d   |  2|
|x   |  3|
|b   |  4|
|d   |  5|
|y   |  6|
|c   |  7|
|f   |  8|
|z   |  9|
