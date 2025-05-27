# Binary Search

## Introduction

Binary Search is an efficient algorithm for finding an item from a sorted list of items. It works by repeatedly dividing in half the portion of the list that could contain the item, until you've narrowed down the possible locations to just one.

## How it works

1. Start with the middle element of the sorted array.
2. If the target value is equal to the middle element, the search is complete.
3. If the target value is less than the middle element, repeat the search on the left half of the array.
4. If the target value is greater than the middle element, repeat the search on the right half of the array.
5. Continue this process until the target value is found or the subarray size becomes zero.

## Complexity

- **Time Complexity:** O(log n)
- **Space Complexity:** O(1)

## Example

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1

# Example usage:
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
target = 4
result = binary_search(arr, target)
print(f"Element found at index: {result}")
```

## Applications

- Finding an element in a sorted array
- Search operations in databases
- Debugging (finding the cause of a bug in a sorted list of commits)

## Advantages

- Much faster than linear search for large datasets
- Simple and easy to implement

## Disadvantages

- Requires the array to be sorted
- Not suitable for linked lists due to their dynamic nature
