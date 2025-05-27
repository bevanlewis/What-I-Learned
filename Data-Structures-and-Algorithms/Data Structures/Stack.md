# Stack

A stack is a data structure that follows the Last In, First Out (LIFO) principle. This means that the last element added to the stack will be the first one to be removed.

Example:

```python
# Initialize an empty stack
stack = []
# Push elements onto the stack
stack.append(1)  # stack: [1]
stack.append(2)  # stack: [1, 2]
stack.append(3)  # stack: [1, 2, 3]
# Pop elements from the stack
top_element = stack.pop()  # top_element is 3, stack: [1, 2]
```

## Stack Operations and Complexity

1. Push (add to top): `O(1)`

In Python, we use `stack.append(value)` to push a value onto the top of the stack.

2. Pop (remove from top): `O(1)`

In Python, we use `stack.pop()` to remove the top value from the stack.

3. Peek (view top element): `O(1)`

In Python, we use `stack[-1]` to view the top value of the stack without removing it.

4. Check if empty: `O(1)`

In Python, we use `len(stack) == 0` to check if the stack is empty.

5. Size of the stack: `O(1)`

In Python, we use `len(stack)` to get the number of elements in the stack.

6. Traversal: `O(n)`

In Python, we use `for value in stack` to traverse the stack.
