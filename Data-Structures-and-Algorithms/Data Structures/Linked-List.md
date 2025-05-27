# Linked List

Linked lists are a sequence of data elements, called nodes, each pointing to the next node by means of a pointer.

![Linked List](Data-Structures-and-Algorithms/images/linked_list.png "Linked List")

A double liinked list consists of nodes that have two pointers, one to the next node and one to the previous node.

![Double Linked List](Data-Structures-and-Algorithms/images/double_linked_list.png "Double Linked List")

## Why do we need a linked list

Arrays have the following limitations:

- Their size is fixed
- They consume a lot of memory
- Inserting and deleting elements in the middle of an array is expensive

Linked lists overcome these limitations by not storing the elements contiguously in memory.

Instead, each element in a linked list is linked to the next element by means of a pointer.

This allows for efficient insertion or removal of elements from any position in the list, provided only the node to be removed is known.

## Array Operations and Complexity

1. Insert/Delete element at begginning: `O(1)`
2. Insert/Delete element at end: `O(n)`
3. Traversal: `O(n)`
4. Accessing element by value (search): `O(n)`

## Python implementation of a linked list

```python
class Node:
  def __init__(self, data=None, next=None):
    self.data = data
    self.next = None

class LinkedList:
  def __init__(self):
    self.head = None

  def insert_at_beginning(self, data):
      new_node = Node(data)
      new_node.next = self.head
      self.head = new_node

  def insert_at_end(self, data):
      new_node = Node(data)
      if self.head is None:
          self.head = new_node
          return
      last = self.head
      while last.next:
          last = last.next
      last.next = new_node

  def delete_node(self, key):
      temp = self.head
      if temp is not None:
          if temp.data == key:
              self.head = temp.next
              temp = None
              return
      while temp is not None:
          if temp.data == key:
              break
          prev = temp
          temp = temp.next
      if temp == None:
          return
      prev.next = temp.next
      temp = None

  def search(self, key):
      current = self.head
      while current is not None:
          if current.data == key:
              return True
          current = current.next
      return False

  def print_list(self):
      temp = self.head
      while temp:
          print(temp.data, end=" ")
          temp = temp.next
      print()
```
