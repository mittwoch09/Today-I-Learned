### Retrieving Data From Multiple Tables

#### 1. Inner Joins

```sql
SELECT order_id, o.customer_id, first_name, last_name
FROM orders o # ALIAS
JOIN customers c # ALIAS
	ON o.customer_id = c.customer_id
```

##### Exercise
 order_id, name, product_id, quantity, unit_price

```sql
SELECT order_id, name, oi.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
```

#### 2. Joining Across Databases

```sql
SELECT *
FROM order_items oi
JOIN sql_inventory.products p
	ON oi.product_id = p.product_id
# prefix database <- same table repeated in multiple places
```

#### 3. Self Joins

```sql
SELECT
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
JOIN employees m
	ON e.reports_to = m.employee_id
```

#### 4. Joining Multiple Tables

```sql
SELECT
	o.order_id,
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```

##### Exercise
Joining payments, clients, payment_methods tables

```sql
SELECT
	p.date,
    p.invoice_id,
    p.amount,
    c.name,
    pm.name
FROM payments p
JOIN clients c
	ON p.client_id = c.client_id
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
````

#### 5. Compound Join Conditions

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_id
    AND oi.product_id = oin.product_id
```

#### 6. Implicit Join Syntax

```sql
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id
```

#### 7. Outer Joins

```sql
SELECT
	c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
	 ON c.customer_id = o.customer_id
ORDER BY c.customer_id
```

##### Exercise
product_id, name, quantity

```sql
SELECT
	p.product_id,
    p.name,
    oi.quantity
FROM products p
LEFT JOIN order_items oi
	ON p.product_id = oi.product_id
```

#### 8. Outer Join Between Multiple Tables

```sql
SELECT
	c.customer_id,
    c.first_name,
    o.order_id,
    sh.name AS shipper
FROM customers c
LEFT JOIN orders o
	ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id
```

##### Exercise
order_date, order_id, first_name, shipper, status

```sql
SELECT
	o.order_id,
    o.order_date,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
FROM  orders o
JOIN customers c
	ON o.customer_id = c.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
JOIN order_statuses os
	ON o.status = os.order_status_id
```

#### 9. Self Outer Joins

```sql
SELECT
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m
	ON e.reports_to = m.employee_id
```

#### 10. The USING Clause

```sql
SELECT
	o.order_id,
    c.first_name,
    sh.name AS shipper
FROM orders o
JOIN customers c
	USING (customer_id)
LEFT JOIN shippers sh
	USING (shipper_id)
# the column name is exactly the same across differe nt tables.
```

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	USING (order_id, product_id)
    # ON oi.order_id = oin.order_id AND
	#	 oi.product_id = oin.product_id
```

##### Exercise

```sql
SELECT
	p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM payments p
JOIN clients c USING (client_id)
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
```

#### 11. Natural Joins

```sql
SELECT
	o.order_id,
    c.first_name
FROM orders o
NATURAL JOIN customers c
```

#### 12. Cross Joins

```sql
SELECT
	c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name
```

##### Exercise
Do a cross join between shippers and products using the implicit syntax and the using the explicit syntax

```sql
SELECT
	sh.name AS shipper,
    p.name AS product
FROM shippers sh
CROSS JOIN products p
ORDER BY sh.name

SELECT
	sh.name AS shipper,
    p.name AS product
FROM shippers sh, products p
ORDER BY sh.name
```

#### 13. Unions

```sql
SELECT
	order_id,
    order_date,
    'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'
UNION
SELECT
	order_id,
    order_date,
    'Archived' AS status
FROM orders
WHERE order_date <= '2019-01-01'
```

##### Exercise

```sql
SELECT
	customer_id, 
    first_name, 
    points, 
    'Bronze' AS type
FROM customers
WHERE points < 2000
UNION
SELECT
	customer_id, 
    first_name, 
    points, 
    'Silver' AS type
FROM customers
WHERE points BETWEEN 2000 AND 3000
UNION
SELECT
	customer_id, 
    first_name, 
    points, 
    'Gold' AS type
FROM customers
WHERE points > 3000
ORDER BY first_name
```
