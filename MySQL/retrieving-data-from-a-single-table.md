### Retrieving Data From a Single Table

#### 1. The SELECT Statement

```sql
USE sql_store;

SELECT *
FROM customers
-- WHERE customer_id = 1
-- ORDER BY first_name
```

#### 2. The SELECT Clause

```sql
SELECT
	first_name, 
    last_name, 
    points, 
    (points + 10) * 100 AS 'discount factor'
FROM customers

SELECT DISTINCT state
FROM customers
```

##### Exercise
Return all the products's name, unit price, new price (unit price * 1.1) 

```sql
SELECT
	name,
    unit_price,
    unit_price * 1.1 AS 'new price'
FROM products
```

#### 3. The WHERE Clase

```sql
SELECT *
FROM Customers
WHERE birth_date > '1990-01-01'
# >, <, >=, <=, !=, <>
```

##### Exercise
Get the orders placed this year

```sql
SELECT *
FROM orders
WHERE order_date >= '2019-01-01'
```

#### 4. The AND, OR and NOT Operators

```sql
SELECT *
FROM Customers
WHERE birth_date > '1990-01-01' OR 
	  (points > 1000 AND state = 'VA')
# AND operator is always evaluated first
```

```sql
SELECT *
FROM Customers
WHERE NOT (birth_date > '1990-01-01' OR points > 1000)
# >/<= , OR /AND, >/<=
```

##### Exercise

From the order_items table, get the itmes for order #6 where the total price is greater than 30

```sql
SELECT *
FROM order_items
WHERE order_id = 6 AND unit_price * quantity > 30
```

#### 5. The IN Operater

```sql
SELECT *
FROM Customers
WHERE state NOT IN ('VA', 'FL', 'GA')
# => WHERE state = 'VA' OR state = 'FL' OR state = 'GA'
```

##### Exercise

Return products with quantity in stock equal to 49, 38, 72

```sql
SELECT *
FROM products
WHERE quantity_in_stock IN (49, 38, 72)
```

#### 6. The BETWEEN Operator

```sql
SELECT *
FROM customers
WHERE points BETWEEN 1000 AND 3000
```

#### Exercise

Return customers born between 1/1/1990 and 1/1/2000

```sql
SELECT *
FROM customers
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01'
```

#### 7. The LIKE Operator

```sql
SELECT *
FROM customers
WHERE last_name LIKE '%b%'
# % -> any number of characters
```

```sql
SELECT *
FROM customers
WHERE last_name LIKE 'b____y'
# _ -> single character
```

##### Exercise

Get the customers whose addresses contain TRAIL or AVENUE

```sql
SELECT *
FROM customers
WHERE address LIKE '%trail%' OR 
	  address LIKE '%avenue%'
```

phone numbers end with 9

```sql
SELECT *
FROM customers
WHERE phone LIKE '%9'
```

#### 8. The REGEXP Operator

```sql
SELECT *
FROM customers
WHERE last_name REGEXP 'field'
# WHERE last_name LIKE '%field%'  
```

```sql
SELECT *
FROM customers
WHERE last_name REGEXP 'field$|mac|rose'
```

```sql
SELECT *
FROM customers
WHERE last_name REGEXP '[gim]e'
# -> ge, ie, me
```

```sql
SELECT *
FROM customers
WHERE last_name REGEXP '[a-h]e'
```

##### Exercise

Get the customers whose first names are ELKA or AMBUR

```sql
SELECT *
FROM customers
WHERE first_name REGEXP 'elka|ambur'
```

Get the customers whose last names end with EY or ON

```sql
SELECT *
FROM customers
WHERE last_name REGEXP 'ey$|on$'
```

Get the customers whose last names start with MY or contains SE

```sql
SELECT *
FROM customers
WHERE last_name REGEXP '^my|se'
```

Get the customers whose last names contain B followed by R or U

```sql
SELECT *
FROM customers
WHERE last_name REGEXP 'b[ru]'
```

`^` begining, `$` end, `|` logical or, `[abcd]` list, `[a-f]` range

#### 9. The IS NULL Operator

```sql
SELECT *
FROM customers
WHERE phone IS NOT NULL
```

##### Exercise

Get the orders that are not shipped

```sql
SELECT *
FROM orders
WHERE shipped_date IS NULL
```

#### 10. The ORDER BY Clause

```sql
SELECT first_name, last_name, 10 AS points
FROM customers
ORDER BY first_name
```

##### Exercise

```sql
SELECT *, quantity * unit_price AS total_price
FROM order_items
WHERE order_id = 2
ORDER BY total_price DESC
```

#### 10. The LIMTI Clause

```sql
SELECT *
FROM customers
LIMIT 6, 3
# OFFSET 6 and LIMIT 3
```

##### Exercise

Get the top three loyal customers

```sql
SELECT *
FROM customers
ORDER BY points DESC
LIMIT 3
```
