# Update

## UPDATE statements

Update specific columns in a table:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

Update all rows in a table (without WHERE clause):

```sql
UPDATE table_name
SET column1 = value1, column2 = value2;
```

## Examples

```sql
-- Update a single user's email
UPDATE users
SET email = 'newemail@example.com'
WHERE id = 1;

-- Update multiple columns
UPDATE users
SET name = 'John Smith', age = 26
WHERE id = 1;

-- Update product price with a percentage increase
UPDATE products
SET price = price * 1.10
WHERE category = 'Electronics';

-- Update multiple users at once
UPDATE users
SET status = 'active'
WHERE last_login > '2023-01-01';

-- Update using a subquery
UPDATE users
SET department_id = (SELECT id FROM departments WHERE name = 'Engineering')
WHERE job_title = 'Developer';
```

## Important Notes

- Always use a WHERE clause to specify which rows to update, unless you intentionally want to update all rows
- Without a WHERE clause, all rows in the table will be updated
- You can update multiple columns in a single UPDATE statement
- You can use subqueries in UPDATE statements
- Mathematical operations can be performed on existing values (e.g., `price = price * 1.10`)
