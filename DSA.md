## Big O

- We use Big O to describe the performance of an algorithm o(n)

```python
# o(1) - Constent
from array import array


def display(arr, names):
    # o(2) --> o(1)
    print(arr[0])  # o(1)
    print(arr[0])  # o(1)


display([1, 2, 3])
```

```python
# o(n) - Linear
from array import array


def display(arr, names):
    # o(2 + n + m) --> o(n + m) --> o(n) -- n > input
    print(arr[0])  # o(1)
    print(arr[0])  # o(1)

    for n in arr:  # o(n)
        print(n)

    for name in names:  # o(m)
        print(name)


display([1, 2, 3], ['john', 'jane', 'bob'])
```

```python
# o(n ^ 2) - Quadratic
from array import array


def display(arr, names):
    # o(n + n ^ 2) --> o(n ^ 2)
    for n in arr:
        print(n)

    for n in arr:  # o(n)
        for name in names:  # o(m)
            print(n, name)


display([1, 2, 3], ['john', 'bob', 'jane'])
```

- O(log n) logarithmic is Scaleable

  ![logarithmic](./assets/python/img/logn.png 'logarithmic')

- Example: Binary Search

- O(2 ^ n) Exponential

  ![exponential](./assets/python/img/exponential.png 'exponential')

- Exponential is Opposite of o(log n) and it is not Scaleable at all

- Big O Curves

  ![Big O curves](./assets/python/img/bigOCurves.png 'Big O curves')

## Space Complexity

```python
from array import array


def display(names):
    # o(1) Space
    # x -> for every name, x will contain only index
    for x in range(len(names)):  # o(m)
        print(f'hi, {names[x]}')

    # o(n) Space
    # names could be n
    names_copy = names
    print(names_copy)


display(["john", "bob", "jane"])
```

## Array

![big o of array](./assets/python/img/arrays.png 'big o of array')

- LookUp - O(1)
- Insert - O(n)
- Delete - O(n)

```python
from array import array


class ArrayClone:
    def __init__(self):
        self.res = array("i")
        self.length = len(self.res)

    def add(self, num):
        self.res.append(num)

    def removeAt(self, num):
        self.res.pop(num)

    def indexOf(self, num):
        return self.res[num]


arr = ArrayClone()

(arr.add(2))
(arr.add(4))

print(arr.res)
arr.removeAt(0)

print(arr.indexOf(0))
```

## Linked List

- LookUp

  - By Value - O(n)
  - By Index - O(n)

- Insert

  - At the End - O(1)
  - At the Beginning - O(1)
  - Middle - O(n)

- Delete
  - From the End - O(n)
  - From the Beginning - O(1)
  - From the Middle - O(n)

```python
class NoSuchElementError(Exception):
    pass


class LinkedList:
    class __Node:
        def __init__(self, value=None, nextNode=None):
            self.__value = value
            self.__next = nextNode

        @property
        def value(self):
            return self.__value

        @value.setter
        def value(self, value):
            if value is None:
                self.__value = value
            else:
                raise ValueError("Not a None type")

        @property
        def next(self):
            return self.__next

        @next.setter
        def next(self, nextNode):
            if __name__ == "linkedList":
                self.__next = nextNode

    def __init__(self):
        self.__head = None
        self.__tail = None
        self.__size = 0

    def toArray(self):
        current = self.__head
        nodes = []
        while current:
            nodes.append(current.value)
            current = current.next
        return nodes

    def addFirst(self, value=None):
        node = self.__Node(value)
        if self.__head:
            node.next = self.__head
            self.__head = node
        else:
            self.__head = self.__tail = node
        self.__size += 1

    def addLast(self, item=None):
        node = self.__Node(item)
        if self.__head:
            self.__tail.next = self.__tail = node
        else:
            self.__head = self.__tail = node
        self.__size += 1

    def removeFirst(self):
        if not self.__head:
            raise NoSuchElementError("Empty List")

        if self.__head == self.__tail:
            self.__head = self.__tail = None
        else:
            newHead = self.__head.next
            self.__head.next = None
            self.__head = newHead
        self.__size -= 1

    def removeLast(self):
        if not self.__head:
            raise NoSuchElementError("Empty List")

        if self.__head == self.__tail:
            self.__head = self.__tail = None
        else:
            current = self.__head.next
            while current:
                if current.next == self.__tail:
                    self.__tail = current
                    self.__tail.next = None
                current = current.next

        self.__size -= 1

    def contains(self, value):
        return self.indexOf(value) != -1

    def indexOf(self, value):
        index = 0
        current = self.__head
        while current:
            if value == current.value:
                return index
            index += 1
            current = current.next
        return -1

    def size(self):
        return self.__size

    def reverse(self):
        if not self.__head:
            return

        previous = self.__head
        current = previous.next
        while current:
            nxt = current.next
            current.next = previous
            previous = current
            current = nxt

        self.__head.next = None
        self.__head, self.__tail = self.__tail, self.__head

    def getKthFromTheEnd(self, k):
        if not self.__head:
            raise ValueError("Empty List")

        a = b = self.__head
        for _ in range(k - 1):
            b = b.next
            if not b:
                raise ValueError("Out Of Range")
        while b != self.__tail:
            a = a.next
            b = b.next

        return a.value
```

![Time Complexity of Arrays and  Linked Lists](./assets/python/img/arrayLinkedlist.png 'Time Complexity of Arrays and  Linked Lists')

## Stacks - LIFO

- Implement the undo feature
- Build compilers (eg syntax checking)
- Evaluate expressions (eg 1+2\*3)
- Build Navigation (eg forward / back)

!["Stack"](./assets/python/img/stacks.png 'Stack')

```python
class Stack:
    def __init__(self):
        self.__stack = []
        self.__size = 0

    def push(self, value):
        self.__stack.append(value)
        self.__size += 1

    def pop(self):
        removed = self.__stack.pop()
        self.__size -= 1
        return removed

    def peek(self):
        return self.__stack[-1]

    def isEmpty(self):
        return self.__size == 0

    def getItems(self):
        return self.__stack
```

## Queues - FIFO

- Application of Queues

  - Printers
  - Operating Systems
  - Web Servers
  - Live Support Systems

- Operations of queue
  - | Operation | Big O |
    | --------- | ----- |
    | enqueue() | O(1)  |
    | dequeue() | O(1)  |
    | peek()    | O(1)  |
    | isEmpty() | O(1)  |
    | isFull()  | O(1)  |

```python
from collections import deque

items = deque([])

items.appendleft(4)
items.append(1)
items.append(2)
items.append(3)
items.popleft()
items.pop()
items.rotate(-2)

print(items.count(2))  # 1
print(items)  # 2, 1
```
