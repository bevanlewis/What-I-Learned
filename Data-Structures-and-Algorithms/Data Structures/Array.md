# Array

An array is a data structure that stores elements of the same type in a contiguous block of memory.
In an array we can access the elements by their index.
Arrays can be one-dimensional or multi-dimensional.

Example:

```python
# One-dimensional array
arr = [1, 2, 3, 4, 5]
# Multi-dimensional array
arr = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

## Array Operations and Complexity

1. Lookup by Index: `O(1)`

In python we use `array[index]` to lookup a value in the array.

If the array is full, we need to loop through the entire array to lookup a value.

2. Search for a value: `O(n)`

In python we use `value in array` to search for a value in the array.

If the array is full, we need to loop through the entire array to search for the value.

3. Append (add to end): `O(1)`

In python we use `array.append(value)` to append a value to the end of the array.

If the array is full, we need to create a new array with a larger size and copy all the elements over.

4. Insert into a specific location: `O(n)`

In python we use `array.insert(index, value)` to insert a value into a specific location.

If the array is full, we need to create a new array with a larger size and copy all the elements over.

5. Delete a value: `O(n)`

In Python, we use `array.remove(value)` to delete a value.

If the array is implemented with a fixed size and is full, removing an element will free up space, but if we need to maintain a specific size, we might need to create a new array with a smaller size and copy all the elements over.

6. Traversal: `O(n)`

In python we use `for value in array` to traverse the array.

If the array is full, we need to loop through the entire array.
