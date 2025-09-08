# Query

## SELECT statements

Select all columns from a table:

```sql
SELECT * FROM table_name;
```

Select specific columns from a table:

```sql
SELECT column1, column2 FROM table_name;
```

Select with column aliases:

```sql
SELECT column1 AS alias1, column2 AS alias2 FROM table_name;
```

## WHERE clause

Filter results with conditions:

```sql
SELECT * FROM table_name
WHERE condition;
```

## Comparison operators

| Operator     | Description              | Example                                                      |
| ------------ | ------------------------ | ------------------------------------------------------------ |
| `=`          | Equal to                 | `SELECT * FROM users WHERE age = 25`                         |
| `!=` or `<>` | Not equal to             | `SELECT * FROM users WHERE age != 25`                        |
| `>`          | Greater than             | `SELECT * FROM users WHERE age > 18`                         |
| `<`          | Less than                | `SELECT * FROM users WHERE age < 65`                         |
| `>=`         | Greater than or equal to | `SELECT * FROM users WHERE age >= 18`                        |
| `<=`         | Less than or equal to    | `SELECT * FROM users WHERE age <= 65`                        |
| `BETWEEN`    | Between two values       | `SELECT * FROM users WHERE age BETWEEN 18 AND 65`            |
| `IN`         | In a list of values      | `SELECT * FROM users WHERE city IN ('NYC', 'LA', 'Chicago')` |
| `LIKE`       | Pattern matching         | `SELECT * FROM users WHERE name LIKE 'J%'`                   |
| `IS NULL`    | Is null                  | `SELECT * FROM users WHERE email IS NULL`                    |

## Logical operators

| Operator | Description                  | Example                           |
| -------- | ---------------------------- | --------------------------------- |
| `AND`    | Both conditions must be true | `WHERE age > 18 AND city = 'NYC'` |
| `OR`     | Either condition can be true | `WHERE age > 18 OR city = 'NYC'`  |
| `NOT`    | Negates the condition        | `WHERE NOT city = 'NYC'`          |

## ORDER BY

Sort results:

```sql
SELECT * FROM users
ORDER BY age ASC;  -- Ascending order

SELECT * FROM users
ORDER BY age DESC; -- Descending order

SELECT * FROM users
ORDER BY age DESC, name ASC; -- Multiple columns
```

## LIMIT

Limit the number of results:

```sql
SELECT * FROM users
LIMIT 10;  -- First 10 rows

SELECT * FROM users
ORDER BY age DESC
LIMIT 5;  -- Top 5 oldest users
```

## JOINS

### INNER JOIN

Returns records that have matching values in both tables:

```sql
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

### LEFT JOIN (LEFT OUTER JOIN)

Returns all records from the left table and matching records from the right table:

```sql
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

### RIGHT JOIN (RIGHT OUTER JOIN)

Returns all records from the right table and matching records from the left table:

```sql
SELECT users.name, orders.amount
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

### FULL JOIN (FULL OUTER JOIN)

Returns all records when there is a match in either left or right table:

```sql
SELECT users.name, orders.amount
FROM users
FULL JOIN orders ON users.id = orders.user_id;
```

### CROSS JOIN

Returns the Cartesian product of both tables:

```sql
SELECT users.name, products.name
FROM users
CROSS JOIN products;
```

### SELF JOIN

Join a table to itself:

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.id;
```

## Aggregate Functions

| Function  | Description   | Example                             |
| --------- | ------------- | ----------------------------------- |
| `COUNT()` | Count rows    | `SELECT COUNT(*) FROM users`        |
| `SUM()`   | Sum values    | `SELECT SUM(salary) FROM employees` |
| `AVG()`   | Average value | `SELECT AVG(price) FROM products`   |
| `MIN()`   | Minimum value | `SELECT MIN(price) FROM products`   |
| `MAX()`   | Maximum value | `SELECT MAX(price) FROM products`   |

## GROUP BY

Group results by one or more columns:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

## HAVING

Filter grouped results:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

## Examples

```sql
-- Find all users older than 21
SELECT * FROM users WHERE age > 21;

-- Get user names and emails sorted by name
SELECT name, email FROM users ORDER BY name;

-- Find products in the 'Electronics' category with price between 100 and 500
SELECT * FROM products
WHERE category = 'Electronics'
AND price BETWEEN 100 AND 500;

-- Get order details with customer names using JOIN
SELECT orders.id, users.name, orders.amount, orders.date
FROM orders
INNER JOIN users ON orders.user_id = users.id;

-- Count users by city
SELECT city, COUNT(*) as user_count
FROM users
GROUP BY city
ORDER BY user_count DESC;

-- Find users who have placed orders (using LEFT JOIN)
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id
WHERE orders.id IS NOT NULL;
```
