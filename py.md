- [Python Data Structures](#python-data-structures)
  - [List Unpacking](#list-unpacking)
  - [Looping Over Lists](#looping-over-lists)
  - [Adding or Removing elements from a list](#adding-or-removing-elements-from-a-list)
  - [Finding Items](#finding-items)
  - [Sort a List](#sort-a-list)
  - [Lambda or Anonymous Functions](#lambda-or-anonymous-functions)
  - [Maps](#maps)
  - [Filter](#filter)
  - [List Comprehensions](#list-comprehensions)
  - [Zip Functions](#zip-functions)
  - [Stack LIFO (last In - First Out)](#stack-lifo-last-in---first-out)
  - [Queues FIFO (First In - First Out)](#queues-fifo-first-in---first-out)
  - [Tuples](#tuples)
  - [Exercise](#exercise)
  - [Array](#array)
  - [Sets](#sets)
  - [Dictionaries](#dictionaries)
  - [Dictionary Comprehensions](#dictionary-comprehensions)
  - [Comprehensions](#comprehensions)
  - [Generator Expressions](#generator-expressions)
  - [Unpacking Operator](#unpacking-operator)
  - [Exercise](#exercise-1)

- [Exception](#exception)
  - [With Statement](#with-statement)
  - [Raising Exception](#raising-exception)
  - [Cost of Raising Exceptions](#cost-of-raising-exceptions)

- [Classes](#classes)
  - [Creating Classes](#creating-classes)
  - [Constructor](#constructor)
  - [Class vs Instance Attributes](#class-vs-instance-attributes)
  - [Class vs Instance Methods](#class-vs-instance-methods)
  - [Magic Methods](#magic-methods)
  - [Making Custom Containers](#making-custom-containers)
  - [Making Custom Containers with a private member](#making-custom-containers-with-a-private-member)
  - [Property Object](#property-object)
  - [Read Only Property](#read-only-property)
  - [Inheritance](#inheritance)
  - [Method Overriding](#method-overriding)
  - [Avoid Multi-level Inheritance](#avoid-multi-level-inheritance)
  - [Multiple Inheritance](#multiple-inheritance)
  - [Inheritance Example](#inheritance-example)
  - [Abstract Method](#abstract-method)
  - [Polymorphism](#polymorphism)
  - [Extending Built-in Types](#extending-built-in-types)
  - [Data Classes](#data-classes)

- [Modules](#modules)
  - [sys.path](#syspath)
  - [Package's](#packages)
  - [Sub Packages](#sub-packages)
  - [Intra-package References](#intra-package-references)
  - [dir()](#dir)

---

# Python Data Structures

- Lists
- Tuples
- Set
- Dictionaries

## Lists

```python
letters = ['a', 'b', 'c']
matrix = [[0, 0], [0, 1], [1, 0], [1, 1]]

zeros = [0] * 3  # [0, 0, 0]
combained = zeros + letters  # [0, 0, 0, 'a', 'b', 'c']

evenNumbers = list(range(2, 10, 2))  # [2, 4, 6, 8]
splitStr = list('hello world!')  # ['h', 'e', 'l', ...]

print(round(4.5))  # 4
```

```python
letters = ['a', 'b', 'c', 'd']

print(letters[0:3])  # ['a', 'b', 'c']
print(letters[:3])  # ['a', 'b', 'c']
print(letters[0:])  # ['a', 'b', 'c', 'd']
print(letters[:])  # ['a', 'b', 'c', 'd']

print(letters[::-1])  # ['d', 'c', 'b', 'a']
print(list(range(0, 15))[::2])  # [0, 2, 4, ... 14]
```

### List Unpacking

```python
alpha = ['a', 'b', 'c', 'd']

a, *ohter, last = alpha
# a, b = names  ## ValueError too many values to unpack

print(a, ohter, last)  # a ['b', 'c'] d

path = (0, 1)
x, y = path

print(x, y)  # 0 1

```

### Looping Over Lists

```python
alpha = ['a', 'b', 'c', 'd']

for index, char in enumerate(alpha):
    print(index, char)  # index char

```

### Adding or Removing elements from a list

```Python
alpha = ['a', 'b', 'c', 'd']

alpha.append('e')
alpha.insert(1, '2')  # insert 2 at index 1

alpha.pop()  # last element
alpha.pop(1)  # pop 2 element
alpha.remove('b')  # first occurrence of 'b'

del alpha[1:2]  # delete with range
alpha.clear()  # empty list

print(alpha)
```

### Finding Items

```Python
alpha = ['b', 'a', 'a', 'd']

if 'a' in alpha:
    print(alpha.index('a')) # 1 ValueError otherwise

print(alpha.count('a'))  # 2
```

### Sort a List

```python
alpha = ["b", "a", "d", "c"]
numbers = [1, 2, 3]
items = [("item1", 33), ("item2", 29), ("item3", 12)]

alpha.sort()  # modifies the alpha - ascending
numbers.sort()  # modifies the numbers - ascending

alpha.sort(reverse=True)  # modifies the alpha - descending
numbers.sort(reverse=True)  # modifies the numbers - descending

newAlpha = sorted(alpha)  # returns a new list
newAlphaDescending = sorted(alpha, reverse=True)


def sortItems(item):
    return item[1]


items.sort(key=sortItems)

newItemsList = sorted(items, key=sortItems, reverse=True)

print(alpha)
print(numbers)

print(newAlpha)
print(newAlphaDescending)

print(items)
print(newItemsList)
```

### Lambda or Anonymous Functions

```python
items = [("item2", 29), ("item1", 33), ("item3", 12)]

# lambda parameters:expression
items.sort(key=lambda item: item[1], reverse=True)

print(items)
```

### Maps

```python
items = [("item2", 29), ("item1", 33), ("item3", 12)]

prices = map(lambda item: item[1], items)  # returns an iterables memory location

total = sum(list(prices))

print(list(prices), total)  # [] 74
```

### Filter

```python
items = [("item2", 29), ("item1", 33), ("item3", 12)]

filtered = list(filter(lambda item: item[1] >= 15, items))

print(filtered)  # [('item2', 29), ('item1', 33)]
```

### List Comprehensions

```python
items = [("item2", 29), ("item1", 33), ("item3", 12)]

# [expression for item in items condition]

# prices = list(map(lambda item: item[1], items))
prices = [item[1] for item in items]

# filtered = list(filter(lambda item: item[1] >= 15, items))
filtered = [item for item in items if item[1] > 15]

print(filtered, prices)  # [('item2', 29), ('item1', 33)]
```

### Zip Functions

```python
list1 = [1, 2, 3]
list2 = [10, 20, 30]

# list3 = []
#
# for index in range(len(list1)):
#     list3.append((list1[index], list2[index]))

list3 = list(zip("ab", list1, list2))  # [('a', 1, 10), ('b', 2, 20)]

print(list3)
```

### Stack LIFO (last In - First Out)

```python
stack = []

stack.append(1)
stack.append(2)
stack.append(3)

stack.pop()
stack.pop()
stack.pop()

if stack:
    print(stack[-1])

if not stack:
    print("stack is empty")
```

### Queues FIFO (First In - First Out)

```python
from collections import deque

queue = deque([])

queue.append(1)
queue.append(2)
queue.append(3)

queue.popleft()

if queue:
    print(queue[0])
    print(queue)

if not queue:
    print("no queue")
```

### Tuples

```python
point = 1,
point = point * 3
point2 = 1, 2

x, *y = (1, 2, 3)

print(point)  # (1,1,1)
print(type(point2))  # <class 'tuple'>
print(x, y)  # 1 [2, 3]
print(tuple(y))  # (2, 3)

# point2[0] = 3  # tuple object does not support item assignment

if 2 in point2:
    print(point2[0])  # [2]

print(tuple("hello"))  # ('h', 'e', 'l', 'l', 'o')
```

### Exercise

```python
# l initialized only once when declaring a function
def justAList(l=[]):
    # l.append(1)
    l.insert(len(l), 1)
    l.insert(len(l), 2)
    l.insert(len(l), 3)
    return l


print(justAList())  # [1, 2, 3]
print(justAList())  # [1, 2, 3, 1, 2, 3]
print(justAList([4, 5]))  # [4, 5, 1, 2, 3]
```

```python
def justAList(l=None):
    if not l:
        l = []
    l.insert(len(l), 1)
    l.insert(len(l), 2)
    l.insert(len(l), 3)
    return l


print(justAList())
print(justAList())
print(justAList([4, 5]))
```

### Array

```python
from array import array

# https://docs.python.org/3/library/array.html

integers = array("i", [1, 2, 3])

# print(integers.append('4'))  # TypeError 'str' -- "i"
print(integers.append(4))  # None
print(integers)  # [1,2,3,4]
```

### Sets

- unordered collection of unique items

```python
numbers = [1, 2, 3, 4, 3, 2, 1]

first = set(numbers)
first.add(5)  # {1, 2, 3, 4, 5}
secound = {9, 3, 2}

print(first, secound)  # {1, 2, 3, 4, 5} {9, 3, 2}
print(first | secound)  # {1, 2, 3, 4, 5, 9}
print(first & secound)  # {2, 3}
print(first ^ secound)  # {1, 4, 5, 9}
print(first - secound)  # {1, 4, 5}

if 9 in secound:
    print(True)
```

### Dictionaries

```python
person = {"id": 3, "name": "john", "name": "jane"}
# person = dict(id=3, name="jane")

print(person, person["name"])  # {'id': 3, 'name': 'jane'} jane
print("name" in person)  # True
print(person.items())  # [(key: value), ...]

for key in person:
    print(f"{key}: {person[key]}")
```

### Dictionary Comprehensions

```python
myList = [1, 2, 3]

print({x * 2 for x in range(5)})  # set
print({x: x * 2 for x in range(5)})  # dict
```

### Comprehensions

```python
print([x * 2 for x in range(5)])  # list
print({x * 2 for x in range(5)})  # set
print({x: x * 2 for x in range(5)})  # dict
print((x * 2 for x in range(5)))  # tuple --> generator object
```

### Generator Expressions

```python
from sys import getsizeof

temp = [x for x in range(10_000)]

temp2 = (x for x in range(100_000))  # generator expression

print(getsizeof(temp))  # 85176 bytes
print(len(temp))  # 10000

print(getsizeof(temp2))  # 104 bytes
# print(len(temp2))  # TypeError
```

### Unpacking Operator

```python
# *iterable, **dict

values = [*range(5)]

values = [99, *values, *"hello", 99]

prices = {"item1": 44, "item2": 99}

prices = {**prices, "item1": 55}

# prices = {*prices}  # only keys {'item1', 'item2'}

print(values)
print(prices)
```

### Exercise

```python
from functools import reduce

sentence = "This is a common interview question"
charFrequency = {}

for c in sentence:
    if (c != " ") and (c not in charFrequency):
        charFrequency[c] = 1
    elif c != " ":
        charFrequency[c] += 1

char = reduce(
    lambda a, b: a if charFrequency[a] > charFrequency[b] else b, charFrequency
)

print((char, charFrequency[char]))
```

## Exception

```python
try:
    age = int(input("Enter your age: "))
    xfactor = 10 / age
    print("successful")
except (ValueError, ZeroDivisionError) as vz:
    print(vz)
except (ZeroDivisionError) as z:
    print(z)  # code is unreachable
else:
    print("completed")
```

```python
try:
    triangle = open("./codewars/02-triangle.py")
    age = int(input("Enter your age: "))
    xfactor = 10 / age
    print("successful")

except (ValueError, ZeroDivisionError) as vz:
    print(vz)

else:
    print("completed")

finally:
    triangle.close()
    print("file closed")
```

### With Statement

- 'with' can be used when we have obj.**enter**() and obj.**exit**() (context management protocol) magic methods in a object

```python
try:
    with open("./codewars/02-triangle.py") as file:
        print("file opened and will be closed by file.__exit__()")

    age = int(input("Enter your age: "))
    xfactor = 10 / age
    print("successful")

except (ValueError, ZeroDivisionError) as vz:
    print(vz)

else:
    print("completed")
```

### Raising Exception

```python
def cal_x_factor(age):
    try:
        if age < 1:
            raise ValueError("Age can not be 0 or less")
        print("successful")

    except ValueError as v:
        print(v)


cal_x_factor(-1)
```

### Cost of Raising Exceptions

```python
from timeit import timeit

code1 = """
def cal_x_factor(age):
    try:
        if age < 1:
            raise ValueError("Age can not be 0 or less")
        return 10 / age

    except ValueError as v:
        pass


x_factor = cal_x_factor(-1)
"""

code2 = """
def cal_x_factor(age):
    if age < 1:
        return None
    return 10 / age


x_factor = cal_x_factor(-1)
if not x_factor:
    pass
"""

print(timeit(code1, number=10_000))  # 0.01
print(timeit(code2, number=10_000))  # 0.004
```

## Classes

> Class: blueprint for creating new objects
>
> > Class: Human

> Object: Instance of a class
>
> > Object: John, Jane, Bob

### Creating Classes

```python
class Point:
    def draw(self):
        print("draw", self)


point = Point()

print(type(point))  # <class '__main__.Point'

point.draw()
```

### Constructor

```python
class Point:
    # Constructor
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def draw(self):
        return (f"Point: ({self.x}, {self.y})")


point = Point(0, 5)

print(Point.draw(point))  # Point: (0, 5)
print(point.draw())  # Point: (0, 5)
```

### Class vs Instance Attributes

```python
class Point:
    default_color = "yellow"

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def draw(self):
        return f"Point: ({self.x}, {self.y})"


point = Point(0, 5)

print(point.default_color)  # yellow
print(Point.default_color)  # yellow

another_point = Point(0, 10)

print(another_point.default_color)  # yellow

another_point.default_color = "coral"

print(another_point.default_color)  # coral
print(Point.default_color)  # yellow
```

### Class vs Instance Methods

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    default_color = "yellow"

    @classmethod
    def zero(cls):
        return cls(0, 0)  # cls -ref-> Point (Point(0, 0))

    def draw(self):
        return f"Point ({self.x}, {self.y})"


point = Point(1, 2)
point_zero = Point.zero()
piz = point.zero()

print(point.draw())
print(point_zero.draw())
print(piz.draw())
```

### Magic Methods

[A Guide to Python's Magic Methods - Rafe Kettler](https://rszalski.github.io/magicmethods/)

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Point: ({self.x}, {self.y})"

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __lt__(self, other):
        return self.x < other.x and self.y < other.y


point = Point(0, 5)
other = Point(-1, 2)

# print(point)  # <__main__.Point object at 0x7....>
# print(point.__str__())  # <__main__.Point object at 0x7....>

print(point)  # Point: (0, 5)
print(point.__str__())  # Point: (0, 5)

print(point == other)  # False
print(point != other)  # True
print(point > other)  # True
print(point < other)  # False
```

- \_\_str\_\_() magic method will be called by default when we convert anything to string

### Making Custom Containers

```python
class TagCloud:
    def __init__(self):
        self.tags = {}

    def add(self, tag):
        self.tags[tag.lower()] = self.tags.get(tag.lower(), 0) + 1

    def __len__(self):
        return len(self.tags)

    def __getitem__(self, tag):
        return self.tags.get(tag, None)

    def __setitem__(self, key, value):
        self.tags[key.lower()] = value

    def __iter__(self):
        iterObj = iter(self.tags)
        # iterObj = enumerate(self.tags)
        return iterObj


cloud = TagCloud()

cloud.add("python")
cloud.add("Python")
cloud["java"] = 33

print(cloud["python"])  # 2
print(cloud["java"])  # 33

print(len(cloud))  # 2

for x in cloud:  # python java
    print(x)
```

### Making Custom Containers with a private member

```python
class TagCloud:
    def __init__(self):
        self.temp = None
        self.__tags = {}

    def add(self, tag):
        self.__tags[tag.lower()] = self.__tags.get(tag.lower(), 0) + 1

    def __len__(self):
        return len(self.__tags)

    def __getitem__(self, tag):
        return self.__tags.get(tag, None)

    def __setitem__(self, key, value):
        self.__tags[key.lower()] = value

    def __iter__(self):
        iterObj = iter(self.__tags)
        # iterObj = enumerate(self.tags)
        return iterObj


cloud = TagCloud()

cloud.add("python")
cloud.add("Python")
cloud["java"] = 33

print(cloud["python"])  # 2
print(cloud["java"])  # 33

print(len(cloud))  # 2

for x in cloud:
    print(x)

# Private Members
try:
    print(cloud.__tags)
except AttributeError as e:
    print(e)  # AttributeError 'TagCloud' object no attribute '__tags'

# __dict__ holds all attributes of a class
# attribute prefixed with '__AttrName' are renamed to '_ClassName__AttrName'
print(cloud.__dict__)  # {'temp': None, '_TagCloud__tags': {...}}
```

### Property Object

```python
class Price:
    def __init__(self, price):
        self.price = price

    @property
    def price(self):
        return self.__price

    @price.setter
    def price(self, value):
        if value < 0:
            raise ValueError("value can not be a negative number")
        self.__price = value

    # price = property(getprice, setprice) insted use @property, @property.setter


product = Price(2)
product.price = 33

print(product.price)
```

### Read Only Property

```python
class Animal:
    def __init__(self, age):
        self.__age = age

    @property
    def age(self):
        return self.__age

    # @age.setter
    # def age(self, age):
    #     if age < 1:
    #         raise ValueError('age cannot be less then one')
    #     self.__age = age


a = Animal(2)
print(a.age)
```

### Inheritance

```python
# Parent or Base Class
class Animal:
    def __init__(self):
        self.age = 2

    def eat(self):
        return "eat"


# Child or Sub Class
class Mammal(Animal):
    def walk(self):
        return "walk"


class Fish(Animal):
    def swim(self):
        return "swim"


a = Animal()
m = Mammal()
f = Fish()

print(f.eat(), f.age)
print(m.eat(), m.age)

print(isinstance(m, Animal))  # True
print(isinstance(m, Mammal))  # True
print(isinstance(m, Fish))  # False

print(issubclass(Mammal, Animal))  # Ture
print(issubclass(Mammal, object))  # True
```

### Method Overriding

```python
class Animal:
    def __init__(self):
        print("animal constructor")
        self.age = 2

    def eat(self):
        return "eat"


class Mammal(Animal):
    # method overriding
    def __init__(self, weight):
        super().__init__()  # method extending
        print("mammal constructor")
        self.weight = weight

    def walk(self):
        return "walk"


class Fish(Animal):
    def swim(self):
        return "swim"


m = Mammal(7)

print(m.age)
print(m.weight)
```

### Avoid Multi-level Inheritance

- Limit Inheritance to one or two levels only

```python
class Animal:
    def eat(self):
        return "eat"


class Bird(Animal):
    def fly(self):
        return "fly"


class Chicken(Bird):  # Inheritance Abuse
    pass
```

### Multiple Inheritance

```python
class Employee:
    def greet(self):
        return "Employee greet"


class Person:
    def greet(self):
        return "Person greet"


class Manger(Employee, Person):
    pass


m = Manger()

# lookup chain
# 1. self
# 2. Employee
# 3. Person
print(m.greet())  # Employee greet
```

- Good Example for Multiple Inheritance

```python
class flyer:
    def fly(self):
        return "fly"


class swimmer:
    def swim(self):
        return "swim"


class FlyingFish(Flyer, Swimmer):
    pass
```

### Inheritance Example

```python
class InvalidOperationError(Exception):
    pass


class Stream:
    def __init__(self):
        self.open = False

    def open(self):
        if self.open:
            raise InvalidOperationError("Stream is already opened.")
        self.open = True

    def close(self):
        if not self.open:
            raise InvalidOperationError("Stream is already closed.")
        self.open = False


class FileStream(Stream):
    def read(self):
        return "reading data from file..."


class NetworkStream(Stream):
    def read(self):
        return "reading data from network..."
```

### Abstract Method

```python
from abc import ABC, abstractmethod


class InvalidOperationError(Exception):
    pass


class Stream(ABC):
    def __init__(self):
        self.open = False

    def open(self):
        if self.open:
            raise InvalidOperationError("Stream is already opened.")
        self.open = True

    def close(self):
        if not self.open:
            raise InvalidOperationError("Stream is already closed.")
        self.open = False

    @abstractmethod
    def read(self):
        pass


class FileStream(Stream):
    def read(self):
        return "reading data from file..."


class NetworkStream(Stream):
    def read(self):
        return "reading data from network..."


class MemoryStream(Stream):
    def read(self):
        return "MemoryStream stream"


s = MemoryStream()
print(s.read())
```

### Polymorphism

```python
from abc import ABC, abstractmethod


class UIControl(ABC):
    @abstractmethod
    def draw(self):
        pass


class TextBox(UIControl):
    def draw(self):
        return "TextBox Draw"


class DropDownList(UIControl):
    def draw(self):
        return "DropDownList Draw"


# Duck Typing
def draw(control):
    for ele in control:
        print(ele.draw())


tb = TextBox()
ddl = DropDownList()

draw([tb, ddl])
```

### Extending Built-in Types

```python
class Text(str):
    def duplicate(self):
        return self + self


class Track(list):
    # extending append method from the list class

    def append(self, object):
        super().append(object)
        print(object, "appended")


text = Text("Python")
track = Track()

print(text.duplicate())
numbers = [1, 2, 3]
track.append(4)

```

### Data Classes

```python
from collections import namedtuple


# class Point:
#     def __init__(self, x, y):
#         self.x = x
#         self.y = y
#
#     def __eq__(self, other):
#         return self.x == other.x and self.y == other.y

# if class contains only data use namedtuple insted

Point = namedtuple("Point", ["x", "y"])  # Immutable

p1 = Point(x=1, y=2)
# p1.x = 3  # AttributeError can't set attribute
p1 = Point(x=2, y=1)
p2 = Point(1, 2)

print(p1 == p2)  # False
print(p1.y)  # 1
```

## Modules

- sales.py

```python
def calc_tax():
    pass


def calc_shipping():
    pass
```

- app.py

```python
from sales import calc_shipping, calc_tax

# import sales

print(calc_shipping())
print(calc_tax())
```

### sys.path

```python
import sys

print(sys.path)
```

### Package's

- Folder(package) --> \_\_init\_\_.py, modules.py
- python File(Module) --> classes, objects, methods

```python
# from ecommerce.sales import calc_shipping
from ecommerce import sales

print(sales.calc_shipping())
```

```python
import ecommerce.sales

print(ecommerce.sales.calc_shipping())
```

### Sub Packages

```python
import ecommerce.shopping.sales

print(ecommerce.shopping.sales.calc_shipping())
```

```python
from ecommerce.shopping import sales

print(sales.calc_shipping())
```

### Intra-package References

- ecommerce(pkg) --> customer(sub-pkg) -> contact(module)
- ecommerce(pkg) --> sales(sub-pkg) -> sales(module)

```python
from ecommerce.customer import contact  # Absolute (pep8)
from ..customer import contact  # Relative
```

### dir()

```python
from ecommerce.shopping import sales

print(dir(sales))
print(sales.__name__)  # ecommerce.shopping.sales
print(sales.__package__)  # ecommerce.shopping
# print(sales.__cached__)
print(sales.__file__)
```
