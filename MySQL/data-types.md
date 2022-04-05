
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
