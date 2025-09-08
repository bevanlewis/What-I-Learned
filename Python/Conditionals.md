# Python Conditionals

Conditional statements allow you to execute different blocks of code based on certain conditions. Python uses `if`, `elif`, and `else` keywords for this purpose.

## if Statement

The `if` statement executes a block of code if a condition is `True`:

```python
age = 18

if age >= 18:
    print("You are an adult.")
    print("You can vote.")

print("This always executes.")  # Outside the if block
```

## if...else Statement

The `if...else` statement executes one block if the condition is `True` and another block if it's `False`:

```python
age = 16

if age >= 18:
    print("You are an adult.")
    print("You can vote.")
else:
    print("You are a minor.")
    print("You cannot vote yet.")
```

## if...elif...else Statement

The `if...elif...else` statement allows you to check multiple conditions:

```python
age = 25

if age < 13:
    print("You are a child.")
elif age < 18:
    print("You are a teenager.")
elif age < 65:
    print("You are an adult.")
else:
    print("You are a senior citizen.")
```

## Nested if Statements

You can nest `if` statements inside other `if` statements:

```python
age = 20
has_license = True

if age >= 18:
    if has_license:
        print("You can drive.")
    else:
        print("You need a license to drive.")
else:
    print("You are too young to drive.")
```

## Conditional Expressions (Ternary Operator)

Python's ternary operator allows you to write simple `if...else` statements in one line:

```python
# Traditional if...else
age = 20
if age >= 18:
    status = "adult"
else:
    status = "minor"

# Ternary operator
status = "adult" if age >= 18 else "minor"

print(status)  # "adult"
```

### Multiple conditions with ternary

```python
score = 85
grade = "A" if score >= 90 else "B" if score >= 80 else "C" if score >= 70 else "F"
print(grade)  # "B"
```

## Using Logical Operators in Conditions

```python
age = 25
has_ticket = True
is_vip = False

# AND operator
if age >= 18 and has_ticket:
    print("You can enter the concert.")

# OR operator
if is_vip or (age >= 65):
    print("You get priority seating.")

# NOT operator
if not is_vip:
    print("Regular admission price applies.")
```

## Membership and Identity Operators

```python
fruits = ["apple", "banana", "orange"]

# in operator
if "apple" in fruits:
    print("Apple is available.")

if "grape" not in fruits:
    print("Grape is not available.")

# is operator
x = [1, 2, 3]
y = [1, 2, 3]
z = x

if x is z:
    print("x and z refer to the same object.")

if x is not y:
    print("x and y refer to different objects, even though they have the same values.")
```

## Truthy and Falsy Values in Conditions

```python
# Empty containers are falsy
shopping_list = []

if shopping_list:
    print("You have items to buy.")
else:
    print("Your shopping list is empty.")

# Zero is falsy
balance = 0

if balance:
    print(f"Your balance is ${balance}.")
else:
    print("Your account is empty.")

# None is falsy
user = None

if user:
    print(f"Welcome back, {user}!")
else:
    print("Please log in.")
```

## Conditional Assignment Patterns

### Using `or` for default values

```python
# Traditional way
name = ""
if name:
    display_name = name
else:
    display_name = "Guest"

# Using or operator
display_name = name or "Guest"

print(display_name)  # "Guest"
```

### Using conditional expressions for defaults

```python
age_input = ""
age = int(age_input) if age_input else 0
print(age)  # 0
```

## Guard Clauses

Guard clauses help make code more readable by handling edge cases early:

```python
def process_user(user):
    if not user:
        print("No user provided.")
        return

    if user.get('age', 0) < 18:
        print("User must be 18 or older.")
        return

    # Main processing logic
    print(f"Processing user: {user['name']}")

# Usage
process_user(None)
process_user({'name': 'Alice', 'age': 16})
process_user({'name': 'Bob', 'age': 25})
```

## Conditional List Comprehensions

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter even numbers
even_numbers = [x for x in numbers if x % 2 == 0]
print(even_numbers)  # [2, 4, 6, 8, 10]

# Conditional transformation
result = [x * 2 if x % 2 == 0 else x for x in numbers]
print(result)  # [1, 4, 3, 8, 5, 12, 7, 16, 9, 20]
```

## Best Practices

### 1. Use descriptive variable names

```python
# Good
if user_age >= 18 and user_has_license:
    print("User can drive.")

# Avoid
if a >= 18 and b:
    print("User can drive.")
```

### 2. Keep conditions simple

```python
# Good
is_adult = age >= 18
has_required_documents = has_id and has_proof_of_address

if is_adult and has_required_documents:
    print("Application approved.")

# Avoid complex conditions
if age >= 18 and (has_id and has_proof_of_address) and not is_suspended:
    print("Application approved.")
```

### 3. Use early returns to reduce nesting

```python
# Good
def validate_user(user):
    if not user:
        return False, "User is required"

    if user.get('age', 0) < 18:
        return False, "User must be 18 or older"

    return True, "User is valid"

# Avoid deep nesting
def validate_user_bad(user):
    if user:
        if user.get('age', 0) >= 18:
            return True, "User is valid"
        else:
            return False, "User must be 18 or older"
    else:
        return False, "User is required"
```

### 4. Use `in` for multiple comparisons

```python
# Good
if day in ['saturday', 'sunday']:
    print("It's weekend!")

# Avoid
if day == 'saturday' or day == 'sunday':
    print("It's weekend!")
```

### 5. Handle None values explicitly

```python
# Good
if user is not None and user['status'] == 'active':
    print("Active user")

# Avoid relying on truthiness for None checks
if user and user['status'] == 'active':  # This works but is less explicit
    print("Active user")
```

## Common Patterns

### Checking ranges

```python
score = 85

if 90 <= score <= 100:
    grade = 'A'
elif 80 <= score < 90:
    grade = 'B'
elif 70 <= score < 80:
    grade = 'C'
else:
    grade = 'F'
```

### Multiple conditions with different actions

```python
user_role = 'admin'
user_status = 'active'

if user_role == 'admin':
    if user_status == 'active':
        print("Full admin access")
    else:
        print("Admin access suspended")
elif user_role == 'moderator':
    if user_status == 'active':
        print("Moderator access")
    else:
        print("Moderator access suspended")
else:
    print("Regular user access")
```
