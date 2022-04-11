#### 1. Creating a User

```sql
CREATE USER john@127.0.0.1
CREATE USER john@localhoust
CREATE USER john@'%.nils.com'
CREATE USER john IDENTIFIED BY '1234'
```

#### 2. Viewing Users

```sql
SELECT * FROM mysql.user;
```

#### 3. Dropping Users

```sql
CREATE USER bob@nils.com IDENTIFIED BY '1234'
DROP USER bob@nils.com;
```

#### 4. Changing Passwords

```sql
SET PASSWORD FOR john = '1234';
```

#### 5. Granting Privileges

1. web/desktop application

```sql
CREATE USER moon_app IDENTIFIED BY '1234';

GRANT SELECT, INSERT, UPDATE, DELETE, EXECUTE
ON sql_store.customers.*
TO moon_app;
```

2. admin

```sql
GRANT ALL
ON *.* -- all tables in all databases
TO john;
```

#### 6. Viewing Privileges

```sql
SHOW GRANT FOR john;
```

#### 7. Revoking Privileges

```sql
GRANT CREATE VIEW
ON sql_store.*
TO moon_app;
```

```sql
REVOKE CREATE VIEW
ON sql_store.*
FROM moon_app;
```
