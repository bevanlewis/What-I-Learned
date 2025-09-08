# Python Classes and OOP

Classes are blueprints for creating objects. Object-oriented programming (OOP) allows you to model real-world entities and relationships using classes, objects, inheritance, and other OOP principles.

## Defining Classes

### Basic Class Structure

```python
class Person:
    """A simple Person class"""

    # Class attribute (shared by all instances)
    species = "Homo sapiens"

    # Constructor method
    def __init__(self, name, age):
        # Instance attributes
        self.name = name
        self.age = age

    # Instance method
    def introduce(self):
        return f"Hello, my name is {self.name} and I am {self.age} years old."

    # Another instance method
    def have_birthday(self):
        self.age += 1
        return f"Happy birthday! You are now {self.age} years old."

# Creating objects (instances)
person1 = Person("Alice", 30)
person2 = Person("Bob", 25)

print(person1.introduce())    # "Hello, my name is Alice and I am 30 years old."
print(person2.introduce())    # "Hello, my name is Bob and I am 25 years old."

# Accessing class attributes
print(Person.species)         # "Homo sapiens"
print(person1.species)        # "Homo sapiens" (inherited from class)
```

## Instance vs Class Attributes

```python
class Example:
    # Class attribute
    class_attr = "I'm shared"

    def __init__(self, value):
        # Instance attribute
        self.instance_attr = value

obj1 = Example("value1")
obj2 = Example("value2")

print(Example.class_attr)     # "I'm shared"
print(obj1.class_attr)        # "I'm shared"
print(obj2.class_attr)        # "I'm shared"

# Modifying class attribute affects all instances
Example.class_attr = "Changed"
print(obj1.class_attr)        # "Changed"
print(obj2.class_attr)        # "Changed"

# Modifying instance attribute only affects that instance
obj1.instance_attr = "modified"
print(obj1.instance_attr)     # "modified"
print(obj2.instance_attr)     # "value2"
```

## Methods

### Instance Methods

```python
class Calculator:
    def __init__(self):
        self.memory = 0

    def add(self, a, b):
        result = a + b
        self.memory = result  # Store in memory
        return result

    def multiply(self, a, b):
        result = a * b
        self.memory = result
        return result

    def get_memory(self):
        return self.memory

calc = Calculator()
print(calc.add(5, 3))        # 8
print(calc.get_memory())     # 8
print(calc.multiply(4, 2))   # 8
print(calc.get_memory())     # 8
```

### Class Methods

```python
class Circle:
    pi = 3.14159

    def __init__(self, radius):
        self.radius = radius

    # Instance method
    def area(self):
        return self.pi * self.radius ** 2

    # Class method - works with class, not instance
    @classmethod
    def from_diameter(cls, diameter):
        radius = diameter / 2
        return cls(radius)

    # Class method for changing class attributes
    @classmethod
    def set_pi(cls, new_pi):
        cls.pi = new_pi

# Using class method
circle1 = Circle(5)
print(circle1.area())  # 78.53975

circle2 = Circle.from_diameter(10)
print(circle2.radius)  # 5.0

Circle.set_pi(3.14)
print(circle1.area())  # 78.5 (using new pi value)
```

### Static Methods

```python
class MathUtils:
    @staticmethod
    def is_even(number):
        return number % 2 == 0

    @staticmethod
    def factorial(n):
        if n == 0 or n == 1:
            return 1
        return n * MathUtils.factorial(n - 1)

# Static methods can be called on class or instance
print(MathUtils.is_even(4))     # True
print(MathUtils.factorial(5))   # 120

utils = MathUtils()
print(utils.is_even(7))         # False
```

## Properties

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero")
        self._celsius = value

    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        self._celsius = (value - 32) * 5/9

temp = Temperature(25)
print(temp.celsius)      # 25
print(temp.fahrenheit)   # 77.0

temp.fahrenheit = 100
print(temp.celsius)      # 37.777...
print(temp.fahrenheit)   # 100.0
```

## Inheritance

### Basic Inheritance

```python
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species

    def make_sound(self):
        return "Some generic animal sound"

    def describe(self):
        return f"I am {self.name}, a {self.species}"

class Dog(Animal):
    def __init__(self, name, breed):
        # Call parent constructor
        super().__init__(name, "dog")
        self.breed = breed

    def make_sound(self):
        return "Woof!"

    def fetch(self, item):
        return f"{self.name} fetches the {item}"

class Cat(Animal):
    def __init__(self, name, color):
        super().__init__(name, "cat")
        self.color = color

    def make_sound(self):
        return "Meow!"

# Usage
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers", "gray")

print(dog.describe())        # "I am Buddy, a dog"
print(dog.make_sound())      # "Woof!"
print(dog.fetch("ball"))     # "Buddy fetches the ball"

print(cat.describe())        # "I am Whiskers, a cat"
print(cat.make_sound())      # "Meow!"
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "I can fly!"

class Swimmable:
    def swim(self):
        return "I can swim!"

class Duck(Flyable, Swimmable):
    def __init__(self, name):
        self.name = name

    def quack(self):
        return "Quack!"

duck = Duck("Donald")
print(duck.fly())     # "I can fly!"
print(duck.swim())    # "I can swim!"
print(duck.quack())   # "Quack!"

# Method Resolution Order (MRO)
print(Duck.__mro__)   # (<class '__main__.Duck'>, <class '__main__.Flyable'>, <class '__main__.Swimmable'>, <class 'object'>)
```

### Method Overriding and super()

```python
class Parent:
    def __init__(self, name):
        self.name = name
        print(f"Parent __init__ called for {name}")

    def greet(self):
        return f"Hello from {self.name}"

class Child(Parent):
    def __init__(self, name, age):
        # Call parent __init__
        super().__init__(name)
        self.age = age
        print(f"Child __init__ called for {name}, age {age}")

    def greet(self):
        # Call parent method and extend it
        parent_greeting = super().greet()
        return f"{parent_greeting}. I am {self.age} years old."

child = Child("Alice", 10)
print(child.greet())  # "Hello from Alice. I am 10 years old."
```

## Special Methods (Dunder Methods)

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __len__(self):
        return 2  # Number of components

    def __getitem__(self, key):
        if key == 0:
            return self.x
        elif key == 1:
            return self.y
        else:
            raise IndexError("Vector index out of range")

v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1)             # Vector(1, 2)
print(repr(v1))       # Vector(1, 2)
print(v1 + v2)        # Vector(4, 6)
print(v1 == v2)       # False
print(len(v1))        # 2
print(v1[0])          # 1
print(v1[1])          # 2
```

## Class and Static Variables

```python
class Car:
    # Class variables
    wheels = 4
    total_cars = 0

    def __init__(self, make, model):
        self.make = make      # Instance variable
        self.model = model    # Instance variable
        Car.total_cars += 1   # Modify class variable

    @classmethod
    def get_total_cars(cls):
        return cls.total_cars

    @classmethod
    def set_wheels(cls, wheels):
        cls.wheels = wheels

car1 = Car("Toyota", "Camry")
car2 = Car("Honda", "Civic")

print(Car.total_cars)        # 2
print(car1.total_cars)       # 2
print(Car.wheels)            # 4

Car.set_wheels(3)            # Change class variable
print(Car.wheels)            # 3
print(car1.wheels)           # 3 (affected by class change)

# Instance can have its own version
car1.wheels = 6
print(car1.wheels)           # 6 (instance variable)
print(Car.wheels)            # 3 (class variable unchanged)
```

## Abstract Base Classes

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

    def describe(self):
        return f"This is a {self.__class__.__name__}"

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

    def perimeter(self):
        return 2 * 3.14159 * self.radius

# Usage
rect = Rectangle(5, 3)
circle = Circle(4)

print(rect.area())       # 15
print(circle.area())     # 50.26544
print(rect.describe())   # "This is a Rectangle"

# Cannot instantiate abstract class
# shape = Shape()  # TypeError
```

## Composition vs Inheritance

```python
# Inheritance (is-a relationship)
class ElectricCar(Car):
    def __init__(self, make, model, battery_capacity):
        super().__init__(make, model)
        self.battery_capacity = battery_capacity

# Composition (has-a relationship)
class Battery:
    def __init__(self, capacity):
        self.capacity = capacity

    def charge(self):
        return "Battery charging..."

class ElectricCar:
    def __init__(self, make, model, battery_capacity):
        self.make = make
        self.model = model
        self.battery = Battery(battery_capacity)

    def charge(self):
        return self.battery.charge()

# Usage
car = ElectricCar("Tesla", "Model S", 100)
print(car.charge())  # "Battery charging..."
```

## Data Classes (Python 3.7+)

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str = "Unknown"  # Default value

    def greet(self):
        return f"Hello, I'm {self.name} from {self.city}"

# Automatically generates __init__, __repr__, __eq__, etc.
person1 = Person("Alice", 30, "New York")
person2 = Person("Bob", 25)

print(person1)           # Person(name='Alice', age=30, city='New York')
print(person1.greet())   # "Hello, I'm Alice from New York"
print(person1 == person2)  # False
```

## Best Practices

### 1. Use descriptive class and method names

```python
# Good
class CustomerAccount:
    def calculate_total_balance(self):
        pass

# Avoid
class CA:
    def calc(self):
        pass
```

### 2. Keep classes focused (Single Responsibility Principle)

```python
# Good: Separate concerns
class UserManager:
    def create_user(self, data):
        pass

class EmailService:
    def send_welcome_email(self, user):
        pass

# Avoid: Mixed responsibilities
class UserHandler:
    def create_user_and_send_email(self, data):
        pass
```

### 3. Use properties for encapsulation

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    @balance.setter
    def balance(self, value):
        if value < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = value
```

### 4. Prefer composition over inheritance

```python
# Good: Composition
class Car:
    def __init__(self, engine):
        self.engine = engine

# Avoid: Deep inheritance hierarchy
class SportsCar(Car):
    pass
```

### 5. Use abstract base classes for interfaces

```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass

    @abstractmethod
    def query(self, sql):
        pass
```

### 6. Implement comparison methods when needed

```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

    def __eq__(self, other):
        return self.grade == other.grade

    def __lt__(self, other):
        return self.grade < other.grade

    def __repr__(self):
        return f"Student({self.name}, {self.grade})"

students = [Student("Alice", 85), Student("Bob", 92), Student("Charlie", 78)]
students.sort()  # Uses __lt__
print(students)
```

### 7. Use class methods for alternative constructors

```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def from_string(cls, date_string):
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)

    @classmethod
    def today(cls):
        # Implementation to get today's date
        return cls(2023, 12, 25)

date1 = Date(2023, 12, 25)
date2 = Date.from_string("2023-12-25")
date3 = Date.today()
```

### 8. Document your classes and methods

```python
class Calculator:
    """A simple calculator class for basic arithmetic operations."""

    def __init__(self):
        """Initialize calculator with memory set to 0."""
        self.memory = 0

    def add(self, a: float, b: float) -> float:
        """
        Add two numbers and store result in memory.

        Args:
            a: First number
            b: Second number

        Returns:
            Sum of a and b
        """
        result = a + b
        self.memory = result
        return result
```

## Common Design Patterns

### Singleton Pattern

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        if not hasattr(self, 'initialized'):
            self.initialized = True
            self.value = 0

# Usage
s1 = Singleton()
s1.value = 42
s2 = Singleton()
print(s2.value)  # 42 (same instance)
```

### Factory Pattern

```python
class AnimalFactory:
    @staticmethod
    def create_animal(animal_type, name):
        if animal_type == "dog":
            return Dog(name)
        elif animal_type == "cat":
            return Cat(name)
        else:
            raise ValueError(f"Unknown animal type: {animal_type}")

# Usage
dog = AnimalFactory.create_animal("dog", "Buddy")
cat = AnimalFactory.create_animal("cat", "Whiskers")
```

### Observer Pattern

```python
class Subject:
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self, message):
        for observer in self._observers:
            observer.update(message)

class Observer:
    def __init__(self, name):
        self.name = name

    def update(self, message):
        print(f"{self.name} received: {message}")

# Usage
subject = Subject()
observer1 = Observer("Observer 1")
observer2 = Observer("Observer 2")

subject.attach(observer1)
subject.attach(observer2)
subject.notify("Hello, observers!")
```

This comprehensive guide covers the fundamentals and advanced concepts of classes and object-oriented programming in Python.
