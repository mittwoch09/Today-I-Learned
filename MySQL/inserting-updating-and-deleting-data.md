### Inserting, Updating, and Deleting Data

#### 1. Column Attributes
Primary Key, Not Null, Defalut Value

#### 2. Inserting a Row

```sql
INSERT INTO customers (
	first_name,
    last_name,
    birth_name,
    address,
    city,
    state)
VALUES (
    'John', 
    'Smith', 
    '1990-01-01',
    'address',
    'city',
    'CA')
```

#### 3. Inserting Multiple Rows

```sql
INSERT INTO shippers (name)
VALUES ('Shipper1'),
	   ('Shipper2'),
       ('Shipper3')
```

##### Exercise
Insert three rows in the products table

```sql
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('Product1', 10, 1.95),
       ('Product2', 11, 1.95),
	   ('Product3', 12, 1.95)
```

#### 4. Inserting Hierarchical Rows

```sql
INSERT INTO orders (customer_id, order_date, status)
VALUES (1, '2019-01-02', 1);

INSERT INTO order_items
VALUES 
	(LAST_INSERT_ID(), 1, 1, 2.95),
    (LAST_INSERT_ID(), 2, 1, 3.95)
```

#### 5. Creating Copy of a Table

```sql
CREATE TABLE orders_archived AS
SELECT * FROM orders

INSERT INTO orders_archived
SELECT *
FROM orders
WHERE order_date < '2019-01-01'
```

##### Exercise

```sql
CREATE TABLE invoices_archived AS
SELECT
	i.invoice_id,
    i.number,
    c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM invoices i
JOIN clients c
	USING (client_id)
WHERE payment_date IS NOT NULL
```

#### 6. Updating a Single Row

```sql
UPDATE invoices
SET payment_total = 10, payment_date = '2019-03-01'
WHERE invoice_id = 1
```

```sql
UPDATE invoices
SET payment_total = DEFAULT, payment_date = NULL
WHERE invoice_id = 1
```

```sql
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE invoice_id = 3
```

#### 7. Updating Multiple Rows

```sql
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE client_id IN (3, 4)
```

##### Exercise
Write a SQL statement to give any customers born before 1990 and 50 extra points

```sql
UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01'
```

#### 8. Using Subqueries in Updates

```sql
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE client_id =
			(SELECT client_id
			FROM clients
			WHERE name = 'Myworks')
```

```sql
UPDATE invoices
SET 
	payment_total = invoice_total * 0.5,
	payment_date = due_date
WHERE client_id IN
			(SELECT client_id
			FROM clients
			WHERE state IN ('CA', 'NY'))
```

##### Exercise

```sql
UPDATE orders
SET comments = 'Gold customer'
WHERE customer_id IN
			(SELECT customer_id
			FROM customers
			WHERE points > 3000)
```

#### 9. Deleting Rows

```sql
DELETE FROM invoices
WHERE client_id = (
			SELECT client_id
			FROM clients
			WHERE name = 'Myworks')
```
