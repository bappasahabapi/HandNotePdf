# 🔥03A-Liskov Substitution Principle (LSP) - Overview

## Introduction
The Liskov Substitution Principle (LSP) states: **"Objects should be replaceable with their subtypes without affecting the correctness of the program."**

## Inheritance
To understand this principle, we will first discuss the concept of inheritance, which is a fundamental feature of any object-oriented programming language. Inheritance is often referred to as the 'Is-A' relationship.

### Examples:
1. **Car Class**: A hatchback extends the car class. So we say a hatchback 'is-a' car.
2. **Bird Class**: An ostrich extends the bird class. So an ostrich 'is-a' bird.
3. **Fuel and Gasoline**: Gasoline extends fuel, meaning gasoline 'is-a' fuel.

### Problems
At first glance, these examples seem perfect, but there are hidden problems with this approach. Particularly, the second example has an issue.

### Analysis
- An ostrich is a bird, but it cannot fly.
- We have a **Bird** class with a `fly` method.
- When the Ostrich class extends the Bird class, it overrides the `fly` method and leaves it unimplemented.

### Design Flaw
**Unimplemented** methods are almost always indicative of a design flaw. While the statement "Ostrich 'is-a' bird" might still be correct, if we apply the Liskov Principle:

**"Objects should be replaceable with their subtypes without affecting the correctness of the program."**

This test fails because you cannot use an Ostrich object in all the places where you use a Bird object.

### Liskov's Perspective
The Liskov Principle requires us to move away from the 'is-a' way of thinking. A popular saying associated with this principle is:

**"If it looks like a duck and quacks like a duck but needs batteries, you probably have the wrong abstraction!"**

Instead, Liskov's way of thinking should be: **"Objects should be replaceable with their subtypes without affecting the correctness of the program."**

--- 

# 🔥03B-Liskov Substitution Principle (LSP) - In-Depth Examination

## Introduction
 Here, we will examine the Liskov Substitution Principle (LSP) in depth. We will take two examples, analyze them to find design flaws, and then apply the Liskov Substitution Principle to improve the design.

## Example 1: Car and RacingCar

### Class Structure
We have a generic **Car** class and a **RacingCar** class that extends the Car class. The arrow indicates inheritance.

### Code Overview
The **Car** class has one method, `getCabinWidth()`, which returns the cabin width of the car. The **RacingCar** class overrides the `getCabinWidth()` method but leaves it unimplemented. This is because racing cars have specifications that may not match those of a generic car.

### Interior Differences
- In a generic car, we refer to the width as cabin width.
- In a racing car, there is no cabin; instead, the interior space is called a cockpit.

Thus, the RacingCar implements a `getCockpitWidth()` method instead.

### Object Creation
Next, we create some objects of Cars and RacingCars using a utility class called **CarUtils**.

- **CarUtils** instantiates three objects with the reference type **Car**. 
- Two of them are instances of **GenericCar**, while the third is an instance of **RacingCar**.
- All three car reference objects are inserted into an ArrayList named `myCars`.

### Iteration Issue
When we iterate through the car list to print out the cabin width of each car:
1. The first two objects are Car objects, so the `getCabinWidth()` method works fine.
2. However, in the third iteration, we encounter a RacingCar object, which has an unimplemented `getCabinWidth()` method. This results in a failure during execution.

### Identifying the Design Flaw
The design flaw here is rooted in inheritance itself. The RacingCar should not extend Car due to its unique characteristics.

## Solution: Breaking the Hierarchy
To fix this issue, we need to break the inheritance chain:
1. Create a new common parent class called **Vehicle**, which can represent any mode of transportation (e.g., truck, boat, airplane).
2. Both **Car** and **RacingCar** will now extend this **Vehicle** class.

### Code Restructuring
1. **Vehicle Class**: Contains one method called `getInteriorWidth()`, which is a more generic abstraction than cabin or cockpit width.
2. **Car Class**: Extends Vehicle and overrides `getInteriorWidth()`, which internally calls its own `getCabinWidth()` method.
3. **RacingCar Class**: Also extends Vehicle and overrides `getInteriorWidth()`, which internally calls its own `getCockpitWidth()` method.

### Updated VehicleUtils Class
The utility class has been renamed to **VehicleUtils**:
- It contains references for both Car and RacingCar objects.
- All three objects are now of reference type Vehicle.

### Iteration Process
When iterating through this new structure:
1. For the first two iterations, `getInteriorWidth()` is called on Car objects, which in turn call their respective `getCabinWidth()` methods.
2. In the third iteration, `getInteriorWidth()` is called on the RacingCar object, which calls its own `getCockpitWidth()` method.

All calls work correctly due to our redesigned hierarchy. This allows us to iterate through any number of Vehicle-type objects without worrying about whether they are Car or RacingCar objects.

## Conclusion
What we demonstrated was our first example of applying the Liskov Substitution Principle by "breaking the hierarchy." 



--- 