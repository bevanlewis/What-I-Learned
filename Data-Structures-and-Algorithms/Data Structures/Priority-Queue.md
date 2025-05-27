# Priority Queue

A priority queue is a data structure that stores elements in a way that allows for efficient retrieval of the element with the highest (or lowest) priority. Unlike a regular queue, elements in a priority queue are dequeued based on their priority rather than their order of insertion.

Example:

```python
import heapq

# Create a priority queue class
class PriorityQueue:
  def __init__(self):
    self.queue = []

  def enqueue(self, value):
    heapq.heappush(self.queue, value)  # Add to the priority queue

  def dequeue(self):
    if not self.is_empty():
      return heapq.heappop(self.queue)  # Remove and return the highest priority element
    return None

  def peek(self):
    if not self.is_empty():
      return self.queue[0]  # View the highest priority element
    return None

  def is_empty(self):
    return len(self.queue) == 0  # Check if empty

  def traverse(self):
    for value in self.queue:
      print(value, end=' ')
    print()

# Example usage
priority_queue = PriorityQueue()

# Enqueue elements
priority_queue.enqueue(3)  # Add 3 to the priority queue
priority_queue.enqueue(1)  # Add 1 to the priority queue
priority_queue.enqueue(2)  # Add 2 to the priority queue

# Priority queue now looks like: [1, 3, 2]

# Dequeue elements
priority_queue.dequeue()  # Remove and return the highest priority element (1)
# Priority queue now looks like: [2, 3]
priority_queue.dequeue()  # Remove and return the highest priority element (2)
# Priority queue now looks like: [3]
```

1. Enqueue (add to priority queue): `O(log n)`

In Python, we use `heapq.heappush(queue, value)` to add a value to the priority queue.

2. Dequeue (remove highest priority element): `O(log n)`

In Python, we use `heapq.heappop(queue)` to remove the highest priority value from the priority queue.

3. Peek (view highest priority element): `O(1)`

In Python, we use `queue[0]` to view the highest priority element of the priority queue without removing it.

4. Check if empty: `O(1)`

In Python, we use `len(queue) == 0` to check if the priority queue is empty.

5. Traversal: `O(n)`

In Python, we use `for value in queue` to traverse the priority queue.

Priority queues are commonly used in scenarios where elements need to be processed based on their priority, such as in scheduling algorithms, Dijkstra's shortest path algorithm, and event-driven simulation systems.
