### Triggers and Events

#### 1. Triggers
A block of SQL code that automatically gets executed before or after an insert update or delete statement. 

```sql
DELIMITER $$
 
CREATE TRIGGER payments_after_insert
	AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
END $$

DELIMITER ;
```

```sql
INSERT INTO payments
VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1)
```

##### Exercise
Create a trigger that gets fired when we delete a payment
 
```sql
DELIMITER $$
 
CREATE TRIGGER payments_after_delete
	AFTER DELETE ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total - OLD.amount
    WHERE invoice_id = OLD.invoice_id;
END $$

DELIMITER ;
```

```sql
DELETE
FROM payments
WHERE payment_id = 9
```

#### 2. Viewing Triggers

```sql
SHOW TRIGGERS LIKE 'payments%'
```

#### 3. Dropping Triggers

```sql
DROP TRIGGER IF EXISTS payments_after_insert;
```

#### 4. Using Triggers for Auditing

```sql
DROP TRIGGER IF EXISTS payments_after_insert;

DELIMITER $$
CREATE TRIGGER payments_after_insert
	AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
    
    INSERT INTO payments_audit
    VALUES (NEW.client_id, NEW.date, NEW.amount, 'Insert', NOW());
END $$

DELIMITER ;
```

```sql
DROP TRIGGER IF EXISTS payments_after_delete;

DELIMITER $$
 
CREATE TRIGGER payments_after_delete
	AFTER DELETE ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total - OLD.amount
    WHERE invoice_id = OLD.invoice_id;
	 
    INSERT INTO payments_audit
    VALUES (OLD.client_id, OLD.date, OLD.amount, 'Delete', NOW());
END $$

DELIMITER ;
```

```sql
INSERT INTO payments
VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1)
```

```sql
DELETE FROM payments
WHERE payment_id = 10
```

#### 5. Events
A task or block of SQL code that gets executed according to a schedule

```sql*
DELIMITER $$

CREATE EVENT yearly_delete_stale_audit_rows
ON SCHEDULE
	# AT '2019-05-01'
    EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
    #DATEADD(NOW(), INTERVAL - 1 YEAR)
    # DATESUB(NOW(), INTERVAL 1 YEAR)
END $$

DELIMITER ;
```

```sql
SHOW EVENTS LIKE 'yearly%';

DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;

ALTER EVENT yearly_delete_stale_audit_rows DISABLE;
ALTER EVENT yearly_delete_stale_audit_rows ENABLE;
```
