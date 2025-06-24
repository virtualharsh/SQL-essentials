# PostgreSQL Database Operations

`Note :` Given commands in `square brackets [] are optional`;

- [Listing all databases](#listing-all-databases)
- [Creating a database](#creating-a-database)
- [Altering the database](#altering-the-database)
- [Currently working database](#currently-working-database)
- [Change currently working database](#change-currently-working-database)
- [Dropping the database](#dropping-the-database)


## Listing all databases
```sql
\l+
```

Returns following attributes in table form:
- Name of database
- Owner
- Encoding
- Collate
- Ctype
- Size of DB
- Tabl (tablespace name)

## Creating a database
```sql
CREATE DATABASE db_name;
    [OWNER =  role_name]
    [TEMPLATE = template]
    [ENCODING = encoding]
    [LC_COLLATE = collate]
    [LC_CTYPE = ctype]
    [TABLESPACE = tablespace_name]
    [CONNECTION LIMIT = max_concurrent_connection]
```

---
| Parameter                  | Description                                                                                             |
| :------------------------- | :------------------------------------------------------------------------------------------------------ |
| **db_name** | The name of the new database to create. It must always be a unique name.                                |
| **role_name** | The role name of the user who will own the new database.                                                |
| **template** | The name of the database template from which the new database is created.                               |
| **encoding** | Specifies the character set encoding for the new database. By default, it's the encoding of the template database. |
| **collate** | Specifies a collation for the new database.                                                             |
| **ctype** | Specifies the character classification for the new database (e.g., digit, lower, upper).                |
| **tablespace_name** | Specifies the tablespace name for the new database.                                                     |
| **max_concurrent_connection** | Specifies the maximum concurrent connections to the new database.                                       |

<br>


**Template**

- You can create your own custom templates from existing databases. This is incredibly useful if you have a standard set of tables, functions, or configurations that you want every new database to start with.
- `For example`, a web application might have a template_webapp database that contains all the basic user authentication tables, session management, etc.

**Collate**

- Collation defines the rules for sorting and comparing character strings within the database.
- For example: When sorting whether to consider 'Apple' and 'apple' same and defines sorting rules for numerical strings like 'File10' and 'File2'
- Accent Sensitivity: 
    - Accent-Sensitive (AS): résumé is not equal to resume.
    - Accent-Insensitive (AI): résumé is equal to resume.

**Tablespace Name** : Allows database administrator to  strategically manage the physical storage of database objects, impacting performance, disk space utilization, and overall system architecture.

For example:
```sql
> CREATE TABLESPACE my_app_tablespace LOCATION '/D:data/tables';

> CREATE DATABASE _____ OWNER _____ TABLESPACE my_app_tablespace;

```

## ALTERING THE DATABASE
```sql
ALTER DATABASE target_database action;
```


`RENAME`:
```sql
ALTER DATABASE target_database RENAME TO new_database;
```

`Change owner of the database`:
```sql
ALTER DATABASE target_database OWNER TO new_owner;
```

`Change tablespace`:
```sql
ALTER DATABASE target_database SET TABLESPACE new_tablespace;
```

## Currently working database
```
\c
```

## Change currently working database
```
\c db_name
```

## Dropping the database
```
DROP DATABASE db_name
```

`Note`: if you encounter error use `\c postgres` and run the DROP command again.