## OOPS in python 

#### Key terminologies
- **Classes** are logical collection of attributes and methods;
- **Objects** are instance of class.
- Actions performed are covered in **Methods**
- To access attribute of your class, use **self parameter**
- Process of creating your object is called **object instantiation**

Everything in python is an object. <br>
eg - mylist = [1,2,3] --if we do type() on this, it returns o/t as 'class list'.<br>
That means, list is a class in python, and above mylist is an object of class list.<br>
So, if you call function, say, append, then it invokes method of class list.<br>
mylist.append(4) -> [1,2,3,4]<br>


#### Attributes
Attributes - an attribute is a property that further defines a class.<br>

##### What is a class attribute? 
- a class attribute are attributes that are shared across all instances of a class.<br>
eg - no. of working hours. <br>
- they are created either as a part of the class or by using className.attributeName.<br>
- so if edited or modified, then reflected for all object instances.<br>
How to edit - to access them, use ClassName.ClassAttributeName and change value. <br>
eg -
```
class Employee:
	HoursOfWork = 40

x=Employee()
x.HoursOfWork --> output = 40

Now edit HoursOfWork -->
Employee.HoursOfWork = 30
x.HoursOfWork --> output = 30
```

We can also edit class attribute specific for an object instead for class as a whole. <br>
eg -
```
class Employee:
	HoursOfWork = 40

x=Employee()
x.HoursOfWork --> output = 40

Now edit HoursOfWork -->
x.HoursOfWork = 50
x.HoursOfWork --> output is 50 --> here we created another instance attribute called HoursOfWork(class attribute remains unchanged)

y=Employee()
y.HoursOfWork --> output = 40
```


##### What is Instance attribute?
Instance attribute are attributes that are specific to each instance of a class. <br>
eg - <br>
```
class Employee:
	HoursOfWork = 40

x=Employee()
x.name = "ram"  --> creates an instance attribute for object 'x'
x.name --> output = ram

y=Employee()
y.name --> give error coz no instance attribute associated with 'y'
```

They are created using objectName.attributeName  <br>
So, if edited or modified, then reflected only for that object instance.  <br>

##### How self parameter is handled?
The method call objectName.methodName() is interpreted as className.methodName(objectName), and this parameter is referred to as '**self**' in method definition.  <br>

#### Methods
Attributes - an attribute is a property that further defines a class.<br>
Method - a method is a function within a class which can access all the attributes of a class and perform a specific task.<br>

##### What is Instance Method?
Methods that accept self parameter as default parameter. <br>
They are used to modify instance attributes. <br>

##### What is a static method?
Methods that do not modify instance attributes of a class are called Static Methods.  <br>
But they can be used to modify Class Attributes. <br>

##### What is init() method?
_init()_ method is the initializer in python. <br>
It is called when an object is instantiated. <br>
All the attributes of the class should be initialized in this method to make your object a fully initialized object. <br>

It has the default parameter as the object are static methods.  <br>


#### self Variable
When you call any class method, then by default, first argument that is passed is class object name. <br>
eg - 
```
class Employee:
	def employeeDetails()
		pass


employeOne = Employee()
employeOne.employeeDetails() is same  as employeeOne.emplpyeDetails(employeeOne)
```

By using self parameter, you are referring to attributes of that object, and the values persist until lifespan of program or until you manually delete the object.
Eg -
```
class Employee:
	def employeeDetails(self):
		self.name = "kush"
		print("name = ", self.name)
		
		age = 30 --> since we didnt use "self", the value age is not available to other methods
		print("age = ",age)
	
	def printEmployeeDetails(self):
		print("my name = ", self.name)
		print("my age = ",age) --> here it will throw error since "age" is not available to other functions. Its scope is restricted to method employeeDetails.
		
```


#### Static Methods and Instance Methods
Instance Methods
- They are methods of our class that make use of self parameter and modify instance attributes of our class. <br>

Static Methods-
- They are methods that do not take default self parameter.  <br>
- To differentiate between static and instance method, we use decorator  @staticmethod <br>

**Decorators** are functions that take another function as input and extend their functionality. <br>
They are denoted by starting them with '@' symbol.  <br>
For example - _@staticmethod_ is a decorator that takes the function as message, extends its functionality  and ignores the binding of the object.  <br>
```
class Employee:
	@staticmethod
	def WelcomeMessage():
		print("welcome to our organization")
```

init() method <br>
- init() method is a special method in python which is used to initialize an object. <br>
eg -
```
class Employee:
	
    def setName(self):
        self.name="ram"

    def printName(self):
        print(self.name)



x=employee()
x.printName() --> it gives error coz we havent initialized name.
```

- init() method is first method to be called in a class.
```
class Employee:
	def __init__(self, name):
		self.name = name
	
	def printName(self):
		print(self.name)

x=employee("ram")
x.printName() --> output is Ram

y=employee("shyam")
y.printName() --> output is shyam
```


### Abstraction and Encapsulation
Abstraction is the process of hiding implementation details from the user. <br>
Encapsulation is done to achieve abstraction. <br>
You use method of class via abstraction.  <br>


### Inheritance
- allows code re-usability.

#### Type 1 : single level inheritance
eg 1 - 
```	
dad = good baseball player. 
son = good baseball player.
```

eg 2 - 

```
class Apple:
	manufacturer = "Apple Inc."
	contactWebsite = "www.apple.com/contact"
	
	def contactDetails(self):
		print("To contact us, log on to ", self.contactWebsite )

class MacBook(Apple):
	def __init__(self):
		self.yearOfManufacture = 2017

	def manufactureDetails(self):
		print("This macbook was manufactured in year {} by {}"
		.format(self.yearOfManufacture, self.manufacturer)")

mymacbook = MacBook()

# output = This macbook was manufactured in year 2017 by Apple Inc.
mymacbook.manufactureDetails

# To contact us, log on to www.apple.com/contact
mymacbook.contactDetails 
```

#### Type2: Multiple Inheritances
eg 1 - 
```
dad = good baseball player.
mom = good cook. 
son = good baseball player and good cook
```
eg 2 - 
```
class OperatingSystem:
	multitasking = True
	name = "Windows OS"

Class Apple:
	website = "www.apple.com"
	name = "Apple OS"

Class MacBook(OperatingSystem, Apple):
	def __init__(self):
		if self.multitasking is True:
		
			# output = this is a multitasking system. Visit www.apple.com for more details.
			print("this is a multi tasking system. Visit {} for more details".format(self.website))
			
			#output = Window OS --why - coz we inherited OperatingSystem class first, and so variable present in both classes will be picked up from first class.
			print("name = ".self.name)



Class NewMacBook(Apple, OperatingSystem):
	def __init__(self):
		if self.multitasking is True:
			# output = this is a multitasking system. Visit www.apple.com for more details.
			print("this is a multi tasking system. Visit {} for more details".format(seld.website))
			
			# output = Apple OS --why - coz we inherited Apple class first, and so variable present in both classes will be picked up from first class. 
			print("name = ".self.name)


```

#### Type 3: multi level inheritance
eg 1- 
```
grandfather = good baseball player
father = good soccer player 
son = good baseball and soccer player
```

eg 2- 

```
class MusicalInstruments:
	numberOfMajorKeys = 12
	
class StringInstruments(MusicalInstruments):
	typeOfWood = "ToneWood"

class Guitar(StringInstruments):
	def __init__(self):
		self.numberOfStrings = 6
		print("this guitar consists of {} strings. It is made of {} and it can play {} keys"
		.format(self.numberOfStrings, self.typeOfWood, self.numberOfMajorKeys))

guitar = Guitar()
output - 
	This guitar consists of 6 strings. It is made of ToneWood and can play 12 keys
```



### Access Specifiers - public, private and protected
- public - 
  - your class, derived class, and further sub-classes derived from derived class --all can access values in main class.
- protected - 
  - only your class and derived class members can access data and methods of your class.
- private- 
  - only your class can access methods and attributes of your class.

To declare attribute or method as - <br>
	1. public - MemberName <br>
	2. Protected - _memberName --> single underscore. <br>
	3. Private - __MemberName --> double underscore. <br>

```
class Car:
	numberOfWheels = 4
	_color = "Black"
	__YearOfManufacture = 2018 #python saves it as _Car__yearOfManufacture i.e _ClassName__AttributeName

Class BMW(Car):
	def __init__(self):
		print("Protected attrubte color : ", self._color)

car=Car()
print("Public Attribute numberOfWheels: ", self.car.NumberOfWheels) --> output =  Public Attribute NumberOfWheels: 4

bmw = BMW() --> output =  Protected attribute color : Black

print("Private Attribute yearOfManufacture: ", car.__yearOfManufacture  ) --> output = error
print("Private Attribute yearOfManufacture: ", car._Car__yearOfManufacture) --> output = Private attribute YearOfManufacture : 2018
```


### PolyMorphism 
It is characteristic of an entity to be able to be present in more than one form. <br>
eg - "+" operator when applied to 2 numbers returns their sum, but whe applied to 2 strings, returns their concatenation.  <br>

#### Overriding and Super() method
Overriding = redefine base class method in your derived class.  <br>

super() function allows you to transfer control back to base class in a derived class. <br>
It is used in case you want to access method in base class. <br>

```
class Employee:
	def setNumberOfWorkingHours(self):
		self.NumberOfWorkingHours = 10


	def displayNumberOfWorkingHours(self):
		print(self.NumberOfWorkingHours)


employee = Employee()
employee.setNumberOfWorkingHours()
print("Number of working hours of emplyee = ", end = ' ')
employee.displayNumberOfWorkingHours()

output -
Number of working hours of Employee = 10


class Trainee(Employee):
	def setNumberOfWorkingHours(self):
		self.NumberOfWorkingHours = 20


	def resetNumberOfWorkingHours(self):
		super().setNumberOfWorkingHours()

trainee = Trainee()

trainee.setNumberOfWorkingHours()
print("Number of working hours of Trainee = ", end = ' ')
trainee.displayNumberOfWorkingHours()


trainee.resetNumberOfWorkingHours()
print("Number of working hours of Trainee after reset = ", end = ' ')
trainee.displayNumberOfWorkingHours()


output -
Number of working hours of Trainee = 20
Number of working hours of Trainee after reset = 10
```

#### Diamond Problem
Here we have 4 classes - A, B, C and D <br>
- A is base class <br>
- B anc C derived from A
- D is derived from B and C


##### Case 1: Method() will Not be overridden in Class B and Class C
```
class A:
	def method(self):
		print("this method belongs to class A")

	pass


class B(A):
	pass

class C(A):
	pass

class D(B,C):
	pass

d = D()
d.method()

output -->
This method belongs to class A
```

##### Case 2: Method() will be overridden in Class B but not in Class C
```
class A:
	def method(self):
		print("this method belongs to class A")

	pass

class B(A):
	def method(self):
		print("this method belongs to class B")

class C(A):
	pass

class D(B,C):
	pass

d = D()
d.method()
output --> This method belongs to class B

```

##### Case 3: Method() will be overridden in Class C but not in Class B
```
class A:
	def method(self):
		print("this method belongs to class A")

class B(A):
	pass

class C(A):
	def method(self):
		print("this method belongs to class C")

class D(B,C):
	pass

d = D()
d.method()
output --> This method belongs to class C
```

##### Case 4: Method() will be overridden in both Class B and Class C
```
class A:
	def method(self):
		print("this method belongs to class A")

class B(A):
	def method(self):
		print("this method belongs to class B")

class C(A):
	def method(self):
		print("this method belongs to class C")

class D(B,C):
	pass

d = D()
output -> This method belongs to class B
why this output -> 
Method Resolution Order -- in case of derived class from multiple classes, method picked from 1st class to which it is obtained;


Had we defined class D as below, output would have been different. 
class D(C,B):
	pass

d = D()
d.method() --> output = This method belongs to class C
```

#### Operator Overloading
There are internal functions of python. <br>
They are overridden in following way -  <br>
eg - overloading addition operator,  the "+" operator when  <br>
- applied to 2 numbers returns their sum. <br>
- applied to 2 strings, returns their concatenation.

```
class Square:
	def __init__(self, side):
		self.side = side


sq1 = Square(5)
sq2 = Square(10)

# output -> ERROR
# Why -> sq1 + sq2 = error coz '+' operator doesn't work on object of class square
print("the sum of sides of both the squares = ", sq1 + sq2)  

So, we need a mechanism to add sides of square and return value.
That should allow us to add above 2 square objects as well.

Solution ->
overload special method add() in your class

class Square:
	def __init__(self, side):
		self.side = side

	def __add__(square1, square2):
		return( (4 * square1.side) + (4 * square2.side) )

sq1 = Square(5)
sq2 = Square(10)

# output -> 60
print("the sum of sides of both the squares = ", sq1 + sq2)

```

### Abstract Base Class
An Abstract Base Class is a base class which doesn't have a definition of its own. <br>
It has abstract methods which forces its implementation in derived classes  <br>

**To declare a class as abstract base class**, it needs to be declared as instance of **ABCMeta class of python**. <br>
eg -

```
# we import ABCMeta and abstractmethod from abc module of python

from abc import ABCMeta, abstractmethod

class shape(metaclass = ABCMeta):
	
	#decorator abstractmethod tells that given method is required to be defined in derived classes.
	@abstractmethod    
	def area(self):
		return 0
	
	#here we say that method area() does not have a defn of its own; but it needs to be defined in derived classes.


class Square(shape):
	side = 4

	def area(self):
		print("area of square", self.side * self.side)

class Rectangle(Shape):
	width = 5
	breadth = 10

	def area(self):
		print("area of rectangle " , self.width * self.length)

sq = Square()
rc = Rectangle()

sq.area()
rc.area()
```

**Note** - if no definition of method area() in class Square and even if we don't call sq.area() --> we get error

An abstract can only be inherited in derived classes, but cannot be instantiated. <br>
If we try to create object of abstract class, we get error.  <br>
```
sh = Shape()
```

**Summary**  <br>
 - polymorphism = ability of an entity to be able to exist in more than one form.  <br>
 - overriding = form of polymorphism in which we redefine method of base class in a derived class. 
   - By doing this we change behaviour of a base class method.  <br>
   - How -
```
	class BaseClass:
		   def BaseClassMethod():
			   #define behaviour
	class DerivedClass(BaseClass):
		   def baseClassMethod():
			   #redefine behaviour
```

 - Operator Overloading - defining a special method for an operator within your class to handle the operation between the objects of that class is called **Operator Overloading**.

 - Abstract Base Class - a base class which consists of abstract methods that should be implemented in its derived class is called abstract base class. <br>
Syntax -
```
	from abc import ABCMeta, abstractmethod
	
	class baseClass(metaclass = ABCMeta):
		
		@abstractmethod
		def abstractMethod(self):
			return
```



