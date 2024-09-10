In Python, a class is a blueprint for creating objects. Classes define a set of attributes (variables) and methods (functions) that the created objects will have. Python classes allow you to bundle data and functionality together, providing a way to create and manage complex data structures and behaviors.

Basics of Python Classes

1. Defining a Class:

A class is defined using the class keyword, followed by the class name and a colon.

By convention, class names are written in CamelCase.


class MyClass:
    pass


2. Attributes:

Attributes are variables that belong to the class or instances of the class.

There are two types of attributes:

Class attributes: Shared among all instances of the class.

Instance attributes: Specific to each object (instance) created from the class.



class MyClass:
    class_attribute = "I am a class attribute"

    def __init__(self, value):
        self.instance_attribute = value


3. Methods:

Methods are functions defined within a class that operate on the class’s attributes.

The first parameter of any method in a class is self, which refers to the instance calling the method.


class MyClass:
    def __init__(self, value):
        self.instance_attribute = value

    def instance_method(self):
        return f"Instance attribute value: {self.instance_attribute}"


4. The __init__ Method:

The __init__ method is a special method called a constructor. It is automatically invoked when a new instance of the class is created.

It is used to initialize the object's attributes.


class MyClass:
    def __init__(self, value):
        self.instance_attribute = value


5. Creating an Object (Instance):

An object is created by calling the class as if it were a function.


obj = MyClass("Hello")
print(obj.instance_attribute)  # Output: Hello


6. Accessing Attributes and Methods:

Attributes and methods are accessed using the dot . notation.


print(obj.instance_method())  # Output: Instance attribute value: Hello


7. Inheritance:

Classes can inherit attributes and methods from other classes.

Inheritance allows for code reuse and the creation of a hierarchical class structure.


class Animal:
    def speak(self):
        return "Animal sound"

class Dog(Animal):
    def speak(self):
        return "Bark"

dog = Dog()
print(dog.speak())  # Output: Bark


8. Encapsulation:

Encapsulation refers to the bundling of data and methods within a class.

Access to some attributes or methods can be restricted using a leading underscore _ (protected) or double underscore __ (private).


class MyClass:
    def __init__(self):
        self._protected_attr = "protected"
        self.__private_attr = "private"


9. Polymorphism:

Polymorphism allows different classes to define methods with the same name, allowing objects of different classes to be treated as if they are the same type.


class Cat(Animal):
    def speak(self):
        return "Meow"

animals = [Dog(), Cat()]
for animal in animals:
    print(animal.speak())
# Output:
# Bark
# Meow



Summary

Class: A blueprint for creating objects.

Object: An instance of a class.

Attributes: Variables that store data.

Methods: Functions that operate on the data.

__init__ method: A constructor that initializes objects.

Inheritance: Allows a class to inherit attributes and methods from another class.

Encapsulation: Restricts access to certain attributes/methods.

Polymorphism: Allows objects of different classes to be treated the same based on common methods.


By using classes, you can structure your code in a way that is modular, reusable, and easier to understand.

