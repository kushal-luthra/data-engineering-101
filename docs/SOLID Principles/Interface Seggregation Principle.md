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