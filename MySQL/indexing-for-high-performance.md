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

### 4. Prefix Indexes

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

### 5. Full-text-Indexes

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
