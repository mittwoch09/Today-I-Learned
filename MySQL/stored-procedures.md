### Stored Procedures

#### 1. What are Stored Procedures

- Store and organize SQL
- Faster execution
- Data security

#### 2. Creating a Stored Procedure

```sql
DELIMITER $$
CREATE PROCEDURE get_clients()
BEGIN
	SELECT * FROM clients;
END$$

DELIMITER ;
```

```sql
CALL get_clients()
```

##### Exercise
Create a stored procedure called get_invoices_with_balance to return all the invoices with a balance > 0

```sql
DELIMITER $$
CREATE PROCEDURE get_invoices_with_balance()
BEGIN
	SELECT * 
	FROM invoices_with_balance # views
	WHERE balance > 0;
END$$

DELIMITER ;
```

#### 3. Creating Procedures Using MySQLWorkbench

right click on the store precedures folder, and create stored procedure

#### 4. Dropping Stored Procedures

```sql
DROP PROCEDURE IF EXISTS get_clients;
```

#### 5. Parameters

```sql
DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
	state CHAR(2) -- CA, NY
)
BEGIN
	SELECT * FROM clients c
    WHERE c.state = state;
END$$

DELIMITER ;
```

```sql
CALL get_clients_by_state('CA')
```

##### Exercise
Write a stored procedure called get_invoice_by_client to return invoices for a given client

```sql
DELIMITER $$
CREATE PROCEDURE get_invoices_by_client
(
	client_id INT
)
BEGIN
	SELECT * 
    FROM invoices i
    WHERE i.client_id = client_id;
END$$

DELIMITER ;

CALL get_invoices_by_client(1)
```

#### 6. Parameters with Default Value

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
	state CHAR(2)
)
BEGIN
	IF state IS NULL THEN
		SET state = 'CA';
	END IF;
	SELECT * FROM clients c
    WHERE c.state = state;
END$$

DELIMITER ;

CALL get_clients_by_state(NULL)
```

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state
(
	state CHAR(2)
)
BEGIN
	SELECT * FROM clients c
	WHERE c.state = IFNULL(state, c.state);
END$$

DELIMITER ;

CALL get_clients_by_state(NULL)
```

##### Exercise
Write a stored procedure called get_payments with two parameters, client_id => INT(4 bytes), payment_method_id => TINYINT(1 byte)

```sql
DELIMITER $$
CREATE PROCEDURE get_payments
(
	client_id INT,
    payment_method_id TINYINT
)
BEGIN
	SELECT * 
    FROM payments p
	WHERE
		p.client_id = IFNULL(client_id, p.client_id) AND
        p.payment_method = IFNULL(payment_method_id, p.payment_method);
END$$

DELIMITER ;

CALL get_payments(5, 2)
```
 
#### 7. Parameter Validiation

```sql
DELIMITER $$
CREATE PROCEDURE make_payments
(
	invoice_id INT,
    payment_amount DECIMAL(9, 2),
    payment_date DATE
)
BEGIN
	IF payment_amount <= 0 THEN
		SIGNAL SQLSTATE '22003'
			SET MESSAGE_TEXT = 'Invalid payment amount';
	END IF;
	UPDATE invoices i
    SET
		i.payment_total = payment_amount,
        i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
END$$

DELIMITER ;

CALL make_payments(2, -100, '2019-01-01')
```

#### 8. Output Parameters

```sql
DELIMITER $$
CREATE PROCEDURE get_unpaid_invoices_for_client
(
	client_id INT,
    OUT invoices_count INT,
    OUT invoices_total DECIMAL(9, 2)
)
BEGIN
	SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id
		AND payment_total = 0;
END$$

DELIMITER ;
```

```sql
SET @invoices_count = 0;
SET @invoices_total = 0;
CALL sql_invoicing.get_unpaid_invoices_for_client(5, @invoices_count, @invoices_total);
SELECT @invoices_count, @invoices_total;
```

#### 9. Variables

- User or session variables

```sql
SET @invoices_count = 0
```

- Local variables

```sql
DELIMITER $$
CREATE PROCEDURE get_risk_factor()
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices;
    
    SET risk_factor = invoices_total / invoices_count * 5;
    
    SELECT risk_factor;
END$$

DELIMITER ;

CALL sql_invoicing.get_risk_factor();
```

#### 10. Functions

```sql
DELIMITER $$
CREATE FUNCTION `get_risk_factor_for_client`(
	client_id INT
) RETURNS int
    READS SQL DATA
BEGIN
	DECLARE risk_factor DECIMAL(9, 2) DEFAULT 0;
    DECLARE invoices_total DECIMAL(9, 2);
    DECLARE invoices_count INT;
    
    SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id;
    
    SET risk_factor = invoices_total / invoices_count * 5;
    
    RETURN IFNULL(risk_factor, 0);
END$$

DELIMITER ;
```

```sql
SELECT
	client_id,
    name,
    get_risk_factor_for_client(client_id) AS risk_factor
FROM clients
```sql
 
 ```sql
 DROP FUNCTION IF EXISTS get_risk_factor_for_client;
 ```
