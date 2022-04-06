
#### 1. String Types

- CHAR(x) -> fixed-length
- VARCHAR(x) -> max : 65,535 chracters (~64KB)
  - VARCHAR(50) : for shor strings
  - VARCHAR(255) : for medium-length stirngs
- MEDIUMTEXT -> max : 16MB
- TINYTEXT -> max : 255bytes
- TEXT -> max : 64KB

BYTES


| 1byte       | 2bytes           | 3bytes  |
| ------------- |:-------------:| -----:|
| English      | European/Middle-easter | Asian |

#### 2. Integer Types

- TINYINT -> 1byte [-128, 127]
- UNSIGNED TINYINT -> 1byte [0, 255]
- SMALLINT -> 2bytes [-32K, 32K]
- MEDIUMINT -> 3bytes [-8M, 8M]
- INT -> 4bytes [-2B, 2B]
- BIGINT -> 8bytes [-9Z, 9Z]

ZEROFILL
- INT(4) -> 0001

#### 3. Fixed-point and Floating-point Types

- DECIMAL(p, s)
  - DECIMAL(9, 2) -> 1234567.89
- DEC
- NUMERIC
- FIXED
- FLOAT -> 4bytes
- DOUBLE -> 8bytes

#### 4. Boolean Types

- BOOL
- BOOLEAN -> TURE or FALSE

#### 5. Enum and Set Types

- ENUM('small', 'medium', 'large')
- SET(...)

#### 6. Date and Time Types

- DATE
- TIME
- DATETIME -> 8bytes
- TIMESTAMP -> 4bytes (up to 2038)
- YEAR

#### 7. Blob Types

- TINYBLOB -> 255bytes
- BLOB -> 64KB
- MEDIUMBLOB -> 16MB
- LONGBLOB -> 4GB

#### 8. JSON Types

```sql
UPDATE products
SET properties = '
{
  "dimenstions" : [1, 2, 3],
  "weight" : 10,
  "manufacturer" : {"name" : "sony"}
}
'
WHERE product_id = 1;
```

```sql
UPDATE products
SET properties = JSON_OBJECT(
  'dimenstions' : JSON_ARRAY(1, 2, 3),
  'weight' : 10,
  'manufacturer' : JSON_OBJECT('name', 'sony')
)
WHERE product_id = 1;
```
```sql
SELECT product_id JSON_EXTRACT(properties, '$.weight') AS weight
FORM product
WHERE product_id = 1;
```
```sql
SELECT product_id, properties -> '$.weight' AS weight
FORM product
WHERE product_id = 1;
```
|product_id|weight|
| ------- | ------- |
| 1 |10  |
```sql
SELECT product_id, properties -> '$.dimensions[0]' AS dimensions
# indexing
FORM product
WHERE product_id = 1;
```
|product_id|dimensions|
| ------- | ------- |
| 1 |1  |

```sql
SELECT product_id, properties ->> '$.manufacturer.name' AS manufacturer
FORM product
WHERE product_id = 1;
```
|product_id|manufacturer|
| ------- | ------- |
| 1 |sony  |

```sql
UPDATE products
SET properties = JSON_SET(
  properties,
  '$.weight', 20, #update
  '$.age', 10 #add
)
WHERE product_id = 1;
```

```sql
UPDATE products
SET properties = JSON_REMOVE(
  properties,
  '$.age' #remove
)
WHERE product_id = 1;
```
