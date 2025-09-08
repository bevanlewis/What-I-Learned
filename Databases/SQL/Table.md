# Tables

## CREATE TABLE

Create a new table with specified columns and constraints:

```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
    [table_constraints]
);
```

## Common Data Types

| Data Type      | Description                    | Example                 |
| -------------- | ------------------------------ | ----------------------- |
| `INT`          | Integer numbers                | `age INT`               |
| `BIGINT`       | Large integer numbers          | `population BIGINT`     |
| `DECIMAL(p,s)` | Decimal numbers with precision | `price DECIMAL(10,2)`   |
| `FLOAT`        | Floating-point numbers         | `temperature FLOAT`     |
| `VARCHAR(n)`   | Variable-length string         | `name VARCHAR(100)`     |
| `CHAR(n)`      | Fixed-length string            | `country_code CHAR(2)`  |
| `TEXT`         | Large text data                | `description TEXT`      |
| `DATE`         | Date (YYYY-MM-DD)              | `birth_date DATE`       |
| `TIME`         | Time (HH:MM:SS)                | `appointment_time TIME` |
| `DATETIME`     | Date and time                  | `created_at DATETIME`   |
| `TIMESTAMP`    | Timestamp                      | `updated_at TIMESTAMP`  |
| `BOOLEAN`      | True/false values              | `is_active BOOLEAN`     |

## Column Constraints

| Constraint       | Description              | Example                                        |
| ---------------- | ------------------------ | ---------------------------------------------- |
| `NOT NULL`       | Cannot be null           | `email VARCHAR(255) NOT NULL`                  |
| `UNIQUE`         | Values must be unique    | `username VARCHAR(50) UNIQUE`                  |
| `PRIMARY KEY`    | Unique identifier        | `id INT PRIMARY KEY`                           |
| `FOREIGN KEY`    | References another table | `user_id INT FOREIGN KEY REFERENCES users(id)` |
| `DEFAULT`        | Default value            | `status VARCHAR(20) DEFAULT 'active'`          |
| `CHECK`          | Custom condition         | `age INT CHECK (age >= 0)`                     |
| `AUTO_INCREMENT` | Auto-incrementing        | `id INT AUTO_INCREMENT`                        |

## Table Constraints

```sql
-- Primary Key constraint
CREATE TABLE users (
    id INT,
    email VARCHAR(255),
    PRIMARY KEY (id)
);

-- Foreign Key constraint
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Unique constraint on multiple columns
CREATE TABLE user_profiles (
    user_id INT,
    profile_type VARCHAR(50),
    UNIQUE (user_id, profile_type)
);
```

## ALTER TABLE

### Add a new column

```sql
ALTER TABLE table_name
ADD column_name datatype [constraints];
```

### Drop a column

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

### Modify a column (change data type or constraints)

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name new_datatype [new_constraints];
```

### Rename a column

```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

### Add constraints

```sql
-- Add NOT NULL constraint
ALTER TABLE table_name
MODIFY COLUMN column_name datatype NOT NULL;

-- Add foreign key
ALTER TABLE table_name
ADD CONSTRAINT fk_name FOREIGN KEY (column_name) REFERENCES other_table(id);

-- Add check constraint
ALTER TABLE table_name
ADD CONSTRAINT ck_name CHECK (column_name > 0);
```

### Drop constraints

```sql
-- Drop constraint by name
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;

-- Drop foreign key
ALTER TABLE table_name
DROP FOREIGN KEY fk_name;

-- Drop primary key
ALTER TABLE table_name
DROP PRIMARY KEY;
```

## DROP TABLE

Remove a table and all its data:

```sql
DROP TABLE table_name;
```

Drop table only if it exists:

```sql
DROP TABLE IF EXISTS table_name;
```

## CREATE INDEX

Create an index to improve query performance:

```sql
-- Single column index
CREATE INDEX idx_name ON table_name (column_name);

-- Composite index
CREATE INDEX idx_name ON table_name (column1, column2);

-- Unique index
CREATE UNIQUE INDEX idx_name ON table_name (column_name);
```

## DROP INDEX

Remove an index:

```sql
DROP INDEX idx_name ON table_name;
```

## Complete Examples

### Create a complete user management system

```sql
-- Create users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    date_of_birth DATE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Create user profiles table
CREATE TABLE user_profiles (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    bio TEXT,
    avatar_url VARCHAR(500),
    location VARCHAR(100),
    website VARCHAR(255),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create posts table
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CHECK (LENGTH(title) > 0)
);

-- Create indexes for better performance
CREATE INDEX idx_users_email ON users (email);
CREATE INDEX idx_users_username ON users (username);
CREATE INDEX idx_posts_user_id ON posts (user_id);
CREATE INDEX idx_posts_created_at ON posts (created_at);
```

### Modify existing tables

```sql
-- Add phone number to users table
ALTER TABLE users
ADD phone VARCHAR(20);

-- Make phone unique
ALTER TABLE users
ADD CONSTRAINT uk_users_phone UNIQUE (phone);

-- Add category to posts
ALTER TABLE posts
ADD category VARCHAR(50) DEFAULT 'general';

-- Change email column to allow longer emails
ALTER TABLE users
MODIFY COLUMN email VARCHAR(320);

-- Remove avatar_url from user_profiles
ALTER TABLE user_profiles
DROP COLUMN avatar_url;

-- Add a check constraint for age
ALTER TABLE users
ADD CONSTRAINT ck_users_age CHECK (date_of_birth <= CURRENT_DATE);
```

## Important Notes

- Always backup your data before making structural changes
- Use `IF EXISTS` and `IF NOT EXISTS` clauses when appropriate
- Consider the impact on existing data when modifying columns
- Foreign key constraints help maintain referential integrity
- Indexes improve read performance but slow down writes
- Use appropriate data types to optimize storage and performance
- Consider naming conventions for constraints and indexes for better maintainability
