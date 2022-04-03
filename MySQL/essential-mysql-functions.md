### Essential MySQL Functions

#### 1. Numeric Functions

```sql
SELECT ROUND(4.567, 2)
# 4.57
```

```sql
SELECT TRUNCATE(4.567, 2)
# 4.56
```

```sql
SELECT CEILING(5.2)
# 6
```

```sql
SELECT FLOOR(5.2)
# 5
```

```sql
SELECT ABS(-5.2)
# 5.2
``

```sql
SELECT RAND()
# random number
```

#### 2. String Functions

```sql
SELECT LENGTH('sky')
# 3
```

```sql
SELECT UPPER('sky')
# SKY
```

```sql
SELECT LOWER('SKY')
# sky
```

```sql
SELECT LTRIM('    sky')
# sky
```

```sql
SELECT RTRIM('sky    ')
# sky
```

```sql
SELECT TRIM('    sky    ')
# sky
```

```sql
SELECT LEFT('Kindergarten', 4)
# Kinder
```

```sql
SELECT RIGHT('Kindergarten', 6)
# garten
```

```sql
SELECT SUBSTRING('Kindergarten', 3, 5)
# nderg
```

```sql
SELECT LOCATE('n', 'Kindergarten')
# 3
```

```sql
SELECT REPLACE('Kindergarten', 'garten', 'garden')
# Kindergarden
```

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM customers
```

#### 3. Date Functions in MySQL

```sql
SELECT NOW(), CURDATE(), CURTIME()
```

```sql
SELECT YEAR(NOW()), MONTH(NOW()), DAY(NOW()), HOUR(NOW()), MINUTE(NOW()), SECOND(NOW())
```sql

```sql
SELECT DAYNAME(NOW()), MONTHNAME(NOW())
```

```sql
SELECT EXTRACT(DAY FROM NOW())
```

##### Exercise

```sql
SELECT *
FROM orders
WHERE YEAR(order_date) = YEAR(NOW())
```

#### 4. Formating Dates and Times

```sql
SELECT DATE_FORMAT(NOW(), '%M %d %Y')
# March 28 2022
```

```sql
SELECT TIME_FORMAT(NOW(), '%H:%i %p')
# 13:32 PM
```

#### 5. Calculating Dates and Times

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 YEAR)
# 2023-03-28 13:35:00
```

```sql
SELECT DATE_SUB(NOW(), INTERVAL 1 DAY)
# 2023-03-27 13:35:00
```sql

```sql
SELECT DATEDIFF('2019-01-05', '2019-01-01')
# 4
```

```sql
SELECT TIME_TO_SEC('09:00')
```


#### 6. THE IFNULL and COALESCE Functions

```sql
SELECT
	order_id,
    IFNULL(shipper_id, 'Not assigned') AS shipper
FROM orders
```

```sql
SELECT
	order_id,
    COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM orders
```

##### Exercise

```sql
SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    IFNULL(phone, 'Unknown') AS phone
FROM customers
```

#### 7. The IF Function

```sql
SELECT
	order_id,
    order_date,
    IF(YEAR(order_date) = YEAR(NOW()), 'Active', 'Archived') AS category
FROM orders
```

##### Exercise

```sql
SELECT
	product_id,
    name,
    COUNT(*) AS orders,
    IF(COUNT(*) > 1, 'Many times', 'Once') AS frequency
FROM products
JOIN order_items USING (product_id)
GROUP BY  product_id, name
```

#### 8. The CASE Operator

```sql
SELECT
	order_id,
    CASE
		WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
        WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
        WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
        ELSE 'Future'
	END AS category
FROM orders
```

##### Exercise

```sql
SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    points,
    CASE
		WHEN points > 3000 THEN 'Gold'
        WHEN points >= 2000 THEN 'Silver'
		ELSE 'Bronze'
	END AS category
FROM customers
```
