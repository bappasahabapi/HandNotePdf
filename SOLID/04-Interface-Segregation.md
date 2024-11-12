# Interface Segregation Principle (ISP) - Overview

## Introduction
Hello and welcome to this session. In this session, we will look at the 'I' in SOLID, which is the fourth design principle. 'I' stands for the Interface Segregation Principle, often abbreviated as ISP.

## What Does ISP Say?
The Interface Segregation Principle states: **"No client should be forced to depend on methods it does not use."**

## Real-World Analogy
To explain this principle, let's start with a real-world analogy:

Imagine you are working in an office with around 200 employees. You have a variety of devices available for use, including printers, scanners, and fax machines. As a software developer, you need to represent these devices using object-oriented concepts in code and design interfaces to ensure uniformity among them.

### The All-in-One Device
You spot a multi-function all-in-one Xerox WorkCentre that has printing, scanning, copying, and faxing capabilities. You decide to create a common interface based on this device.

### Designing the Interface
You design an interface named **IMultiFunction**. The 'I' prefix is a convention used by some programmers to indicate interfaces. This interface defines methods for various functions:
- `print()` and `getPrintSpoolDetails()` for printing.
- `scan()` and `scanPhoto()` for scanning.
- `fax()` and `internetFax()` for fax-related functions.

### Implementing the Interface
You create a concrete class representing the Xerox WorkCentre that implements the IMultiFunction interface and successfully implement all six methods.

### The HP Device
Next, you encounter an HP printer-scanner device. While it can print and scan, it does not have fax capabilities. You implement the IMultiFunction interface but leave the fax methods unimplemented, providing blank implementations instead.

### The Canon Device
Continuing on, you find a Canon printer that only has printing functions. Again, you implement the IMultiFunction interface but leave all non-printing methods blank.

## Identifying the Problem
You start to see the issue: unimplemented methods are indicative of poor design. This scenario violates the Interface Segregation Principle because:

**"No client should be forced to depend on methods it does not use."**

### Consequences of Poor Design
Consider an employee portal application that needs to access these devices. If a programmer sees the `fax()` method on the Canon Device class and invokes it without realizing it's unimplemented, this can lead to runtime errors and broken functionality.

## Conclusion
This highlights why such designs are problematic and why the Interface Segregation Principle recommends avoiding them.

--- 

# 🔥04B-Interface Segregation Principle (ISP) - Problem Solving

## Introduction
Hello and welcome to this session. In this session, we will address the problem we identified in the previous session and explore how to solve it.

## Recap of the Problem
We left off with one generic interface, **IMultiFunction**, and three classes that implement it. However, not all methods make sense for all classes, leading to some blank method implementations.

## Solution: Splitting the Interface
The easiest way to resolve this issue is to split the large interface into smaller, more specific interfaces. 

### New Interfaces
We will create three new interfaces:
- **IPrint**: Contains the `print()` and `getPrintSpoolDetails()` methods.
- **IScan**: Contains the `scan()` and `photoScan()` methods.
- **IFax**: Contains the fax-related methods, `fax()` and `internetFax()`.

### Updating the Classes
Now, let's update the classes accordingly:
1. **Xerox WorkCentre**: This class will implement all three interfaces (IPrint, IScan, IFax) since it can perform all functions.
2. **HP Device Class**: This class will implement only the IPrint and IScan interfaces, allowing us to remove the blank implementations of the fax methods.
3. **Canon Device Class**: This class will implement only the IPrint interface, as it can only perform printing functions. We can remove all remaining blank implementations from this class.

### Resulting Clean Design
By implementing these changes, we have made the classes much cleaner. There are no more blank method implementations. If we package these classes as a library for other programmers, there is no ambiguity; they can only call the defined methods in each class.

## Conclusion
We have solved the problem by segregating the interfaces. This is one way to adhere to the Interface Segregation Principle.

Additionally, if you feel that there are certain functions common between a printer, scanner, and fax machine, you could create a parent interface with those common methods. The IPrint, IScan, and IFax interfaces could then extend this parent interface while adding their specific methods.

In our next session, we will discuss standard techniques for identifying violations of the Interface Segregation Principle and how this principle relates to other SOLID principles. 

Thank you for joining this session, and I'll see you in the next one!

--- 

Feel free to adjust any part of this text as needed!