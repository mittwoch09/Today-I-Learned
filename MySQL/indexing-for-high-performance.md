#### 1. Indexs

- Reserve indexes for **performance critical** queries
- Design indexes based on your **queries**, not your tables.

#### 2. Creating Indexes

```sql
CREATE INDEX idx_state ON customers (state);
```

```sql
EXPLAIN SELECT customer_id FROM customers WHERE state = 'CA';
```

rows : 1010 -> 112

#### 3. Viewing Indexes

```sql
SHOW INDEXES IN customers;
```

Index_type -> BTREE(Binary Tree)

#### 4. Prefix Indexes

String Columns : `CHAR` `VARCHAR` `TEXT` `BLOB`

```sql
CREATE INDEX idx_lastname ON customers (last_name(5));
```

Find Optimal Prefix

```sql
SELECT
  COUNT(DISTINCT LEFT(last_name, 1)),
  COUNT(DISTINCT LEFT(last_name, 5)),   COUNT(DISTINCT LEFT(last_name, 10)) FROM customers;
```

#### 5. Full-text-Indexes

```sql
SELECT *
FROM posts
WHERE title LIKE '%react redux%' OR
      body LIKE '%react rexux%';
```

```sql
CREATE FULLTEXT INDEX idx_title_body ON posts (title, body);
```
```sql
SELECT *, MATCH(title, body) AGAINST('react redux')
FROM posts
WHERE MATCH(title, body) AGAINST('react redux');
```

**Relevance Score**

<img width="846" alt="Screen Shot 2022-04-08 at 4 10 00 PM" src="https://user-images.githubusercontent.com/73784742/162393736-5b0bdc9c-97db-45d6-8c37-6d5f5c8c9335.png">

#### 6. Composite Indexes

```sql
CREATE INDEX idx_state_points ON customers (state, points);
EXPLAIN SELECT customer_id FROM customers
WHERE state = 'CA' AND 'points' > 1000;
```
```sql
DROP INDEX idx_points ON customers;
```

#### 7. Order of Columns in Composite Indexes

Order of Columns
- Put the most frequently used columns first
- Put the cloumns with a higer cardinality first

<img width="845" alt="Screen Shot 2022-04-08 at 4 25 42 PM" src="https://user-images.githubusercontent.com/73784742/162396377-e138eaa3-e3c6-483f-991f-c43bd9ee0487.png">

---

<img width="846" alt="Screen Shot 2022-04-08 at 4 26 11 PM" src="https://user-images.githubusercontent.com/73784742/162396522-422d8e43-dee0-4119-a1fe-324e30eb18df.png">

- Take your queries into account

#### 8. When Indees are Ignored

```sql
SELECT customer_id FROM customers
WHERE state = 'CA' OR points > 1000;
```
```sql
CREATE INDEX idx_points ON customers (points);

EXPLAIN
  SELECT customer_id FROM customers
  WHERE state = 'CA'
  UNION
  SELECT customer_id FROMS customers
  WHERE points > 1000;
```

1010 records -> 640 records

```sql
EXPLAIN SELECT customer_id FROM customers
WHERE points + 10 > 2010;
```

1010 records

```sql
EXPLAIN SELECT customer_id FROM customers
WHERE points > 2000;
```

3 records!

#### 9. Using Indexes for Sorting

The basic rule of thumb is that these columns that you have with the order by clause should be in the same order as the columns in the index.

```sql
EXPALIN SELECT customer_id FROM customers
ORDER BY state DESC, points DESC;
SHOW STATUS LIKE 'last_query_cost';
```

#### 10. Covering Indexes

When designing your indexes fist look at your where clause, then look at the columns in the order by clause. And finally look ath the columns used in the select clause.

#### 11. Index Maintenance

`Duplicate Indexes` , `Redundant Indexes`

Before creating new indexes, check the existing ones.

#### Summary

1. Smaller tables perform better. Don’t store the data you don’t need. Solve today’s problems, not tomorrow’s future problems that may never happen.
2. Use the smallest data types possible. If you need to store people’s age, a TINYINT is sufficient. No need to use an INT. Saving a few bytes is not a big deal in a small table, but has a significant impact in a table with millions of records.
3. Every table must have a primary key. 
4. Primary keys should be short. Prefer TINYINT to INT if you only need to store a hundred records.
5. Prefer numeric types to strings for primary keys. This makes looking up records by the primary key faster.
6. Avoid BLOBs. They increase the size of your database and have a negative impact on the performance. Store your files on disk if you can.
7. If a table has too many columns, consider splitting it into two related tables using a one-to-one relationship. This is called vertical partitioning. For example, you may have a customers table with columns for storing their address. If these columns don’t get read often, split the table into two tables (users and user_addresses). 
8. In contrast, if you have several joins in your queries due to data fragmentation, you may want to consider denormalizing data. Denormalizing is the opposite of normalization. It involves duplicating a column from one table in another table (to reduce the number of joins) required.
9. Consider creating summary/cache tables for expensive queries. For example, if the query to fetch the list of forums and the number of posts in each forum is expensive, create a table called forums_summary that contains the list of forums and the number of posts in them. You can use events to regularly refresh the data in this table. You may also use triggers to update the counts every time there is a new post.
10. Full table scans are a major cause of slow queries. Use the EXPLAIN statement and look for queries with type = ALL. These are full table scans. Use indexes to optimize these queries.
11. When designing indexes, look at the columns in your WHERE clauses first. Those are the first candidates because they help narrow down the searches. Next, look at the columns used in the ORDER BY clauses. If they exist in the index, MySQL can scan your index to return ordered data without having to perform a sort operation (filesort). Finally, consider adding the columns in the SELECT clause to your indexes. This gives you a covering index that covers everything your query needs. MySQL doesn’t need to retrieve anything from your tables.
12. Prefer composite indexes to several single-column index.
13. The order of columns in indexes matter. Put the most frequently used columns and the columns with a higher cardinality first, but always take your queries into account.
14. Remove duplicate, redundant and unused indexes. Duplicate indexes are the indexes on the same set of columns with the same order. Redundant indexes are unnecessary indexes that can be replaced with the existing indexes. For example, if you have an index on columns (A, B) and create another index on column (A), the latter is redundant because the former index can help.
15. Don’t create a new index before analyzing the existing ones. 
16. Isolate your columns in your queries so MySQL can use your indexes. 
17. Avoid SELECT *(all). Most of the time, selecting all columns ignores your indexes and returns unnecessary columns you may not need. This puts an extra load on your database server. 
18. Return only the rows you need. Use the LIMIT clause to limit the number of rows returned. 
19. Avoid LIKE expressions with a leading wildcard (eg LIKE ‘%name’). 
20. If you have a slow query that uses the OR operator, consider chopping up the query into two queries that utilize separate indexes and combine them using the UNION operator.
