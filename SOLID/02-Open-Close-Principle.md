# 🔥02A-Open-Closed Principle (OCP)

The Open-Closed Principle (OCP) is one of the five SOLID principles of object-oriented design. It states that **software entities (classes, modules, functions, etc.) should be open for extension but closed for modification**. This means that you should be able to add new functionality to existing code without altering its source code.

### Key Concepts

- **Closed for Modification**: 
    - Existing code should not be changed when adding new features. This helps avoid introducing bugs and maintains the stability of the system.
    - **New features getting added to the software component should not have to modify the existing code.**
  
- **Open for Extension**: 
    - New functionalities can be added through new code, allowing the system to grow without modifying existing components.

### Real-World Analogy

A practical analogy to understand OCP is the **Nintendo Wii [Joy Stick] tool** gaming console:
- The Wii console comes with a basic controller and allows for various accessories (like the Wii Zapper and steering wheel) to be added without modifying the console itself.
- This design ensures that users can enhance their gaming experience without needing to alter the core system.

### Benefits of OCP

1. **Maintainability**: Reduces the risk of bugs by keeping existing code untouched.
2. **Scalability**: Facilitates easy addition of new features as requirements evolve.
3. **Reusability**: Encourages the reuse of existing modules and classes in different contexts.

### Implementation Example in Java

To illustrate OCP in practice, consider a scenario where you need to compare areas of different shapes:

#### Without OCP
```java
class Square {
    int height;
    int area() { return height * height; }
}

class OpenOpenExample {
    public int compareArea(Square a, Square b) {
        return a.area() - b.area();
    }
}
```
This approach requires modification if a new shape (like Circle) is introduced.

#### With OCP
By using an interface:
```java
interface Shape {
    int area();
}

class Circle implements Shape {
    int radius;
    int area() { return (int)(Math.PI * radius * radius); }
}

class Square implements Shape {
    int height;
    int area() { return height * height; }
}

class OpenClosedExample {
    public int compareArea(Shape a, Shape b) {
        return a.area() - b.area();
    }
}
```
In this design, you can add new shapes without modifying existing code, adhering to the Open-Closed Principle.

### Conclusion



The Open-Closed Principle encourages developers to design systems that are robust and adaptable.

By focusing on extending functionality rather than modifying existing code, developers can create more maintainable and scalable applications. 

In future sessions, we will explore practical coding examples that demonstrate OCP in action.

# 🔥02B-ওপেন-ক্লোজড প্রিন্সিপল: কোড উদাহরণ

### পরিচিতি
আমরা একটি কোড উদাহরণ দেখব যা এই নীতির বাস্তবায়নকে চিত্রিত করবে।

### প্রেক্ষাপট

ধরি, **One State** একটি বীমা কোম্পানি যা মূলত স্বাস্থ্য বীমার সাথে জড়িত। তাদের বীমা গণনা একটি Java লাইব্রেরিতে কোড করা হয়েছে। 

#### কোড স্নিপেট

এখানে একটি কোড স্নিপেট রয়েছে যা প্রিমিয়াম ডিসকাউন্ট ক্যালকুলেশন দেখায়:

```java
class InsurancePremiumDiscountCalculator {
    public double calculatePremiumDiscountPercent(HealthInsuranceCustomerProfile profile) {
        if (profile.isLoyalCustomer()) {
            return 10.0; // Loyal customer discount
        }
        return 0.0; // No discount
    }
}
```

- **HealthInsuranceCustomerProfile** ক্লাসে একটি `isLoyalCustomer()` পদ্ধতি রয়েছে যা সত্য (true) ফেরত দেয় যদি গ্রাহক একজন বিশ্বস্ত গ্রাহক হয়, অন্যথায় মিথ্যা (false) ফেরত দেয়।

### নতুন চ্যালেঞ্জ

ধরি, One State কোম্পানি একটি নতুন গাড়ি বীমা কোম্পানি অধিগ্রহণ করে এবং তাদের ট্যাগলাইন পরিবর্তন করে: "আপনার স্বাস্থ্য এবং গাড়ি বীমার জন্য সবকিছু"। এখন আমাদের গাড়ি বীমার ডিসকাউন্টও সমর্থন করতে হবে।

#### নতুন ক্লাস যোগ করা

আমরা একটি নতুন ক্লাস যুক্ত করি: **VehicleInsuranceCustomerProfile**। এটি **HealthInsuranceCustomerProfile** এর মতো এবং এর `isLoyal()` পদ্ধতি রয়েছে।

```java
class VehicleInsuranceCustomerProfile {
    public boolean isLoyal() {
        // Logic to determine loyalty
    }
}
```

### সমস্যা

এখন আমাদের **InsurancePremiumDiscountCalculator** ক্লাসটি পরিবর্তন করতে হবে, কারণ এর `calculate` পদ্ধতি বর্তমানে শুধুমাত্র **HealthInsuranceCustomerProfile** অবজেক্ট গ্রহণ করে। 

#### OCP লঙ্ঘন

- নতুন বৈশিষ্ট্য যোগ করার জন্য আমাদের বিদ্যমান কোডে পরিবর্তন করতে হচ্ছে, যা ওপেন-ক্লোজড প্রিন্সিপলের বিরুদ্ধে যায়।
- যদি আমরা বাড়ির বীমাও সমর্থন করতে চাই, তাহলে আবার কোড পরিবর্তন করতে হবে।

### ডিজাইন পুনর্গঠন

আমরা আমাদের ডিজাইনটি পুনর্গঠন করি:

1. **CustomerProfile** নামে একটি নতুন ইন্টারফেস তৈরি করি।
2. এই ইন্টারফেসে একটি মাত্র পদ্ধতি থাকবে: `isLoyalCustomer()`।
3. উভয় **HealthInsuranceCustomerProfile** এবং **VehicleInsuranceCustomerProfile** ক্লাস এই সাধারণ ইন্টারফেসটি বাস্তবায়ন করবে।

```java
interface CustomerProfile {
    boolean isLoyalCustomer();
}

class HealthInsuranceCustomerProfile implements CustomerProfile {
    public boolean isLoyalCustomer() {
        // Logic for health insurance loyalty
    }
}

class VehicleInsuranceCustomerProfile implements CustomerProfile {
    public boolean isLoyalCustomer() {
        // Logic for vehicle insurance loyalty
    }
}
```

### নতুন ক্যালকুলেটর ক্লাস

এখন **InsurancePremiumDiscountCalculator** ক্লাসের পদ্ধতি পরিবর্তন করি:

```java
class InsurancePremiumDiscountCalculator {
    public double calculatePremiumDiscountPercent(CustomerProfile profile) {
        if (profile.isLoyalCustomer()) {
            return 10.0; // Loyal customer discount
        }
        return 0.0; // No discount
    }
}
```

### সুবিধা

- এখন আমরা যদি বাড়ির বীমার জন্য একটি নতুন ক্লাস তৈরি করি, যেমন **HomeInsuranceCustomerProfile**, এটি শুধুমাত্র **CustomerProfile** ইন্টারফেসটি বাস্তবায়ন করবে।
- আমাদের **InsurancePremiumDiscountCalculator** ক্লাসে কোনো পরিবর্তন করতে হবে না।

### উপসংহার

আমরা দেখেছি কিভাবে ডিজাইন পুনর্গঠন করে ওপেন-ক্লোজড প্রিন্সিপল অনুযায়ী কাজ করা যায়। প্রথমে ডিজাইনটি OCP অনুসরণ করছিল না, কিন্তু পুনর্গঠনের মাধ্যমে এটি OCP-র সাথে সামঞ্জস্যপূর্ণ হয়ে উঠেছে। 

এখন ডিজাইনটি ভবিষ্যতের এক্সটেনশনের জন্য আরও শক্তিশালী এবং কার্যকরী। 

এটি ছিল ওপেন-ক্লোজড প্রিন্সিপলের একটি উদাহরণ। 

# 🔥02C-Key Takeaways from the Open-Closed Principle Code Example

### Introduction

We will summarize the key takeaways from the previous code example that illustrated the Open-Closed Principle (OCP). 

### Principal Benefits of the New Design

1. **Ease of Adding New Features**
   - The primary benefit of adhering to the Open-Closed Principle is the ease with which new features can be added to the system.
   - This ease translates into **cost savings** for development and testing.

#### Cost Savings Explained

- If we do not follow OCP, adding new features **often requires modifying existing code.** 
- Each modification increases the time spent on testing and quality assurance to ensure that no new bugs are introduced into the existing codebase.
- In contrast, when following OCP, testing new code is simpler and less time-consuming than running a full regression test suite on existing code.
- This principle is well-known among QA testers, who emphasize its importance in maintaining software quality.

### Additional Benefits

2. **Decoupling of Components**
   - By redesigning our system to conform to OCP, we inadvertently achieved a higher degree of **loose coupling** between components.
   - This decoupling aligns with the **Single Responsibility Principle (SRP)**, as each component now has a clearer purpose and responsibility.

### Interconnectedness of SOLID Principles

- It is crucial to understand that the SOLID principles are intertwined and interdependent. 
- They are most effective when applied together, providing a holistic approach to software design.

### Cautionary Note

- **Do Not Follow OCP Blindly**: 
  - While OCP is beneficial, blindly applying it can lead to an excessive number of classes, complicating your overall design.
  - For instance, if you need to fix a bug and believe that modifying existing code is necessary for an effective fix, do so without hesitation.
  - Avoid overhauling your design solely for bug fixes unless you notice recurring issues that could be mitigated by a redesign.

### Subjective Application of OCP

- Deciding when and where to apply the Open-Closed Principle is subjective rather than objective. 
- Use your judgment based on the specific context of your project.

### Conclusion

- Re-designed code to make it follow OCP
- Following OCP can lead to cost benifits in the long run.
- OPC and SRP can work together to achieve a better design.
- Do not apply the OCPblindly and introduce unwanted complexity to your code.

The lessons learned from our code example related to the Open-Closed Principle. Understanding these key takeaways will help you apply OCP effectively in your future designs. 

