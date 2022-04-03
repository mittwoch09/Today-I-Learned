### Transactions and Concurrency

#### 1. Transactions
A group of SQL statements that represent a single unit of work

ACID Properties
- Atomicity
- Consistency
- Isolation
- Durability

#### 2. Creating Transactions

```sql
START TRANSACTION;

INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 1);

COMMIT;
#ROLLBACK;
```

#### 3. Concurrency and Locking
 
```sql
START TRANSACTION;
UPDATE customers
SET points = points + 10
WHERE customer_id = 1;
COMMIT;
```

#### 4. Concurrency Problems

- Lost Updates
- Dirty Reads
- Non-repeating Reads
- Phantom Reads

#### 5. Transaction Isolation Levels

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE READ

```sql
SHOW VARIABLES LIKE 'transaction_isolation';
# REPEATABLE-READ
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

#### 6. READ UNCOMMITTED Isolation Level

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT points
FROM customers
WHERE customer_id = 1;
```

```sql
START TRANSACTION;
UPDATE customers
SET points = 20
WHERE customer_id = 1;
COMMIT;
```

#### 7. READ COMMITTED Isolation Level

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT points FROM customers WHERE customer_id = 1;
SELECT points FROM customers WHERE customer_id = 1;
COMMIT;
```

```sql
START TRANSACTION;
UPDATE customers
SET points = 30
WHERE customer_id = 1;
COMMIT;
```

#### 8. REPEATABLE READ Isolation Level

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION;
SELECT * FROM customers WHERE state = 'VA';
COMMIT;
```

```sql
START TRANSACTION;
UPDATE customers
SET state = 'VA'
WHERE customer_id = 1;
COMMIT;
```

#### 9. SERIALIZABLE Isolation Level

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT * FROM customers WHERE state = 'VA';
COMMIT;
```

```sql
START TRANSACTION;
UPDATE customers
SET state = 'VA'
WHERE customer_id = 3;
COMMIT;
```

#### 10. Deadlocks

```sql
START TRANSACTION;
UPDATE customers SET state = 'VA' WHERE customer_id = 1;
UPDATE orders SET status = 1 WHERE order_id = 1;
COMMIT;
```

```sql
START TRANSACTION;
UPDATE orders SET status = 1 WHERE order_id = 1;
UPDATE customers SET state = 'VA' WHERE customer_id = 1;
COMMIT;
```
