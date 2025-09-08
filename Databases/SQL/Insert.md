# Insert

## INSERT statements

Insert a single row into a table:

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

Insert multiple rows into a table:

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES
  (value1, value2, value3),
  (value4, value5, value6),
  (value7, value8, value9);
```

Insert data into all columns (without specifying column names):

```sql
INSERT INTO table_name
VALUES (value1, value2, value3);
```

## Examples

```sql
-- Insert a single user
INSERT INTO users (name, email, age)
VALUES ('John Doe', 'john@example.com', 25);

-- Insert multiple products
INSERT INTO products (name, price, category)
VALUES
  ('Laptop', 999.99, 'Electronics'),
  ('Book', 19.99, 'Education'),
  ('Headphones', 79.99, 'Electronics');
```
