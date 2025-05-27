# Queue

A queue is a data structure that stores elements in a linear order, following the First-In-First-Out (FIFO) principle.
In a queue, elements are added at the rear (enqueue) and removed from the front (dequeue).

Example:

```python
# Create a queue class
class Queue:
  def __init__(self):
    self.queue = []

  def enqueue(self, value):
    self.queue.append(value)  # Add to rear

  def dequeue(self):
    if not self.is_empty():
      return self.queue.pop(0)  # Remove from front
    return None

  def peek(self):
    if not self.is_empty():
      return self.queue[0]  # View front element
    return None

  def is_empty(self):
    return len(self.queue) == 0  # Check if empty

  def traverse(self):
    for value in self.queue:
      print(value, end=' ')
    print()

# Example usage
queue = Queue()

# Enqueue elements
queue.enqueue(1)  # Add 1 to the rear of the queue
queue.enqueue(2)  # Add 2 to the rear of the queue
queue.enqueue(3)  # Add 3 to the rear of the queue

# Queue now looks like: [1, 2, 3]

# Dequeue elements
queue.dequeue()  # Remove and return the front element (1)
# Queue now looks like: [2, 3]
queue.dequeue()  # Remove and return the front element (2)
# Queue now looks like: [3]
```

1. Enqueue (add to rear): `O(1)`

In Python, we use `queue.append(value)` to add a value to the rear of the queue.

2. Dequeue (remove from front): `O(1)`

In Python, we use `queue.popleft()` to remove a value from the front of the queue.

3. Peek (view front element): `O(1)`

In Python, we use `queue[0]` to view the front element of the queue without removing it.

4. Check if empty: `O(1)`

In Python, we use `len(queue) == 0` to check if the queue is empty.

5. Traversal: `O(n)`

In Python, we use `for value in queue` to traverse the queue.

Queues are commonly used in scenarios where order needs to be preserved, such as task scheduling, breadth-first search in graphs, and handling requests in web servers.
