## Creating a Database and Tables

```sql

CREATE DATABASE database_name;
USE database_name;

CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);

```

## Inserting Data

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

```

## Querying Data

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;

```

## Updating Data

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

```