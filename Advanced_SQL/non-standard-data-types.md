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
|----+-----------------------|----------|
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
