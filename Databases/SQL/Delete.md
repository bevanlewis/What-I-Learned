# Delete

## DELETE statements

Delete specific rows from a table:

```sql
DELETE FROM table_name
WHERE condition;
```

Delete all rows from a table:

```sql
DELETE FROM table_name;
```

## TRUNCATE TABLE

Remove all rows from a table (faster than DELETE for large tables):

```sql
TRUNCATE TABLE table_name;
```

## DROP TABLE

Remove the entire table and its structure:

```sql
DROP TABLE table_name;
```

## Examples

```sql
-- Delete a specific user
DELETE FROM users
WHERE id = 1;

-- Delete users older than 65
DELETE FROM users
WHERE age > 65;

-- Delete inactive users
DELETE FROM users
WHERE last_login < '2020-01-01';

-- Delete products with low stock
DELETE FROM products
WHERE stock_quantity = 0;

-- Delete using a subquery
DELETE FROM users
WHERE department_id IN (
    SELECT id FROM departments
    WHERE budget < 10000
);
```

## Important Notes

- Always use a WHERE clause to specify which rows to delete, unless you intentionally want to delete all rows
- Without a WHERE clause, all rows in the table will be deleted
- DELETE removes rows but keeps the table structure
- TRUNCATE TABLE is faster than DELETE for removing all rows, but cannot be used with WHERE clause
- DROP TABLE removes the entire table structure and data
- Foreign key constraints may prevent deletion if related data exists in other tables
