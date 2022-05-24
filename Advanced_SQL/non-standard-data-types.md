## Enumerations

```sql
DROP TYPE IF EXISTS episode CASCADE;
CREATE TYPE episode AS ENUM
  ('ANH', 'ESB', 'TPM', 'AOTC', 'ROTS', 'ROTJ', 'TFA', 'TLJ', 'TROS');

DROP TABLE IF EXISTS starwars;
CREATE TABLE starwars(film    episode PRIMARY KEY,
                      title   text,
                      release date);

INSERT INTO starwars(film,title,release) VALUES
    ('TPM',  'The Phantom Menace',      'May 19, 1999'),
    ('AOTC', 'Attack of the Clones',    'May 16, 2002'),
    ('ROTS', 'Revenge of the Sith',     'May 19, 2005'),
    ('ANH',  'A New Hope',              'May 25, 1977'),
    ('ESB',  'The Empire Strikes Back', 'May 21, 1980'),
    ('ROTJ', 'Return of the Jedi',      'May 25, 1983'),
    ('TFA',  'The Force Awakens',       'Dec 18, 2015'),
    ('TLJ',  'The Last Jedi',           'Dec 15, 2017'),
    ('TROS', 'The Rise of Skywalker',   'Dec 19, 2019');
```

```sql
INSERT INTO starwars(film,title,release) VALUES
  ('R1', 'Rogue One', 'Dec 15, 2016');
--   ↑
-- ⚠️ not an episode value
```

```sql
SELECT s.*
FROM   starwars AS s
ORDER BY s.film; 
```

|film|title                  |release   |
|----|-----------------------|----------|
|ANH |A New Hope             |1977-05-25|
|ESB |The Empire Strikes Back|1980-05-21|
|TPM |The Phantom Menace     |1999-05-19|
|AOTC|Attack of the Clones   |2002-05-16|
|ROTS|Revenge of the Sith    |2005-05-19|
|ROTJ|Return of the Jedi     |1983-05-25|
|TFA |The Force Awakens      |2015-12-18|
|TLJ |The Last Jedi          |2017-12-15|
|TROS|The Rise of Skywalker  |2019-12-19|


```sql
SELECT s.*
FROM   starwars AS s
ORDER BY s.release;
```

|film|title                  |release   |
|----|-----------------------|----------|
|ANH |A New Hope             |1977-05-25|
|ESB |The Empire Strikes Back|1980-05-21|
|ROTJ|Return of the Jedi     |1983-05-25|
|TPM |The Phantom Menace     |1999-05-19|
|AOTC|Attack of the Clones   |2002-05-16|
|ROTS|Revenge of the Sith    |2005-05-19|
|TFA |The Force Awakens      |2015-12-18|
|TLJ |The Last Jedi          |2017-12-15|
|TROS|The Rise of Skywalker  |2019-12-19|

## Binary Arrays (BLOBs)

<img width="500" alt="Screen Shot 2022-05-22 at 6 54 16 PM" src="https://user-images.githubusercontent.com/73784742/169691734-2737fbfe-5ad6-4ef6-abb8-36fe904ede67.png">

## Ranges (Intervals)

<img width="500" alt="Screen Shot 2022-05-22 at 6 55 06 PM" src="https://user-images.githubusercontent.com/73784742/169691752-4eac9ba1-1ae9-4323-be44-588f86ddb4fb.png">

<img width="500" alt="Screen Shot 2022-05-22 at 6 55 38 PM" src="https://user-images.githubusercontent.com/73784742/169691770-93b54508-cd0b-4618-956d-0bf7f11a90ab.png">

```sql
SELECT '(1,10]' :: int4range;
```

|int4range|
|---------|
|   [2,11)|

```sql
SELECT int4range(1,5,'[]') * '[5,10)' :: int4range;
```

|?column?|
|--------|
|   [5,6)|

## Geometric Objects

<img width="500" alt="Screen Shot 2022-05-23 at 2 18 56 PM" src="https://user-images.githubusercontent.com/73784742/169755659-f24a7db4-3b94-4428-bdfb-bde42e6649e9.png">

## JSON (JavaScript Object Notation)

<img width="500" alt="Screen Shot 2022-05-23 at 2 19 53 PM" src="https://user-images.githubusercontent.com/73784742/169755800-fe503c1c-b00b-4046-aa6c-46bf5703b68b.png">

```sql
-- jsonb
VALUES (1, '{ "b":1, "a":2 }'       ::jsonb),  -- ← pair order may flip
       (2, '{ "a":1, "b":2, "a":3 }'       ),  -- ← duplicate field
       (3, '[ 0,   false,null ]'           );  -- ← whitespace normalized
```

|column1|column2         |
|-------|----------------|
|      1|{"a": 2, "b": 1}|
|      2|{"a": 3, "b": 2}|
|      3|[0, false, null]|

```sql
-- json
VALUES (1, '{ "b":1, "a":2 }'       ::json ),  -- ← pair order and ...
       (2, '{ "a":1, "b":2, "a":3 }'       ),  -- ← ... duplicates preserved
       (3, '[ 0,   false,null ]'           );  -- ← whitespace preserved
```

|column1|column2                |
|-------|-----------------------|
|      1|{ "b":1, "a":2 }       |
|      2|{ "a":1, "b":2, "a":3 }|
|      3|[ 0,   false,null ]    |

```sql
--  Navigating a JSON value using subscripting
SELECT ('{ "a":0, "b": { "b₁":[1,2], "b₂":3 } }' :: jsonb)['b']['b₁'][1] :: int + 40;
--                                                                       ↑
--                                     extracts a jsonb value, cast for computation
```

|?column?|
|--------|
|      42|

### Bridging between JSON and SQL

<img width="500" alt="Screen Shot 2022-05-23 at 2 30 47 PM" src="https://user-images.githubusercontent.com/73784742/169757415-09baecfa-8515-45b6-aa94-195b156e7ef5.png">

```sql
INSERT INTO T VALUES
  (1, 'x',  true, 10),
  (2, 'y',  true, 40),
  (3, 'x', false, 30),
  (4, 'y', false, 20),
  (5, 'x',  true, NULL);
  
TABLE T;
```

|a|b|c    |d |
|-|-|-----|--|
|1|x|true |10|
|2|y|true |40|
|3|x|false|30|
|4|y|false|20|
|5|x|true |  |

#### Goal: Convert table into JSON (jsonb) array of objects

```sql
-- Step ➊: convert each row into a JSON object (columns ≡ fields)
SELECT row_to_json(t)::jsonb
FROM   T AS t;
```

|row_to_json                             |
|----------------------------------------|
|{"a": 1, "b": "x", "c": true, "d": 10}  |
|{"a": 2, "b": "y", "c": true, "d": 40}  |
|{"a": 3, "b": "x", "c": false, "d": 30} |
|{"a": 4, "b": "y", "c": false, "d": 20} |
|{"a": 5, "b": "x", "c": true, "d": null}|

```sql
-- Step ➋: aggregate the table of JSON objects into one JSON array
--          (here: in some element order)
--
--  may understood as a unity for now (array_agg() in focus soon)
--     ╭──────────┴───────────╮
SELECT array_to_json(array_agg(row_to_json(t)))::jsonb
FROM   T as t;
```

|array_to_json                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|[{"a": 1, "b": "x", "c": true, "d": 10}, {"a": 2, "b": "y", "c": true, "d": 40}, {"a": 3, "b": "x", "c": false, "d": 30}, {"a": 4, "b": "y", "c": false, "d": 20}, {"a": 5, "b": "x", "c": true, "d": null}]|

```sql
-- Pretty-print JSON output
--
SELECT jsonb_pretty(array_to_json(array_agg(row_to_json(t)))::jsonb)
FROM   T as t;
```

```sql
-- Table T represented as a JSON object (JSON value in single row/single column)
DROP TABLE IF EXISTS like_T_but_as_JSON;
CREATE TEMPORARY TABLE like_T_but_as_JSON(a) AS
  SELECT array_to_json(array_agg(row_to_json(t)))::jsonb
  FROM   T as t;

TABLE like_T_but_as_JSON;
```

#### Goal: Convert JSON object (array of regular objects) into a table

```sql
-- Step ➊: convert JSON array into table of JSON objects
--
SELECT objs.o
FROM   jsonb_array_elements((TABLE like_T_but_as_JSON)) AS objs(o);
```

|o                                       |
|----------------------------------------|
|{"a": 1, "b": "x", "c": true, "d": 10}  |
|{"a": 2, "b": "y", "c": true, "d": 40}  |
|{"a": 3, "b": "x", "c": false, "d": 30} |
|{"a": 4, "b": "y", "c": false, "d": 20} |
|{"a": 5, "b": "x", "c": true, "d": null}|

```sql
-- NB: Steps ➋a and ➋b/c lead to alternative tabular representation:

-- Step ➋a: turn JSON objects into key/value pairs (⚠️ column value::jsonb)
--
SELECT t.*
FROM   jsonb_array_elements((TABLE like_T_but_as_JSON)) AS objs(o),
       jsonb_each(objs.o) AS t;
```

|key|value|
|---|-----|
|a  |1    |
|b  |"x"  |
|c  |true |
|d  |10   |
|a  |2    |
|b  |"y"  |
|c  |true |
|d  |40   |
|a  |3    |
|b  |"x"  |
|c  |false|
|d  |30   |
|a  |4    |
|b  |"y"  |
|c  |false|
|d  |20   |
|a  |5    |
|b  |"x"  |
|c  |true |
|d  |null |

```sql
-- Step ➋b: turn JSON objects into rows (fields ≡ columns)
--
SELECT t.*
FROM   jsonb_array_elements((TABLE like_T_but_as_JSON)) AS objs(o),
       jsonb_to_record(objs.o) AS t(a int, b text, c boolean, d int);
--                                ╰────────────────┬───────────────╯
--                   explicitly provide column name and type information
--                      (⚠️ column and field names/types must match)


-- Step ➋c: turn JSON objects into rows (fields ≡ columns)
--
SELECT t.*
FROM   jsonb_array_elements((TABLE like_T_but_as_JSON)) AS objs(o),
       jsonb_populate_record(NULL :: T, objs.o) AS t;
--                          ╰───┬────╯
--   derive column names and types from T's row type (cf. Chapter 02)
--            (⚠️ column and field names/types must match)


-- Steps ➊+➋: from array of JSON objects directly to typed tables
SELECT t.*
FROM   jsonb_populate_recordset(NULL :: T, (TABLE like_T_but_as_JSON)) AS t;
```

|a|b|c    |d |
|-|-|-----|--|
|1|x|true |10|
|2|y|true |40|
|3|x|false|30|
|4|y|false|20|
|5|x|true |  |

## Sequences

<img width="500" alt="Screen Shot 2022-05-23 at 3 36 43 PM" src="https://user-images.githubusercontent.com/73784742/169767737-c5bd3d17-8c1d-4eda-b776-3826ba0bffd4.png">

```sql
DROP SEQUENCE IF EXISTS seq;
CREATE SEQUENCE seq START 41 MAXVALUE 100 CYCLE;

SELECT nextval('seq');      -- ⇒ 41
SELECT nextval('seq');      -- ⇒ 42
SELECT currval('seq');      -- ⇒ 42
SELECT setval ('seq',100);  -- ⇒ 100 (+ side effect)
SELECT nextval('seq');      -- ⇒ 1   (wrap-around)
```

```sql
SELECT setval ('seq',100,false);  -- ⇒ 100 (+ side effect)
SELECT nextval('seq');            -- ⇒ 100
```

```sql
TABLE seq;
```

|last_value|log_cnt|is_called|
|----------|-------|---------|
|         1|     32|true     |

```sql
DROP TABLE IF EXISTS self_concious_T;
CREATE TABLE self_concious_T (me int GENERATED ALWAYS AS IDENTITY,
                              a  int ,
                              b  text,
                              c  boolean,
                              d  int);
```

```sql
--                    column me missing (⇒ receives GENERATED identity value)
--                         ╭───┴──╮
INSERT INTO self_concious_T(a,b,c,d) VALUES
  (1, 'x',  true, 10);

INSERT INTO self_concious_T(a,b,c,d) VALUES
  (2, 'y',  true, 40);
```

```sql
INSERT INTO self_concious_T(a,b,c,d) VALUES
  (5, 'x', true,  NULL),
  (4, 'y', false, 20),
  (3, 'x', false, 30)
  RETURNING me, c;
--            ↑
--     General INSERT feature:
--     Any list of expressions involving the column name of
--     the inserted rows (or * to return entire inserted rows)
--     ⇒ User-defined SQL functions (UDFs)
```

|me|c    |
|--|-----
| 3|true |
| 4|false|
| 5|false|

```sql
TABLE self_concious_T;
```

|me|a|b|c    |d |
|--|-|-|-----|--|
| 1|1|x|true |10|
| 2|2|y|true |40|
| 3|5|x|true |  |
| 4|4|y|false|20|
| 5|3|x|false|30|

