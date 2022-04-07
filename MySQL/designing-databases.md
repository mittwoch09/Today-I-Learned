#### 1. Data Modelling
1. Understand the requirements
2. Build a **Conceptual** Model
3. Build a **Logical** Model
4. Build a **Physical** Model

#### 2. Conceputal Models
Represents the entities and their relationships.
- Entity Relationship(ER)
- UML(Unified Modeling Languages)

Modelling Tools -> [diagrams.net](https://app.diagrams.net/)

<img width="811" alt="Screen Shot 2022-04-06 at 11 44 45 AM" src="https://user-images.githubusercontent.com/73784742/161891665-cbf871ce-264a-48ec-a68f-f0ca5e03ab4f.png">

#### 3. Logical Models
`Type`

`Relationships`
- One-to-one
- One-to-many
- Many-to-many

<img width="837" alt="Screen Shot 2022-04-06 at 11 54 54 AM" src="https://user-images.githubusercontent.com/73784742/161892624-2d15ea61-0647-4f60-833f-68e26b7b152f.png">

#### 4. Physical Models

<img width="773" alt="Screen Shot 2022-04-06 at 12 02 49 PM" src="https://user-images.githubusercontent.com/73784742/161893393-d6696d2f-e983-48dc-a899-ceb7cddc695a.png">

#### 5. Primary Keys

<img width="850" alt="Screen Shot 2022-04-06 at 2 10 47 PM" src="https://user-images.githubusercontent.com/73784742/161907059-8fc647f7-6c18-469f-8421-6c27072c87cf.png">

---

`Composite Primary Keys in enrollments table`

<img width="853" alt="Screen Shot 2022-04-06 at 2 22 15 PM" src="https://user-images.githubusercontent.com/73784742/161908762-e7f1243c-6ea8-4020-8bcc-f615fbbed7dc.png">


#### 6. Foreign Keys

`foreign key` is a column in one table that references the primary key of another table.

**stdudents table**

`parent` or `the primar key table`

**enrollments talbe**

`child` or `the foregin key table`

---

<img width="851" alt="Screen Shot 2022-04-06 at 2 24 12 PM" src="https://user-images.githubusercontent.com/73784742/161909040-fd9856b3-337e-409f-a7ea-5114b9e515c1.png">

#### 7. Foreign Key Constraints

A primary key of a table changes, we want to make sure that the foreign key table is updated.

<img width="850" alt="Screen Shot 2022-04-06 at 2 31 13 PM" src="https://user-images.githubusercontent.com/73784742/161909944-538c7997-672b-4a05-9f80-e66fced15b29.png">

#### 8. Normalization

Normalization is the proecess of reviewing our design and ensuring that it follows a few predefined rules that **prevent data duplication.**

#### 9. 1NF-First Normal Form

Each cell should have a **single value** and we cannot have repeated columns.

1. `Link Tables` -> course_tags table

2. `Composite Primary Key` -> course_id, tag_id

3. remove tags column from the courses table

<img width="853" alt="Screen Shot 2022-04-06 at 2 45 51 PM" src="https://user-images.githubusercontent.com/73784742/161912228-318bcd66-8d60-4437-8e87-1df0c5064525.png">
---

`Link Tables` -> enrollments table

many-to-many -> two one-to-many

#### 10. 2NF-Second Normal Form

Every **table** should describe **one entity**, and every column in that table should describe that entity.

The `instructor column` doesn't really belong th this table. If the same instructor teaches multiple courese, their name is going to **duplicated** in this table.

1. create instructors table
2. one-to-many relationship
      - parent -> instructors table
      - child -> courses table 

<img width="851" alt="Screen Shot 2022-04-06 at 3 11 31 PM" src="https://user-images.githubusercontent.com/73784742/161916508-c8d337c8-1362-4f93-8d7c-66bba39f1d85.png">

#### 11. 3NF-Third Normal Form

A column in a table should not be **derived** from other columns.

|first_name|last_name|full_name|
| ------- | ------- | ------- |
| SeJun | Jang |Sejun Jang  |

We can always calculate it by combining the first and last names.

#### 12. Creating and Dropping Databases

```sql
CREATE DATABASE IF NOT EXISTS sql_store2;
```

```sql
DROP DATABASE IF EXISTS sql_store2;
```

##### 13. Creating Tables

```sql
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers
(
  customer_id INT PRIMARY KEY AUTO_INCREMENT,
  first_name VARCHAR(50) NOT NULL,
  points INT NOT NULL DEFAULT 0,
  email VARCHAR(50) NOT NULL UNIQUE
);
```

#### 14. Altering Tables

```sql
ALTER TABLE customers
  ADD last_name VARCHAR(50) NOT NULL AFTER first_name
  ADD city VARCHAR(50) NOT NULL,
  MODIFY COLUMN first_name VARCHAR(55) DEFALUT ''
  DROP points;
```

#### 15. Creating Relationships

```sql
DROP TABLE IF EXISTS orders;
CREATE TABLE orders
(
  order_id INT PRIMARY KEY,
  customer_id INT NOT NULL,
  FOREIGN KEY fk_orders_customers (customer_id) 
    REFERENCE customers (customer_id)
    ON UPDATE CASCADE
    ON DELETE NO ACTION
 );
 ```

#### 16. Altering Primary and Foreign Key Constraints

```sql
ALTER TABLE orders
  ADD PRIMARY KEY (order_id),
  DROP PRIMARY KEY,
  DROP FOREIGN KEY fk_orders_customers,
  ADD FOREIGN KEY fk_orders_customers (customer_id)
    REFERENCE customers (customer_id)
    ON UPDATE CASCADE
    ON DELETE NO ACTION;
```

#### 17. Character Sets and Collations

<img width="851" alt="Screen Shot 2022-04-07 at 4 40 56 PM" src="https://user-images.githubusercontent.com/73784742/162158310-851f4c14-3b99-4176-bc12-928eb728df60.png">

---

```sql
CREATE DATABASE db_name
  CHARACTER SET latin1
```

```sql
DROP TABLE IF EXISTS customers;
CREATE TABLE IF NOT EXISTS customers
(
  customer_id INT PRIMARY KEY AUTO_INCREMENT,
  first_name VARCHAR(50) CHARACTER SET latin1 NOT NULL,
  points INT NOT NULL DEFAULT 0,
  email VARCHAR(50) NOT NULL UNIQUE
);
```

#### 18. Storage Engines

<img width="853" alt="Screen Shot 2022-04-07 at 4 44 37 PM" src="https://user-images.githubusercontent.com/73784742/162159073-7e3b2362-8745-413c-ad29-0b6126e7729f.png">

---

```sql
ALTER TABLE customers
ENGINE = InnoDB
```
