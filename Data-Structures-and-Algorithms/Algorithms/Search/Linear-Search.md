# Linear Search

Linear search is a simple search algorithm that checks every element in a list sequentially until the desired element is found or the list ends. It is also known as a sequential search. List does not need to be sorted prior to search.

## How Linear Search Works

1. Start from the first element of the list.
2. Compare the current element with the target element.
3. If the current element matches the target, return the index of the current element.
4. If the current element does not match the target, move to the next element.
5. Repeat steps 2-4 until the target element is found or the end of the list is reached.

## Time Complexity

- **Best Case:** O(1) - The target element is the first element in the list.
- **Worst Case:** O(n) - The target element is the last element in the list or not present at all.
- **Average Case:** O(n) - The target element is somewhere in the middle of the list.

## Python Implementation

Here is a simple implementation of linear search in Python:

```python
def linear_search(arr, target):
  """
  Perform a linear search for the target in the given list.

  Parameters:
  arr (list): The list to search through.
  target: The element to search for.

  Returns:
  int: The index of the target element if found, otherwise -1.
  """
  for index, element in enumerate(arr):
    if element == target:
      return index
  return -1

# Example usage
arr = [10, 23, 45, 70, 11, 15]
target = 70

result = linear_search(arr, target)

if result != -1:
  print(f"Element found at index {result}")
else:
  print("Element not found in the list")
```

In this example, the `linear_search` function iterates through the list `arr` and compares each element with the `target`. If the target is found, it returns the index of the target element. If the target is not found, it returns -1.
