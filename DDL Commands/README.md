# Data Defination Language 

- [Create Table](#create-table)  
  - [Data-types - PostgreSQL](#datatypes)  
    - [Numeric](#numeric)  
    - [Character](#character)  
    - [Data-Time](#data-time)  
    - [Boolean](#boolean)  
    - [Network](#network)  
    - [Other](#other)  
  - [Constraints](#constraints)  
- [List Tables](#list-tables)  
- [Alter Table](#alter-table)
- [Rename Table](#rename-table)  
- [Drop Table](#drop-table)  
- [Truncate Table](#truncate-table)  

## Create Table

```sql
CREATE TABLE table_name (
    column1 data_type [constraints],
    column2 data_type [constraints],
    ...
    ...
);
```

`Example`: 
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    body TEXT NOT NULL UNIQUE,
    author VARCHAR(30) NOT NULL,
	votes BIGINT CHECK(votes>=0),
	client MACADDR NOT NULL
);
```
`SERIAL:` Auto-incrementing integer.

## Datatypes

### Numeric

| Type               | Description                   | Example                  |
| ------------------ | ----------------------------- | ------------------------ |
| `SMALLINT`         | 2 bytes, -32,768 to 32,767    | `age SMALLINT`           |
| `INTEGER`          | 4 bytes, -2B to +2B           | `id INTEGER`             |
| `BIGINT`           | 8 bytes, huge range           | `views BIGINT`           |
| `DECIMAL`          | Exact, user-defined precision | `price DECIMAL(10,2)`    |
| `NUMERIC`          | Same as `DECIMAL`             | `amount NUMERIC(8,3)`    |
| `REAL`             | 4 bytes, float (inexact)      | `temp REAL`              |
| `DOUBLE PRECISION` | 8 bytes, float (inexact)      | `score DOUBLE PRECISION` |
| `SERIAL`           | Auto-increment int (4 bytes)  | `id SERIAL`              |
| `BIGSERIAL`        | Auto-increment int (8 bytes)  | `id BIGSERIAL`           |

### Character 

| Type         | Description                | Example             |
| ------------ | -------------------------- | ------------------- |
| `CHAR(n)`    | Fixed-length               | `code CHAR(5)`      |
| `VARCHAR(n)` | Variable-length, limit `n` | `name VARCHAR(255)` |
| `TEXT`       | Unlimited variable-length  | `description TEXT`  |

### Data-Time

| Type          | Description               | Example                  |
| ------------- | ------------------------- | ------------------------ |
| `DATE`        | Calendar date             | `birth_date DATE`        |
| `TIME`        | Time of day (no timezone) | `start_time TIME`        |
| `TIMESTAMP`   | Date + time (no timezone) | `created_at TIMESTAMP`   |
| `TIMESTAMPTZ` | Date + time with timezone | `login_time TIMESTAMPTZ` |
| `INTERVAL`    | Time span/duration        | `duration INTERVAL`      |

### Boolean

| Type      | Description     | Example             |
| --------- | --------------- | ------------------- |
| `BOOLEAN` | True / False / `NULL` | `is_active BOOLEAN` |

### Network

| Type      | Description           | Example                             |
| --------- | --------------------- | ----------------------------------- |
| `INET`    | IP address            | `ip_address INET`                   |
| `CIDR`    | IP network            | `network CIDR`                      |
| `MACADDR` | MAC address           | `mac MACADDR`                       |

### Other

| Type       | Description                   | Example                 |
| ---------- | ----------------------------- | ----------------------- |
| `JSON`     | Raw JSON                      | `profile JSON`          |
| `JSONB`    | Binary JSON (faster indexing) | `data JSONB`            |
| `ARRAY`    | Array of any type             | `tags TEXT[]`           |
| `BYTEA`    | Binary data (e.g. images)     | `file BYTEA`            |
| `ENUM`     | Custom predefined values      | `status my_status_type` |
| `GEOMETRY` | For spatial data              | `location GEOMETRY`     |


## Constraints


### Column level

| Constraint    | Description                                                   | Example                              |
| ------------- | ------------------------------------------------------------- | ------------------------------------ |
| `NOT NULL`    | Ensures the column cannot have `NULL` values                  | `name VARCHAR(100) NOT NULL`         |
| `UNIQUE`      | Ensures all values in the column are unique                   | `email TEXT UNIQUE`                  |
| `PRIMARY KEY` | Uniquely identifies each row, implies `NOT NULL` and `UNIQUE` | `id SERIAL PRIMARY KEY`              |
| `DEFAULT`     | Assigns a default value if none is provided                   | `created_at TIMESTAMP DEFAULT now()` |
| `CHECK`       | Ensures the value meets a specific condition                  | `age INT CHECK (age >= 18)`          |
| `REFERENCES`  | Defines a foreign key reference to another table              | `user_id INT REFERENCES users(id)`   |

### Table level

| Constraint          | Description                                    | Example                                      |
| ------------------- | ---------------------------------------------- | -------------------------------------------- |
| `PRIMARY KEY (...)` | Sets a primary key across multiple columns     | `PRIMARY KEY (user_id, role_id)`             |
| `UNIQUE (...)`      | Enforces uniqueness across a group of columns  | `UNIQUE (first_name, last_name)`             |
| `CHECK (...)`       | Applies a check condition at the table level   | `CHECK (start_date < end_date)`              |
| `FOREIGN KEY (...)` | Declares a foreign key for one or more columns | `FOREIGN KEY (user_id) REFERENCES users(id)` |


`Example:`
```
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE NOT NULL,
    grade CHAR(2),
    
    PRIMARY KEY (student_id, course_id),
    UNIQUE (student_id, course_id, enrollment_date),
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE RESTRICT,
    CHECK (grade IN ('A', 'B', 'C', 'D', 'F', 'P', 'NP'))
);

```

### References options

| Option                | Description                                   | Example                                                        |
| --------------------- | --------------------------------------------- | -------------------------------------------------------------- |
| `ON DELETE CASCADE`   | Delete child rows when parent row is deleted  | `FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE` |
| `ON UPDATE CASCADE`   | Update child values if parent key is updated  | `ON UPDATE CASCADE`                                            |
| `ON DELETE SET NULL`  | Set child column to `NULL` on parent deletion | `ON DELETE SET NULL`                                           |
| `ON DELETE RESTRICT`  | Prevent delete if child rows exist            | `ON DELETE RESTRICT`                                           |

## List Tables

CMD
```bash

> \dt
```

QUERY TOOL
```sql
SELECT table_name FROM information_schema.tables WHERE table_schema = 'public' AND table_type = 'BASE TABLE';
```

## Alter Table

| Operation            | Syntax                                                                  | Example                                                         |
| -------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Add column**       | `ALTER TABLE table_name ADD COLUMN column_name data_type;`              | `ALTER TABLE users ADD COLUMN age INT;`                         |
| **Drop column**      | `ALTER TABLE table_name DROP COLUMN column_name;`                       | `ALTER TABLE users DROP COLUMN age;`                            |
| **Rename column**    | `ALTER TABLE table_name RENAME COLUMN old_name TO new_name;`            | `ALTER TABLE users RENAME COLUMN fullname TO name;`             |
| **Change data type** | `ALTER TABLE table_name ALTER COLUMN column_name TYPE new_data_type;`   | `ALTER TABLE users ALTER COLUMN age TYPE SMALLINT;`             |
| **Set default**      | `ALTER TABLE table_name ALTER COLUMN column_name SET DEFAULT value;`    | `ALTER TABLE users ALTER COLUMN status SET DEFAULT 'active';`   |
| **Drop default**     | `ALTER TABLE table_name ALTER COLUMN column_name DROP DEFAULT;`         | `ALTER TABLE users ALTER COLUMN status DROP DEFAULT;`           |
| **Set NOT NULL**     | `ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;`         | `ALTER TABLE users ALTER COLUMN email SET NOT NULL;`            |
| **Drop NOT NULL**    | `ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL;`        | `ALTER TABLE users ALTER COLUMN email DROP NOT NULL;`           |
| **Add constraint**   | `ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_def;` | `ALTER TABLE users ADD CONSTRAINT email_unique UNIQUE (email);` |
| **Drop constraint**  | `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`               | `ALTER TABLE users DROP CONSTRAINT email_unique;`               |

## Rename Table

```sql
ALTER TABLE old_table_name RENAME TO new_table_name;
```

## Drop Table

```sql
DROP TABLE [IF EXISTS] table_name;
```
`Note:` Deletes Schema along with data.

## Truncate Table
```sql
TRUNCATE TABLE table_name [CASCADE];
```
`Note:` Truncates drops the table and build it again along with constraints preventing it's schema.