# 🔥03A-Liskov Substitution Principle (LSP)

## ভূমিকা

লিস্কভ সাবস্টিটিউশন প্রিন্সিপল (Liskov Substitution Principle) একটি গুরুত্বপূর্ণ নীতি যা অবজেক্ট ওরিয়েন্টেড প্রোগ্রামিংয়ের ভিত্তিতে তৈরি। এই নীতির মূল বক্তব্য হল: 

- **"অবজেক্টগুলোকে তাদের সাবটাইপ দিয়ে প্রতিস্থাপন করা উচিত, যাতে প্রোগ্রামের সঠিকতা প্রভাবিত না হয়।"**

- **"Objects should be replaceable with their subtypes without affecting the correctness of the program."**

## ইনহেরিটেন্স এবং 'Is-A' সম্পর্ক

**Inheritance** হল অবজেক্ট ওরিয়েন্টেড প্রোগ্রামিংয়ের একটি মৌলিক বৈশিষ্ট্য, যা 'Is-A' সম্পর্ক হিসেবে পরিচিত। 

### উদাহরণ ১: গাড়ি ক্লাস

ধরি, আমাদের একটি `Car` ক্লাস আছে। 

```java
class Car {
    // গাড়ির বৈশিষ্ট্য এবং পদ্ধতি
}
```

এখন, একটি `Hatchback` ক্লাস তৈরি করি যা `Car` ক্লাস থেকে সম্প্রসারিত।

```java
class Hatchback extends Car {
    // হ্যাচব্যাকের বৈশিষ্ট্য এবং পদ্ধতি
}
```

এখন আমরা বলতে পারি, "হ্যাচব্যাক একটি গাড়ি।"

### উদাহরণ ২: পাখি ক্লাস

আমাদের একটি `Bird` ক্লাস আছে এবং `Ostrich` ক্লাসটি `Bird` থেকে সম্প্রসারিত।

```java
class Bird {
    void fly() {
        // উড়ার পদ্ধতি
    }
}

class Ostrich extends Bird {
    @Override
    void fly() {
        // এখানে কিছুই নেই, কারণ উড়তে পারে না
    }
}
```

এখন আমরা বলতে পারি, "অস্ট্রিচ একটি পাখি।"

### উদাহরণ ৩: জ্বালানি ক্লাস

আমরা `Fuel` এবং `Gasoline` এর মধ্যে সম্পর্কও দেখতে পারি।

```java
class Fuel {
    // জ্বালানির বৈশিষ্ট্য এবং পদ্ধতি
}

class Gasoline extends Fuel {
    // গ্যাসোলিনের বৈশিষ্ট্য এবং পদ্ধতি
}
```

### Problems
At first glance, these examples seem perfect, but there are hidden problems with this approach. Particularly, the second example has an issue.

### Analysis
- An ostrich is a bird, but it cannot fly.
- We have a **Bird** class with a `fly` method.
- When the Ostrich class extends the Bird class, it overrides the `fly` method and leaves it unimplemented.

### অস্ট্রিচের সমস্যা

যদিও অস্ট্রিচ একটি পাখি, কিন্তু এটি উড়তে পারে না। 

যখন আমরা `Bird` ক্লাসে `fly()` মেথড তৈরি করি, তখন `Ostrich` ক্লাসে এই মেথডটি অপ্রয়োজনীয় হয়ে পড়ে। 

```java
class Ostrich extends Bird {
    @Override
    void fly() {
        // এখানে কিছুই নেই, কারণ অস্ট্রিচ উড়তে পারে না
    }
}
```

অর্থাৎ, যদি আমরা `Bird` অবজেক্টের জায়গায় `Ostrich` অবজেক্ট ব্যবহার করি এবং কেউ `fly()` মেথড কল করে, তাহলে আমাদের প্রোগ্রাম ব্যর্থ হবে।

## লিস্কভ প্রিন্সিপলের গুরুত্ব

লিস্কভ সাবস্টিটিউশন প্রিন্সিপল আমাদের বলে যে:

> "অবজেক্টগুলোকে তাদের সাবটাইপ দিয়ে প্রতিস্থাপন করা উচিত, যাতে প্রোগ্রামের সঠিকতা প্রভাবিত না হয়।"

এটি 'Is-A' সম্পর্কের চেয়ে বেশি কঠোর পরীক্ষা দাবি করে। 

### সঠিক অ্যাবস্ট্রাকশন


লিস্কভের দৃষ্টিভঙ্গি হল:



> "যদি এটি একটি হাঁসের মতো দেখায় এবং হাঁসের মতো ডাক দেয় কিন্তু ব্যাটারি প্রয়োজন হয়, তাহলে আপনার সম্ভবত ভুল অ্যাবস্ট্রাকশন হয়েছে!"


### Liskov's Perspective
The Liskov Principle requires us to move away from the 'is-a' way of thinking. A popular saying associated with this principle is:

**"If it looks like a duck and quacks like a duck but needs batteries, you probably have the wrong abstraction!"**

Instead, Liskov's way of thinking should be: **"Objects should be replaceable with their subtypes without affecting the correctness of the program."**

## উপসংহার

এখন পর্যন্ত আমরা লিস্কভ সাবস্টিটিউশন প্রিন্সিপলের একটি উচ্চ স্তরের পরিচয় পেয়েছি।


--- 

# 🔥03B-Liskov Substitution Principle (LSP) - গভীর বিশ্লেষণ

## ভূমিকা

স্বাগতম! আজকের সেশনে আমরা লিস্কভ সাবস্টিটিউশন প্রিন্সিপল (LSP) গভীরভাবে পরীক্ষা করব। আমরা দুটি উদাহরণ নেব এবং প্রতিটির ডিজাইন ত্রুটি বিশ্লেষণ করব। পরে, আমরা LSP প্রয়োগ করে ডিজাইন উন্নত করব।

## উদাহরণ ১: গাড়ি এবং রেসিং গাড়ি

### সমস্যা চিহ্নিতকরণ

আমাদের কাছে একটি সাধারণ `Car` ক্লাস আছে এবং একটি `RacingCar` ক্লাস যা `Car` থেকে সম্প্রসারিত। 

```java
class Car {
    int getCabinWidth() {
        // গাড়ির কেবিনের প্রস্থ ফেরত দেয়
        return 50; // উদাহরণস্বরূপ
    }
}

class RacingCar extends Car {
    @Override
    int getCabinWidth() {
        // এখানে কিছুই নেই, কারণ রেসিং গাড়ির কেবিন নেই
    }
}
```

রেসিং গাড়ির জন্য, কেবিনের পরিবর্তে "ককপিট" থাকে। তাই, `RacingCar` ক্লাসে `getCabinWidth()` মেথডটি অপ্রয়োজনীয় হয়ে পড়ে।

### সমস্যা উদ্ভব

এখন, যদি আমরা `CarUtils` ক্লাসে তিনটি গাড়ির অবজেক্ট তৈরি করি:

```java
class CarUtils {
    List<Car> myCars = new ArrayList<>();
    
    void addCars() {
        myCars.add(new Car());
        myCars.add(new Car());
        myCars.add(new RacingCar());
    }
    
    void printCabinWidths() {
        for (Car car : myCars) {
            System.out.println(car.getCabinWidth());
        }
    }
}
```

এখন, প্রথম দুটি অবজেক্টের জন্য `getCabinWidth()` মেথড কাজ করবে, কিন্তু তৃতীয় অবজেক্টের জন্য এটি ব্যর্থ হবে কারণ `RacingCar` ক্লাসে মেথডটি অপ্রয়োগিত।

### সমাধান

এই সমস্যার সমাধানের জন্য আমাদের ইনহেরিটেন্স ভেঙে দিতে হবে। 

#### নতুন শ্রেণী তৈরি করা

আমরা একটি নতুন ক্লাস তৈরি করব যা `Vehicle` নামে পরিচিত। এটি সকল ধরনের পরিবহন মাধ্যমকে উপস্থাপন করবে।

```java
class Vehicle {
    int getInteriorWidth() {
        // একটি সাধারণ অভ্যন্তরীণ প্রস্থ ফেরত দেয়
        return 0; // উদাহরণস্বরূপ
    }
}

class Car extends Vehicle {
    @Override
    int getInteriorWidth() {
        return getCabinWidth();
    }
    
    int getCabinWidth() {
        return 50; // উদাহরণস্বরূপ
    }
}

class RacingCar extends Vehicle {
    @Override
    int getInteriorWidth() {
        return getCockpitWidth();
    }
    
    int getCockpitWidth() {
        return 40; // উদাহরণস্বরূপ
    }
}
```

### আপডেটেড কার ইউটিলস ক্লাস

এখন `CarUtils` ক্লাসটিকে `VehicleUtils` বলা হবে:

```java
class VehicleUtils {
    List<Vehicle> myVehicles = new ArrayList<>();
    
    void addVehicles() {
        myVehicles.add(new Car());
        myVehicles.add(new Car());
        myVehicles.add(new RacingCar());
    }
    
    void printInteriorWidths() {
        for (Vehicle vehicle : myVehicles) {
            System.out.println(vehicle.getInteriorWidth());
        }
    }
}
```

### ফলাফল

এখন, যখন আমরা `printInteriorWidths()` মেথড কল করি, প্রথম দুটি অবজেক্টের জন্য `getInteriorWidth()` মেথড কল হবে এবং তারা তাদের নিজস্ব কেবিন প্রস্থ ফেরত দেবে। তৃতীয় অবজেক্টের জন্যও একইভাবে কাজ করবে, কিন্তু এটি ককপিট প্রস্থ ফেরত দেবে।

## উপসংহার

আমরা প্রথম উদাহরণে Liskov Substitution Principle প্রয়োগ করে ডিজাইন উন্নত করেছি। এই পদ্ধতিটি "হায়ারার্কি ভাঙা" নামে পরিচিত।  [ "breaking the hierarchy." ]

---

# 🔥03C-Liskov Substitution Principle (LSP) - দ্বিতীয় উদাহরণ

## ভূমিকা

এই সেশনে, আমরা লিস্কভ সাবস্টিটিউশন প্রিন্সিপল (LSP) প্রয়োগের একটি দ্বিতীয় উদাহরণ দেখব। আমরা একটি সাধারণ `Product` ক্লাস এবং একটি `InHouseProduct` ক্লাস ব্যবহার করব। এই উদাহরণটি ই-কমার্স সাইটের দৃষ্টিকোণ থেকে দেখা হবে, যেমন Amazon।

## উদাহরণ: পণ্য এবং ইন-হাউস পণ্য

### প্রাথমিক ডিজাইন

আমাদের কাছে একটি `Product` ক্লাস আছে যা ডিসকাউন্ট ভেরিয়েবল এবং একটি `getDiscount()` মেথড ধারণ করে। 

```java
class Product {
    double discount = 20; // বেস ডিসকাউন্ট 20%

    double getDiscount() {
        return discount; // ডিসকাউন্ট ফেরত দেয়
    }
}

class InHouseProduct extends Product {
    void applyExtraDiscount() {
        discount *= 1.5; // ডিসকাউন্ট 1.5 গুণ বৃদ্ধি
    }
}
```

### সমস্যা চিহ্নিতকরণ

এখন, আমরা একটি `PricingUtils` ক্লাস তৈরি করি যা তিনটি অবজেক্ট তৈরি করে: দুটি সাধারণ পণ্য এবং একটি ইন-হাউস পণ্য। 

```java
class PricingUtils {
    List<Product> products = new ArrayList<>();

    void addProducts() {
        products.add(new Product());
        products.add(new Product());
        products.add(new InHouseProduct());
    }

    void printDiscounts() {
        for (Product product : products) {
            if (product instanceof InHouseProduct) {
                ((InHouseProduct) product).applyExtraDiscount(); // ইন-হাউস পণ্যের জন্য অতিরিক্ত ডিসকাউন্ট প্রয়োগ
            }
            System.out.println(product.getDiscount()); // ডিসকাউন্ট প্রিন্ট
        }
    }
}
```

### ডিজাইন ত্রুটি

এই ডিজাইনে, `PricingUtils` ক্লাসটি `instanceof` অপারেটর ব্যবহার করে অবজেক্টের টাইপ পরীক্ষা করছে। এটি Liskov Substitution Principle এর বিরুদ্ধে যায়, কারণ আমাদের সব পণ্যকে `Product` অবজেক্ট হিসেবে পরিচালনা করা উচিত।

## সমাধান

### কোড পুনর্গঠন

আমরা `InHouseProduct` ক্লাসে `getDiscount()` মেথডটি ওভাররাইড করব যাতে এটি অতিরিক্ত ডিসকাউন্ট প্রয়োগ করে।

```java
class InHouseProduct extends Product {
    @Override
    double getDiscount() {
        applyExtraDiscount(); // অতিরিক্ত ডিসকাউন্ট প্রয়োগ
        return discount; // নতুন ডিসকাউন্ট ফেরত দেয়
    }
}
```

### আপডেটেড প্রাইজিং ইউটিলস ক্লাস

এখন `PricingUtils` ক্লাসে `instanceof` চেক বাদ দেওয়া হবে:

```java
class PricingUtils {
    List<Product> products = new ArrayList<>();

    void addProducts() {
        products.add(new Product());
        products.add(new Product());
        products.add(new InHouseProduct());
    }

    void printDiscounts() {
        for (Product product : products) {
            System.out.println(product.getDiscount()); // ডিসকাউন্ট প্রিন্ট
        }
    }
}
```

### ফলাফল

এখন, যখন আমরা `printDiscounts()` মেথড কল করি, এটি সব পণ্যকে তাদের নিজস্ব ডিসকাউন্ট ফেরত দেবে, এবং ইন-হাউস পণ্যের জন্য অতিরিক্ত ডিসকাউন্টও প্রয়োগ হবে। এইভাবে, আমরা Liskov Substitution Principle এর পরীক্ষায় সফল হয়েছি।

## উপসংহার

আমরা দুটি সমস্যা সমাধানের প্যাটার্ন দেখেছি। প্রথম উদাহরণের সমাধান ছিল "হায়ারার্কি ভাঙা", এবং দ্বিতীয় উদাহরণের সমাধান ছিল "Tell, Don't Ask" নীতি অনুসরণ করা। 

