## Data Types in (Postgre)SQL

```sql
SELECT string_agg(t.typname, ' ') AS "data types"
FROM   pg_catalog.pg_type AS t
WHERE  t.typelem  = 0 AND  t.typrelid = 0;
```

| data types                                           |
|------------------------------------------------------|
| bool bytea char int8 int2 int4 regproc text oid tid  |
| oid tid xid cid json xml pg_node_tree pg_ddl_command |
| smgr path polygon float4 float8 abstime reltime      |
| tinterval unknown circle money macaddr inet cidr ⋯   |

https://www.postgresql.org/docs/current/datatype.html

## SQL Type Casts

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


#### Runtime type conversion

```sql
SELECT 6.2 :: int;
```

|int4|
|----|
|   6|

```sql
SELECT 6.6 :: int;
```

|int4|
|----|
|   7|

```sql
SELECT date('May 4, 2022');
```

|date      |
|----------|
|2022-05-04|

### Implicit conversion if target type is known

```sql
INSERT INTO T(a,b,c,d) VALUES (6.2, NULL, 'true', '0');
--                              ↑     ↑      ↑     ↑
--                             int  text  boolean int
```

### Literal input syntax using '...' (cast from text to any other type):

```sql
SELECT booleans.yup :: boolean, booleans.nope :: boolean
FROM   (VALUES ('true', 'false'),
               ('True', 'False'),
               ('t',    'f'),               
               ('1',    '0'),
               ('yes',  'no'),
               ('on',   'off')) AS booleans(yup, nope);
```

|yup |nope |
|----|-----|
|true|false|
|true|false|
|true|false|
|true|false|
|true|false|

### Literals (Casts From text)

<img width="500" alt="Screen Shot 2022-05-20 at 12 06 13 PM" src="https://user-images.githubusercontent.com/73784742/169448375-8e0ef3c8-ee40-4acb-a1ef-31a70072b1a6.png">

## Text Data Types


<img width="500" alt="Screen Shot 2022-05-20 at 1 12 09 PM" src="https://user-images.githubusercontent.com/73784742/169455135-828141ad-da77-4e8e-b6b6-57f31f2965c9.png">

```sql
SELECT '01234' :: char(3);
```

|bpchar|
|------|
|012   |

truncation to enforce limit after cast

```sql
SELECT t.c :: char(10)
FROM   (VALUES ('01234'),
			   ('0123456789') --- padding with 5 x '⎵' padding when result is printed
	   ) AS t(c);
```
|c         |
|----------|
|01234     |
|0123456789|

```sql
SELECT '012' :: char(5) = '012  ' :: char(5);
```

|?column?|
|--------|
|true    |

Values of type char(n) are padded with blanks (spaces, ⎵) when stored or printed, but trailing blanks are removed before computation.

```sql
SELECT t.c,
       length(t.c)       AS chars,
       octet_length(t.c) AS bytes
FROM   (VALUES ('x'),
               ('⚠'), -- ⚠ = U+26A0, in UTF8: 0xE2 0x9A 0xA0
               ('👩🏾')
       ) AS t(c);
```

|c   |chars|bytes|
|----|-----|-----|
|x   |    1|    1|
|⚠   |    1|    3|
|👩🏾|    2|    8|

```sql
SELECT octet_length('012346789' :: varchar(5)) AS c1, -- 5 (truncation)
       octet_length('012'       :: varchar(5)) AS c2, -- 3 (within limits)
       octet_length('012'       :: char(5))    AS c3, -- 5 (blank padding in storage)
       length('012'             :: char(5))    AS c4, -- 3 (padding in storage only)
       length('012  '           :: char(5))    AS c5; -- 3 (trailing blanks removed)
```

|c1|c2|c3|c4|c5|
|--|--|--|--|--|
| 5| 3| 5| 3| 3|

## NUMERIC: Large Numeric Values with Exact Arithmetics

<img width="500" alt="Screen Shot 2022-05-20 at 1 23 35 PM" src="https://user-images.githubusercontent.com/73784742/169456334-11ef1e5f-a98d-4b73-9e59-7cd713e725e7.png">

```sql
SELECT (2::numeric)^100000; -- OK¹ (⚠ SQL syntax allows numeric(1000) only)

-- ¹ PostgresSQL actual limits:
--   up to 131072 digits before the decimal point,
--   up to 16383 digits after the decimal point
```

## Timestamps and Time Intervals

```sql
SELECT 'now'::date      AS "now (date)",
       'now'::time      AS "now (time)",
       'now'::timestamp AS "now (timestamp)";
```

|now (date)|now (time)|now (timestamp)        |
|----------|----------|-----------------------|
|2022-05-21|  11:31:30|2022-05-21 11:31:30.477|

```sql
SELECT 'now'::timestamp AS now,
       'now'::timestamp with time zone AS "now with tz";
```

|now                    |now with tz                  |
|-----------------------|-----------------------------|
|2022-05-21 11:32:59.725|2022-05-21 11:32:59.725 +0800|

### Dates may be specified in a variety of forms

```sql
SELECT COUNT(DISTINCT birthdays.d::date) AS interpretations
FROM   (VALUES ('August 26, 1968'),
               ('Aug 26, 1968'),
               ('8.26.1968'),
               ('08-26-1968'),
               ('8/26/1968')) AS birthdays(d);
```

|interpretations|
|---------------|
|              1|

### Special timestamps and dates

```sql
SELECT 'epoch'::timestamp    AS epoch,
       'infinity'::timestamp AS infinity,
       'today'::date         AS today,
       'yesterday'::date     AS yesterday,
       'tomorrow'::date      AS tomorrow;
```

|epoch                  |infinity|today     |yesterday |tomorrow  |
|-----------------------|--------|----------|----------|----------|
|1970-01-01 00:00:00.000|infinity|2022-05-21|2022-05-20|2022-05-22|

```sql
SELECT '1 year 2 months 3 days 4 hours 5 minutes 6 seconds'::interval
        =
       'P1Y2M3DT4H5M6S'::interval; -- ISO 8601
--      └──┬──┘└──┬──┘
--   date part   time part
```

|?column?|
|--------|
|true    |

### Date/time arithmetics with intervals

```sql
SELECT 'Aug 31, 2035'::date - 'now'::timestamp                     AS retirement,
       'now'::date + '30 days'::interval                           AS in_one_month,
       'now'::date + 2 * '1 month'::interval                       AS in_two_months,
       'tomorrow'::date - 'now'::timestamp                         AS til_midnight,
        extract(hours from ('tomorrow'::date - 'now'::timestamp))  AS hours_til_midnight,
       'tomorrow'::date - 'yesterday'::date                        AS two, -- ⚠ yields int
       make_interval(days => 'tomorrow'::date - 'yesterday'::date) AS two_days;
```

|retirement               |in_one_month           |in_two_months          |til_midnight   |hours_til_midnight|two|two_days|
|-------------------------|-----------------------|-----------------------|---------------|------------------|---|--------|
|4849 days 12:21:15.552393|2022-06-20 00:00:00.000|2022-07-21 00:00:00.000|12:21:15.552393|                12|  2|  2 days|

```sql
SELECT (make_date(2022, months.m, 1) - '1 day'::interval)::date AS last_day_of_month
FROM   generate_series(1,12) AS months(m);
```

|last_day_of_month|
|-----------------|
|       2021-12-31|
|       2022-01-31|
|       2022-02-28|
|       2022-03-31|
|       2022-04-30|
|       2022-05-31|
|       2022-06-30|
|       2022-07-31|
|       2022-08-31|
|       2022-09-30|
|       2022-10-31|
|       2022-11-30|

```sql
SELECT timezones.tz AS timezone,
       'now'::timestamp with time zone -- uses default ("show time zone")
         -
       ('now'::timestamp::text || ' ' || timezones.tz)::timestamp with time zone AS difference
FROM   (VALUES ('America/New_York'),
               ('Europe/Berlin'),
               ('Asia/Singapore'),
               ('PST'),
               ('UTC'),
               ('UTC-6'),
               ('+3')
       ) AS timezones(tz)
ORDER BY difference;
```

|timezone        |difference|
|----------------|----------|
|PST             | -16:00:00|
|America/New_York| -12:00:00|
|UTC             | -08:00:00|
|Europe/Berlin   | -06:00:00|
|+3              | -05:00:00|
|UTC-6           | -02:00:00|
|Asia/Singapore  |  00:00:00|

```sql
SELECT holiday.holiday
FROM   (VALUES ('Easter',    'Apr 19, 2022', 'Apr 23, 2022'),
               ('Summer',    'May 21, 2022', 'May 25, 2022'),
               ('Autumn',    'Nov  2, 2022', 'Nov  4, 2022'),
               ('Winter',    'Dec 21, 2022', 'Jan  7, 2023')) AS holiday(holiday, "start", "end")
WHERE  (holiday.start :: date, holiday.end :: date) overlaps ('today','today');
```

|holiday|
|-------|
|Summer |
