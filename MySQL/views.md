### Views

#### 1. Creating Views

```sql
CREATE VIEW sales_by_client AS
SELECT
    c.client_id,
    c.name,
    SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name
```

```sql
SELECT *
FROM sales_by_client
JOIN clients USING (client_id)
```

##### Exercise
Create a view to see the balance for each client.

```sql
CREATE VIEW clients_balance AS
SELECT
    c.client_id,
    c.name,
    SUM(invoice_total - payment_total) AS balance
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name
```

#### 2. Altering or Dropping Views

```sql
DROP VIEW sales_by_client
```

```sql
CREATE OR REPLACE VIEW sales_by_client AS
SELECT
    c.client_id,
    c.name,
    SUM(invoice_total) AS total_sales
FROM clients c
JOIN invoices i USING (client_id)
GROUP BY client_id, name
```

#### 3. Updatable Views

```sql
CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT
    invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0
```

```sql
DELETE FROM invoice_with_balance
WHERE invoice_id = 1
```

```sql
UPDATE FROM invoice_with_balance
SET due_date = DATE_ADD(due_date, INTERVAL 2 DAY)
WHERE invoice_id = 2
```

#### 4. THE WITH OPTION CHECK Clause

```sql
CREATE OR REPLACE VIEW invoices_with_balance AS
SELECT
    invoice_id,
    number,
    client_id,
    invoice_total,
    payment_total,
    invoice_total - payment_total AS balance,
    invoice_date,
    due_date,
    payment_date
FROM invoices
WHERE (invoice_total - payment_total) > 0
WITH CHECK OPTION
# prevent disappear
```

```sql
UPDATE FROM invoice_with_balance
SET payment_total =  invoice_total
WHERE invoice_id = 3
# error
```

#### 5. Other Benefits of Views
- Simplify queries
- Reduce the impact of changes
- Restrict access to the data
