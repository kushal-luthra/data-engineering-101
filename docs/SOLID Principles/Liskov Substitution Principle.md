### Introduction

![img_24.png](img_24.png) <br>

'L' stands for Liskov Substitution Principle. This is often abbreviated as LSP. <br>
It is named after a computer scientist Barbara Liskov who explained this principle in a book that she authored. <br>

The principle says "Objects should be replaceable with their subtypes without affecting the correctness of the program" <br>

To explain this, we will start with inheritance, which is a basic feature of any object oriented programming language. <br>

#### 'Is-A' relationship
Inheritance is also referred to as the 'Is-A' relationship. <br>
![img_25.png](img_25.png) <br>

Take the example of a car class. <br>
A hatchback extends a car class. So we say hatchback 'is-a' car. <br>

Let's take another one: We have a bird class, and ostrich extends bird. So Ostrich 'is-a' bird. <br>
Another one: Fuel and gasoline. So gasoline extends fuel, or gasoline 'is-a' fuel. <br>

On the face of it, all of this sounds perfect and sounds like text-book examples. <br>

But there are hidden problems with this approach, which may not seem obvious at first. <br>

Among these 3 examples, the one in the middle has a hidden problem i.e. _Ostrich is-a bird_. <br>

So Ostrich is a bird. And by all means of real-world classification, an ostrich is a bird alright. <br>

As per the biological classification too, ostrich is a bird. <br>
But, here's an interesting fact, an ostrich cannot fly. <br>

Let's see this in code now. <br>
![img_26.png](img_26.png) <br>

So we have a Bird class with a fly method. <br>
Then, we have an Ostrich class that extends the Bird class. <br>
Because the fly() method does not make sense for an Ostrich, what the Ostrich class does is, it overrides the fly method from the Bird class and leaves it unimplemented. <br>
**Unimplemented methods are almost always indicative of a design flaw**. <br>
Now, the statement Ostrich 'is-a' bird might still be correct. <br>

But if we apply the Liskov Principle here, which says: Objects should be replaceable with their subtypes without affecting the correctness of the program.   <br>
This test fails - because you cannot use an Ostrich object in all the places where you use the Bird object. <br>
If you do so, and someone calls the fly() method on the Bird object, your program will fail. <br>

So the Liskov Substitution Principle requires a test standard that is more strict than the 'is-a' test. <br>

It wants us to move away from the 'is-a' way of thinking. <br>

![img_27.png](img_27.png) <br>
 <br>

This is a line often associated with the Liskov Substitution Principle: If it looks like a duck and quacks like a duck, but it needs batteries, you probably have the wrong abstraction! <br>
Instead, the Liskov way of thinking should be: Objects should be replaceable with their subtypes without affecting the correctness of the program. <br>


### Breaking the Hierarchy
Here we are going to examine the Liskov Substitution Principle in depth. <br>
We will take 2 examples and analyze and find out the design flaws in each one of them. <br>
Then we will apply the Liskov Substitution Principle and improve the design. <br> <br>

One example is discussed here, and we use the approach 'Breaking the Hierarchy'. <br>

In a given case, we have a generic Car class. <br>
We have a racing car, the ones from the Formula One racing. <br>
Racing car extends car. <br>

Let's see what the code looks like now. <br> <br>

![img_28.png](img_28.png) <br> <br>


The Car class has one method getCabinWidth() that returns the cabin width of the car. <br>

The RacingCar class overrides the getCabinWidth() function and leaves it unimplemented. <br>

Why? That's because racing cars have a set of specifications some of which might not match that of a generic car. <br>

In a generic car, we call the width as cabin width. <br>
But in a racing car, there is no cabin. The interior space is called a cockpit. <br>
So racing cars do not have a cabin width. But they have a cockpit width. <br>
So the racing car implements a getCockpitWidth() method accordingly.  <br>

Now, lets create some objects of Cars and RacingCars and play around with it. <br>

![img_29.png](img_29.png) <br>

In order to do this, we will look at a utils class called CarUtils. <br>
So CarUtils instantiates 3 objects with reference Type Car. <br>
Note that even though the reference for all 3 objects is Car,2 of them are generic car instances, the 3rd one is a racing car instance. <br>

We insert all 3 car reference objects into an ArrayList and name it myCars. <br>

Next we iterate through the car list and we print out the cabin width of each car. <br>

The first two objects are Car Objects. So the getCabinwidth method works fine. <br>
But in the third iteration, the object is a RacingCar object. <br>
And RacingCar overrides the getCabinWidth() method and leaves it unimplemented. <br>
So in the third iterations, this code will not work correctly, because of an unimplemented method. <br>

_So this is the design hole that has been exposed._ <br>

So how do you fix this? <br>
![img_30.png](img_30.png) <br> <br>
We have to strike at the root, which is the inheritance itself. <br>
So we have to break the inheritance. <br>
RacingCar should no longer extend Car. <br>
Instead, we'll come up with a common parent for both. <br>
So we create a new class called Vehicle which is a very generic class which can represent any modes of transportation including a truck, a boat, or an airplane. <br>
Then we make both Car and RacingCar extend this Vehicle class. <br>

We need to restructure the code also accordingly. <br>

Let's see how that can be done. <br>
First is the vehicle class, which has one method, called getInteriorWidth(). <br>
Note that it is neither cabin width nor cockpit width, but a much more generic abstraction called interior width. <br>
Next we have the Car class that extends Vehicle. The 'Car' class overrides the getInteriorWidth(),which in turn calls its getCabinWidth() method internally. <br>

Next we have the RacingCar class that also extends Vehicle. <br>

The 'RacingCar' class also overrides the same getInteriorWidth(), which in turn calls its getCockpitWidth() method internally. <br>
So that's the code restructuring that is needed to follow the new hierarchy. <br>

Finally, lets look at the same CarUtils class again. <br>
We have renamed it to VehicleUtils by the way. <br>
Again, we have the first two car objects and the third RacingCar object. <br>

First two iterations, the getInteriorWidth() method gets called on the car objects. The Car objects, in turn,call their getCabinWidth method. <br>
In the third and last iteration, the getInteriorWidth() method gets called on the RacingCar objects, which in turn, call their getCockpitWidth method. <br>
So all calls work correctly. And the whole thing is dynamic. <br>
Because of the way we designed the hierarchy, we can simply iterate through any number of objects of reference type Vehicle and get the interior width,without ever bothering to know if they are Car objects or RacingCar objects. <br>

So the solution that we applied to this pattern of problems is called '**_Breaking the hierarchy_**'. <br>

There is one more pattern where we apply the Liskov Substitution Principle in a different way. <br>


### Tell, Don't Ask
Case Study -> <br>
We have a generic Product class.  We also have an InHouseProduct class that extends the Generic Product class. <br>
Just think of it from the perspective of an e-commerce website like Amazon. <br>
Amazon sells a number of products on its website, mostly from third party sellers. But Amazon has its own set of products too, which they manufacture InHouse (Amazon Basics). <br>

Assume ,all Products get a base discount of 20%. <br>
But, if it's an In-House Product, it gets 1.5 times the existing base discount. <br>
This is the hierarchy that we need to implement. <br>

Lets start with an initial version. <br> <br>

![img_31.png](img_31.png) <br> <br>

We have a product class, which has a discount variable, and a getDiscount() method which returns the discount percentage. <br>
Then we have the InHouseProduct class, which has a applyExtraDiscount method, that simply takes the existing discount and multiplies it by 1.5. <br>
Note that the InHouseProduct does NOT override the getDiscount() method, so it will simply inherit the getDiscount() method from its parent, the Product class. <br>
We will now use a bunch of these product and InHouseProduct objects in another class. This class is named PricingUtils. <br>
 <br>
It instantiates 3 objects, 2 of them generic products, and the third one an InHouse Product. <br>
Note that all 3 are of type reference Product. <br>
Next, we iterate through this list of objects which we just created. <br>
 <br>
For the first two products, the 'if' condition returns false, because they are not InHOuseProducts, and so the instanceof operator returns false. <br>
So it skips what is inside the 'if' condition. <br>
And it simply prints the discount for each of the two products. For the third product though, because the product is an instance of InHouseProduct, it gets inside the 'if' condition. <br>
Then it calls the applyExtraDiscount() method which multiples the existing discount by 1.5. <br>
Then it exits the 'if' loop and prints the final modified discount. <br> 
<br>
What we are seeing now in this Utils class, is NOT a good design. This is _against the Liskov Substitution Principle_. <br>
We should have been able to deal with all the objects as Product objects itself instead of typecasting to InHouseProduct for some of them. <br>
So how can restructure the code to achieve this? <br>
 <br>
![img_32.png](img_32.png) <br>
 <br>
First, we will override the getDiscount() method in the InHouseProduct class. <br>
The overridden getDiscount() method, in turn, will call the applyExtraDiscount() method. <br>
 <br>
Now, we'll do the corresponding change in PricingUtils. <br>
No change to the product instantiations. <br>
 <br>
The only difference is that the instanceof check is now gone. <br>
 <br>
We do not need to bother if the objects are instances of Generic Product or instances of InHouseProduct. <br>
By doing this, the Liskov substitution test gets passed now. <br>
So objects can be substituted by their subtypes without affecting the correctness of the program, in this particular example. <br>
 <br>
So we have the product reference here, but it contains a InHouseProduct, which is a subtype of Product. <br>
 <br>
Still the program works as expected, and its correctness is not affected. <br>
This is a second pattern of problems, where the Liskov Substitution Principle is applied. <br>
 <br>
The solution to this pattern is called 'Tell, Don't Ask'. <br> 
In the initial version of code,we are ASKING or enquiring about the subtype , from INSIDE the Utils class. <br>
But, lets go the final version now. Here, we are TELLING the Utils class, the Utils class does not need to ask anything. This is what we mean by 'Tell,Don't Ask'. <br>
 <br>
So, to sum up, we saw two patterns of problems, the solution to the first one was to break the hierarchy as we saw in the RacingCar example. <br>
And the solution to the second one is to restructure the code, so as to follow the 'Tell, don't ask' rule. <br>

