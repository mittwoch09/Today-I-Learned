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

---

```sql
DROP TABLE IF EXISTS prehistoric;
CREATE TABLE prehistoric (class        text,
                          "herbivore?" boolean,
                          legs         int,
                          species      text);

INSERT INTO prehistoric VALUES
  ('mammalia',  true, 2, 'Megatherium'),
  ('mammalia',  true, 4, 'Paraceratherium'),
  ('mammalia', false, 2, NULL),
  ('mammalia', false, 4, 'Sabretooth'),
  ('reptilia',  true, 2, 'Iguanodon'),
  ('reptilia',  true, 4, 'Brachiosaurus'),
  ('reptilia', false, 2, 'Velociraptor'),
  ('reptilia', false, 4, NULL);
  
 TABLE prehistoric;
 ```

|class   |herbivore?|legs|species        |
|--------|----------|----|---------------|
|mammalia|true      |   2|Megatherium    |
|mammalia|true      |   4|Paraceratherium|
|mammalia|false     |   2|               |
|mammalia|false     |   4|Sabretooth     |
|reptilia|true      |   2|Iguanodon      |
|reptilia|true      |   4|Brachiosaurus  |
|reptilia|false     |   2|Velociraptor   |
|reptilia|false     |   4|               |

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

### GROUP BY

<img width="500" alt="Screen Shot 2022-05-10 at 2 57 37 PM" src="https://user-images.githubusercontent.com/73784742/167567149-5e040ccd-9d83-4256-b7a8-1067619ad967.png">

```sql
SELECT t.b                           AS "group",
       COUNT(*)                      AS size,
       SUM(t.d)                      AS "∑d",
       bool_and(t.a % 2 = 0)         AS "∀even(a)",
       string_agg(t.a :: text, ';')  AS "all a"
FROM   T AS t
GROUP BY t.b;
```

|group|size|∑d|∀even(a)|all a|
|-----|----|--|--------|-----|
|y    |   2|60|true    |2;4  |
|x    |   3|40|false   |1;3;5|

HAVING p acts like WHERE but after grouping: p = false discards groups (not rows).

```sql
SELECT t.b                           AS "group",
       COUNT(*)                      AS size,
       SUM(t.d)                      AS "∑d",
       bool_and(t.a % 2 = 0)         AS "∀even(a)",
       string_agg(t.a :: text, ';')  AS "all a"
FROM   T AS t
GROUP BY t.b
HAVING COUNT(*) > 2;
```

|group|size|∑d|∀even(a)|all a|
|-----|----|--|--------|-----|
|x    |   3|40|false   |1;3;5|

```sql
SELECT t.a % 2 AS "a odd?",
       COUNT(*) AS size
FROM   T AS t
GROUP BY t.a % 2;
```

|a odd?|size|
|------|----|
|     0|   2|
|     1|   3|

⚠️ t.a is **not** constant in each group (but t.a % 2 is)

```sql
SELECT t.b AS "group",
       t.a % 2 AS "a odd?" -- constant in the 'x'/'y' groups, but PostgreSQL doesn't know...
FROM   T AS t
GROUP BY t.b, t.a % 2;
```

|group|a odd?|
|-----|------|
|y    |     0|
|x    |     1|

• if t.b = 'x', then t.a is odd  ⇒ x | 1;3;5
• if t.b = 'y', then t.a is even ⇒ y | 2;4

functionally dependent on t.b ⇒ will not affect grouping, list here explicitly as grouping criterion, so we may use it in the SELECT clause

### Bag and Set Operations

<img width="500" alt="Screen Shot 2022-05-10 at 3 55 49 PM" src="https://user-images.githubusercontent.com/73784742/167578101-87bcb7e5-7664-4a0b-8249-215ac2d1df01.png">

For all bag/set operations, the Left/Right argument tables need to contribute compatible rows:
 • row widths must match
 • field types in corresponding columns must be cast-compatible
 • the row type of the Left argument determines the result's field types and names

 ```sql
SELECT t.*
FROM   T AS t
WHERE  t.c
  UNION ALL
SELECT t.*
FROM   T AS t
WHERE  NOT t.c;
```

|a|b|c    |d |
|-|-|-----|--|
|1|x|true |10|
|2|y|true |40|
|5|x|true |  |
|3|x|false|30|
|4|y|false|20|

```sql
SELECT t.b
FROM   T AS t
WHERE  t.c
  UNION ALL -- <-> UNION (queries contribute duplicate rows)
SELECT t.b
FROM   T AS t
WHERE  NOT t.c;
```

|b|
|-|
|x|
|y|
|x|
|x|
|y|

```sql
SELECT '1' AS q, t.b
FROM   T AS t
WHERE  t.c
  UNION ALL
SELECT '2' AS q, t.b
FROM   T AS t
WHERE  NOT t.c;
```

|q|b|
|-|-|
|1|x|
|1|y|
|1|x|
|2|x|
|2|y|

```sql
SELECT t.b        -- ⎫
FROM   T AS t     -- ⎬  q₁ contributes 2 × 'x', 1 × 'y'
WHERE  t.c        -- ⎭
  EXCEPT ALL
SELECT t.b        -- ⎫
FROM   T AS t     -- ⎬  q₂ contributes 1 × 'x', 1 × 'y'
WHERE  NOT t.c;   -- ⎭
```

|b|
|-|
| |

```sql
SELECT t.b        -- ⎫
FROM   T AS t     -- ⎬  q₂ contributes 1 × 'x', 1 × 'y'
WHERE  NOT t.c    -- ⎭
  EXCEPT ALL
SELECT t.b        -- ⎫
FROM   T AS t     -- ⎬  q₁ contributes 2 × 'x', 1 × 'y'
WHERE  t.c;       -- ⎭
```

|b|
|-|
| |

### GROUP BYs: GROUPING SETS

<img width="500" alt="Screen Shot 2022-05-10 at 5 12 36 PM" src="https://user-images.githubusercontent.com/73784742/167593598-e3d8d33f-366e-487a-97ee-3dc825d8c00f.png">

```sql
SELECT p.class,
       p."herbivore?",
       p.legs,
       string_agg(p.species, ', ') AS species  -- string_agg ignores NULL (may use COALESCE(p.species, '?'))
FROM   prehistoric AS p
GROUP BY GROUPING SETS ((class), ("herbivore?"), (legs));
```

|class   |herbivore?|legs|species                                               |
|--------|----------|----|------------------------------------------------------|
|reptilia|          |    |Iguanodon, Brachiosaurus, Velociraptor                |
|mammalia|          |    |Megatherium, Paraceratherium, Sabretooth              |
|        |false     |    |Sabretooth, Velociraptor                              |
|        |true      |    |Megatherium, Paraceratherium, Iguanodon, Brachiosaurus|
|        |          |   4|Paraceratherium, Sabretooth, Brachiosaurus            |
|        |          |   2|Megatherium, Iguanodon, Velociraptor                  |


Equivalent to GROUPING SETS ((class), ("herbivore?"), (legs))

```sql
SELECT p.class,
       NULL :: boolean             AS "herbivore?", -- ⎱  NULL is polymorphic ⇒ PostgreSQL
       NULL :: int                 AS legs,         -- ⎰  will default to type text
       string_agg(p.species, ', ') AS species
FROM   prehistoric AS p
GROUP BY p.class

  UNION ALL

SELECT NULL :: text                AS class,
       p."herbivore?",
       NULL :: int                 AS legs,
       string_agg(p.species, ',' ) AS species
FROM   prehistoric AS p
GROUP BY p."herbivore?"

  UNION ALL

SELECT NULL :: text                AS class,
       NULL :: boolean             AS "herbivore?",
       p.legs AS legs,
       string_agg(p.species, ', ') AS species
FROM   prehistoric AS p
GROUP BY p.legs;
```

### ROLLUP

<img width="500" alt="Screen Shot 2022-05-10 at 5 20 45 PM" src="https://user-images.githubusercontent.com/73784742/167595226-f316c0b3-fb59-490b-abd8-61b77f8dd63c.png">

```sql
SELECT p.class,
       p."herbivore?",
       p.legs,
       string_agg(p.species, ', ') AS species
FROM   prehistoric AS p
GROUP BY ROLLUP (class, "herbivore?", legs);
-- optional: "visualize" hierarchy (least specific last)
-- ORDER BY (class IS NULL) :: int + ("herbivore?" IS NULL) :: int + (legs IS NULL) :: int, class, "herbivore?", legs
```

|class   |herbivore?|legs|species                                                                         |
|--------|----------|----|--------------------------------------------------------------------------------|
|mammalia|false     |   2|                                                                                |
|mammalia|false     |   4|Sabretooth                                                                      |
|mammalia|true      |   2|Megatherium                                                                     |
|mammalia|true      |   4|Paraceratherium                                                                 |
|reptilia|false     |   2|Velociraptor                                                                    |
|reptilia|false     |   4|                                                                                |
|reptilia|true      |   2|Iguanodon                                                                       |
|reptilia|true      |   4|Brachiosaurus                                                                   |
|mammalia|false     |    |Sabretooth                                                                      |
|mammalia|true      |    |Megatherium, Paraceratherium                                                    |
|reptilia|false     |    |Velociraptor                                                                    |
|reptilia|true      |    |Iguanodon, Brachiosaurus                                                        |
|mammalia|          |    |Sabretooth, Megatherium, Paraceratherium                                        |
|reptilia|          |    |Velociraptor, Iguanodon, Brachiosaurus                                          |
|        |          |    |Sabretooth, Megatherium, Paraceratherium, Velociraptor, Iguanodon, Brachiosaurus|

<img width="600" alt="Screen Shot 2022-05-10 at 5 31 04 PM" src="https://user-images.githubusercontent.com/73784742/167597320-c64c749f-4c0e-466c-9651-b1216648c800.png">

```sql
SELECT string_agg(p.species, ', ') AS species
FROM   prehistoric AS p
GROUP BY ();
```

|species                                                                         |
|--------------------------------------------------------------------------------|
|Megatherium, Paraceratherium, Sabretooth, Iguanodon, Brachiosaurus, Velociraptor|

With the empty set ∅ ≡ () of grouping criteria, all rows form a **single** large group:

### CUBE

<img width="500" alt="Screen Shot 2022-05-10 at 5 38 57 PM" src="https://user-images.githubusercontent.com/73784742/167598983-62c5ceeb-eed7-473e-bc20-b95de7d1a52e.png">

```sql
SELECT p.class,
       p."herbivore?",
       p.legs,
       string_agg(p.species, ', ') AS species
FROM   prehistoric AS p
GROUP BY CUBE (class, "herbivore?", legs);
```

|class   |herbivore?|legs|species                                                                         |
|--------|----------|----|--------------------------------------------------------------------------------|
|mammalia|false     |   2|                                                                                |
|mammalia|false     |   4|Sabretooth                                                                      |
|mammalia|true      |   2|Megatherium                                                                     |
|mammalia|true      |   4|Paraceratherium                                                                 |
|reptilia|false     |   2|Velociraptor                                                                    |
|reptilia|false     |   4|                                                                                |
|reptilia|true      |   2|Iguanodon                                                                       |
|reptilia|true      |   4|Brachiosaurus                                                                   |
|mammalia|false     |    |Sabretooth                                                                      |
|mammalia|true      |    |Megatherium, Paraceratherium                                                    |
|mammalia|          |   2|Megatherium                                                                     |
|mammalia|          |   4|Sabretooth, Paraceratherium                                                     |
|reptilia|false     |    |Velociraptor                                                                    |
|reptilia|true      |    |Iguanodon, Brachiosaurus                                                        |
|reptilia|          |   2|Velociraptor, Iguanodon                                                         |
|reptilia|          |   4|Brachiosaurus                                                                   |
|        |false     |   2|Velociraptor                                                                    |
|        |false     |   4|Sabretooth                                                                      |
|        |true      |   2|Megatherium, Iguanodon                                                          |
|        |true      |   4|Paraceratherium, Brachiosaurus                                                  |
|mammalia|          |    |Sabretooth, Megatherium, Paraceratherium                                        |
|reptilia|          |    |Velociraptor, Iguanodon, Brachiosaurus                                          |
|        |false     |    |Velociraptor, Sabretooth                                                        |
|        |true      |    |Megatherium, Iguanodon, Paraceratherium, Brachiosaurus                          |
|        |          |   2|Megatherium, Velociraptor, Iguanodon                                            |
|        |          |   4|Sabretooth, Paraceratherium, Brachiosaurus                                      |
|        |          |    |Sabretooth, Megatherium, Paraceratherium, Velociraptor, Iguanodon, Brachiosaurus|

<img width="600" alt="Screen Shot 2022-05-10 at 5 43 21 PM" src="https://user-images.githubusercontent.com/73784742/167599882-b7c9b133-4eeb-451d-ab3b-158b75d3adcc.png">

### SQL evaluation order 

<img width="500" alt="Screen Shot 2022-05-11 at 11 51 10 AM" src="https://user-images.githubusercontent.com/73784742/167765616-5601767e-dad9-49fb-8223-4ab121c6f879.png">

```sql
EXPLAIN VERBOSE

SELECT DISTINCT ON ("∑d") 1 AS branch, NOT t.c AS "¬c", SUM(t.d) AS "∑d"
FROM   T AS t
WHERE  t.b = 'x'
GROUP BY "¬c"
HAVING SUM(t.d) > 0

  UNION ALL

SELECT DISTINCT ON ("∑d") 2 AS branch, NOT t.c AS "¬c", SUM(t.d) AS "∑d"
FROM   T AS t
WHERE  t.b = 'x'
GROUP BY "¬c"
HAVING SUM(t.d) > 0

ORDER BY branch
OFFSET 0
LIMIT  7;
```

<img width="600" alt="Screen Shot 2022-05-11 at 12 00 56 PM" src="https://user-images.githubusercontent.com/73784742/167766563-1a215c2d-f6bb-4f06-9d53-5d21562302f7.png">

### WITH (Common Table Expressions)

<img width="500" alt="Screen Shot 2022-05-11 at 12 04 11 PM" src="https://user-images.githubusercontent.com/73784742/167766899-d0b1058a-eba0-4d88-b48b-5b9d5094cf66.png">

<img width="500" alt="Screen Shot 2022-05-11 at 12 04 46 PM" src="https://user-images.githubusercontent.com/73784742/167766958-7709dac5-6194-4779-9dd6-e093e5fd7b2f.png">

#### Use Case: WITH

<img width="500" alt="Screen Shot 2022-05-11 at 1 46 43 PM" src="https://user-images.githubusercontent.com/73784742/167777388-8ea3485e-b51d-45ac-9f04-41557bc0de64.png">

```sql
DROP TABLE IF EXISTS dinosaurs;
CREATE TABLE dinosaurs (species text, height float, length float, legs int);

INSERT INTO dinosaurs(species, height, length, legs) VALUES
  ('Ceratosaurus',      4.0,   6.1,  2),
  ('Deinonychus',       1.5,   2.7,  2),
  ('Microvenator',      0.8,   1.2,  2),
  ('Plateosaurus',      2.1,   7.9,  2),
  ('Spinosaurus',       2.4,  12.2,  2),
  ('Tyrannosaurus',     7.0,  15.2,  2),
  ('Velociraptor',      0.6,   1.8,  2),
  ('Apatosaurus',       2.2,  22.9,  4),
  ('Brachiosaurus',     7.6,  30.5,  4),
  ('Diplodocus',        3.6,  27.1,  4),
  ('Supersaurus',      10.0,  30.5,  4),
  ('Albertosaurus',     4.6,   9.1,  NULL),  -- Bi-/quadropedality is
  ('Argentinosaurus',  10.7,  36.6,  NULL),  -- unknown for these species.
  ('Compsognathus',     0.6,   0.9,  NULL),  --
  ('Gallimimus',        2.4,   5.5,  NULL),  -- Try to infer pedality from
  ('Mamenchisaurus',    5.3,  21.0,  NULL),  -- their ratio of body height
  ('Oviraptor',         0.9,   1.5,  NULL),  -- to length.
  ('Ultrasaurus',       8.1,  30.5,  NULL);  --

TABLE dinosaurs;
```

|species        |height|length|legs|
|---------------|------|------|----
|Ceratosaurus   |   4.0|   6.1|   2|
|Deinonychus    |   1.5|   2.7|   2|
|Microvenator   |   0.8|   1.2|   2|
|Plateosaurus   |   2.1|   7.9|   2|
|Spinosaurus    |   2.4|  12.2|   2|
|Tyrannosaurus  |   7.0|  15.2|   2|
|Velociraptor   |   0.6|   1.8|   2|
|Apatosaurus    |   2.2|  22.9|   4|
|Brachiosaurus  |   7.6|  30.5|   4|
|Diplodocus     |   3.6|  27.1|   4|
|Supersaurus    |  10.0|  30.5|   4|
|Albertosaurus  |   4.6|   9.1|    |
|Argentinosaurus|  10.7|  36.6|    |
|Compsognathus  |   0.6|   0.9|    |
|Gallimimus     |   2.4|   5.5|    |
|Mamenchisaurus |   5.3|  21.0|    |
|Oviraptor      |   0.9|   1.5|    |
|Ultrasaurus    |   8.1|  30.5|    |

Infer bipedality (or quadropedality) for dinosaurs based on their body length and shape.

<img width="500" alt="Screen Shot 2022-05-11 at 1 50 57 PM" src="https://user-images.githubusercontent.com/73784742/167777884-9eae2f42-4034-490d-afa6-5f0508a1ea37.png">

```sql
-- ➊ Determine characteristic height/length (= body shape) ratio
-- separately for bipedal and quadropedal dinosaurs:
WITH bodies(legs, shape) AS (
  SELECT d.legs, AVG(d.height / d.length) AS shape
  FROM   dinosaurs AS d
  WHERE  d.legs IS NOT NULL
  GROUP BY d.legs
)
-- ➋ Realize query plan (assumes table bodies exists)
SELECT d.species, d.height, d.length,
       (SELECT b.legs                               -- Find the shape entry in bodies
        FROM   bodies AS b                          -- that matches d's ratio of
        ORDER BY abs(b.shape - d.height / d.length) -- height to length the closest
        LIMIT 1) AS legs                            -- (pick row with minimal shape difference)
FROM  dinosaurs AS d
WHERE d.legs IS NULL
                      -- ↑ Locomotion of dinosaur d is unknown
  UNION ALL           ----------------------------------------
                      -- ↓ Locomotion of dinosaur d is known
SELECT d.*
FROM   dinosaurs AS d
WHERE  d.legs IS NOT NULL;
```

