# Data Manupulation Language

- [INSERT](#insert)
- [UPDATE](#update)
- [DELETE](#delete)
- [RETURNING](#returning)

## INSERT 
```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
``` 

`Example:` 
```sql
INSERT INTO quotes (body,author,client) values ('Firrt quote','harsh','08:00:2b:01:02:03');
```

## UPDATE

```sql
UPDATE table_name SET COLUMN = VALUE, ....
WHERE [CONDITION]
``` 

`Example:` 
```sql
UPDATE quotes SET author='Peter Atkins', body='Life is short' where id=4;
```

## DELETE

```sql
DELETE FROM table_name WHERE [CONDITION];
```

`Example:`
```sql
DELETE FROM quotes where id=2 ;
```


## RETURNING
`Note:` Postgres give us an additonal option to return specific columns after executing the DML queries.

`Example:`

Returns id:
```sql
UPDATE quotes SET author='mark', body='Art of not giving a fu*ck' where id=4 RETURNING id;
```

Returns the row
```sql
DELETE FROM quotes where name='harsh' RETURNING *;
```


