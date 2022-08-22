### Introduction
![img_33.png](img_33.png) <br>
 <br>
'I' stands for Interface Segregation Principle. This is often abbreviated as ISP. <br>
This principle says - "No client should be forced to depend on methods it does not use". <br>
 <br>
To explain this, as always, lets start with a real world analogy. <br>
Assume you are working in an office which has around 200 employees. <br>
You have a bunch of printers , scanners and fax machines available for the employees to use. <br>
As a software developer, you have been asked to represent these devices using object oriented concepts in code. <br>
You have to design some interfaces too, to ensure a certain level of uniformity among these devices. <br>
 <br>
You start walking around the office floor, and you spot THIS device.This is a multi-function all-in-one Xerox WorkCentre, that has a printer, scanner, copier, and a fax machine all built into one. <br>
![img_34.png](img_34.png) <br>
 <br>
You think that this could be a good starting point. <br>
You feel that if you can design a common interface based on this all-in-one device, that will give you a good start. <br>
So lets see this in code. <br>
 <br>
First, you design an interface named IMultiFunction. <br>
This interface defines methods that represent various functions. <br>
 ![img_41.png](img_41.png) <br>
 <br>
We have the print() and getPrintSpoolDetails() methods that are associated with printing. <br>
We have the scan() and scanPhoto() methods that are associated with scanning. <br>
The fax() and internetFax() methods deal with fax related functions. <br>
 <br>
Now you create a concrete class that represents this particular device - The Xerox WorkCentre. <br>
You make it implement the IMultiFunction interface. So you will have to implement all the 6 methods(). <br> <br>
![img_38.png](img_38.png)
 <br> <br>
All good so far. <br> <br>

Now you walk over to the next couple of rooms in your office, and encounter 2 different machines. <br> <br>
First is an HP printer-scanner device. <br>
Its not as multi-faceted as the Xerox WorkCentre but it can still do a couple of functions - printing and scanning. <br>
For it, we implement the print(), getPrintSpoolDetails(), scan() and scanPhoto() methods. <br>
What about the other methods in the interface? <br>
If you are implementing an interface, you HAVE to implement ALL methods in it. <br>
So what we will do is we will implement the fax() and internetFax() method and give it a blank implementation. <br>
 <br>
This machine does not have to ability to send or receive fax, which is why, we have to leave these fax related methods unimplemented. <br>
 <br>
Second one, is a Canon printer device and has printing functions alone. <br>
So we will create a class to represent this device and make it implement the IMultiFunction interface again. <br>
We will implement the print() and getPrintSpoolDetails() methods. <br>
But then, we will have to implement all the other methods as well and leave the implementations blank. <br>
So scan(), scanPhoto(), fax(), internetFax() , leave it all blank. <br>
 <br>
![img_36.png](img_36.png) <br>
 <br>

Now you start seeing the problem. <br>

Unimplemented methods are almost always indicative of a poor design. <br>
 <br>
This goes against the Interface Segregation Principle which says:"No client should be forced to depend on methods it does not use". <br>
 <br>
So what is the problem if we don't fix this? Why is this a bad design? <br>
 <br>
We leave the methods blank - What's the big deal? <br>
 <br>
![img_39.png](img_39.png) <br>
 <br>

Assume you have a employee portal application in your office that needs to directly access these devices. <br>
So you have to provide these device classes packaged as a library, to a group of programmers who are working on this employee portal. <br>
 <br>
One programmer wants to send a fax, he sees the fax() method on your Canon Device class, and invokes it. <br>
He does not care to look inside the method, and understand that it is a blank implementation. <br>
He does not need to, in fact. <br>
 <br>
All the programmer sees is the fax() method on the Canon class, assumes that this device has the ability to send a fax, and invokes it from the employee portal. <br>
This is calling for trouble, and the code will definitely break, because this method implementation is blank. <br>
 <br>
So this is the reason why this is a bad design. <br>
 <br>
And this is why Interface Segregation Principle recommends to avoid such designs. <br>

### Restructing the Code to follow ISP
![img_42.png](img_42.png) <br>
 <br>
Continuing our ongoing example, we have one generic interface, and 3 classes that implement it. <br>
Not all methods make sense to all classes, so some have blank method implementations. <br>
So the easiest way to fix this is to split the Big Interface into smaller interfaces <br>
 <br>
So lets take the generic interface IMultiFunction and split it into 3 interfaces - IPrint, IScan and IFax interfaces. <br>
![img_43.png](img_43.png) <br>
 <br>
Now, lets change the classes too accordingly. <br>
So the Xerox WorkCentre will implement all 3 interfaces. Remember, all object oriented programming languages allow a class to implement multiple interfaces. <br>
The HP device class will implement the IPrint and IScan interfaces.  So, we can delete the fax() and internetFax() blank implementations now from this class. <br>
Finally the Canon device will implement the IPrint interface alone, as it can do only printing related functions. <br>
So, we can remove all the remaining blank implementations from the Canon device class. <br>
 <br>
So you can see that the classes have become much cleaner now. <br>
No more blank implementations of methods. <br>
 <br>
If we decide to package these classes as a library, and pass it on to a programmer, then there is no ambiguity - A programmer can only call the defined methods in a class. <br>
So, we have solved the problem by segregating the interfaces.This is one way of following the Interface Segregation Principle. <br>
 <br>
Now, if you feel that there are certain functions that are common between a printer, scanner, and a fax machine, you can have a parent interface with the common methods. <br> 
And you can have the IPrint, IScan, and IFax interfaces extend the parent interface adding specific methods. <br>


### Techniques to identify violations
Lets first look at some standard techniques to identify violations of the interface segregation principle.

1. Fat Interfaces -
Fat interfaces mean interfaces with high number of methods.
If you recall our IMultiFunction interface from our introductory session, it had around 6 methods to deal with multifarious functions like printing, scanning, and fax.
This is almost always indicative of a violation of the Interface Segregation Principle.

2. Interfaces with Low Cohesion.
Lets again talk in the context of our IMultiFunction interface. 
We had methods for fax and photoScan. Now fax and photoScans are two entirely different functions.
One deals with transmitting a printed piece of paper over a telephone line, whereas the other deals with digitally capturing the colors in a photograph.
In other words, there is Low Cohesion between the methods in the IMultiFunction interface.

This is another indication that we are breaking the Interface Segregation Principle in all probability.

3. empty implementations of methods.
Again , we saw this in our introductory session where some classes had to leave certain method implementations as blank.
Blank implementations of interface methods is almost always a violation of the interface segregation principle.

So these are some of the ways you can identify violations of the interface segregation principle.

#### Relationship with other SOLID principles
Next, lets explore the relation that this principle has, with other solid principles. <br>
Lets look at our problem solution page. <br>
![img_43.png](img_43.png) <br>
 <br>
After we split the interface, we can now say that one interface has one responsibility, or a single responsibility. <br>
So the IPrint interface has only a single responsibility, i.e. it holds print related method definitions. <br>
Similarly, the IScan interface has only a single responsibility of holding scanning related method definitions. <br>
So we are inadvertently following the Single Responsibility Principle.  <br>
 <br>
Also, after we segregated the interfaces, we are indirectly following the Liskov Substitution Principle as well. <br>
Because now , in places where we use , for instance, the IPrint interface, we could substitute it with the CanonPrinter class instead. <br>
 <br>
So as you can see , most of the SOLID principles are intricately linked to one another. <br>
 <br>
**_SOLID principles complement each other, and work together in unison to achieve the common purpose of well-designed software_**. <br>