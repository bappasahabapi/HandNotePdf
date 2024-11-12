# 🔥05A-Dependency Inversion Principle (DIP) - পরিচিতি

## ভূমিকা

আজকের সেশনে আমরা SOLID নীতির 'D', অর্থাৎ ডিপেনডেন্সি ইনভারশন প্রিন্সিপল (Dependency Inversion Principle) আলোচনা করব। এই নীতির মূল বক্তব্য হল:

1. **"High Level Modules should not depend on Low Level Modules. Both should depend on abstractions."**
2. **"Abstractions should not depend on details. Details should depend on abstractions."**

এই দুটি অংশের মধ্যে মূল ধারণাটি একই, এবং আমরা শীঘ্রই এটি বুঝতে পারব।

## উচ্চ স্তরের এবং নিম্ন স্তরের মডিউল

### উদাহরণ

ধরি, আপনি একটি ই-কমার্স ওয়েবসাইট পরিচালনা করছেন। এই সফটওয়্যারটি জটিল এবং এতে হাজার হাজার লাইনের কোড রয়েছে। 

### উচ্চ স্তরের মডিউল

যদি আমরা একটি খুব উচ্চ স্তরের ডিজাইন মডেল তৈরি করি, তাহলে এটি তিনটি প্রধান ব্যবসায়িক কার্যক্রমে বিভক্ত হবে:

- **ProductCatalog**
- **PaymentProcessor**
- **CustomerProfile**

এই মডিউলগুলো ব্যবসায়িক কার্যক্রমের সাথে ঘনিষ্ঠভাবে সম্পর্কিত এবং এগুলোকে উচ্চ স্তরের মডিউল বলা হয়।

### নিম্ন স্তরের মডিউল

এখন, নিম্ন স্তরের মডিউলগুলোর দিকে তাকালে, আমরা দেখতে পাই:

- **SQLProductRepository**
- **GooglePayGateway**
- **WireTransfer**
- **EmailSender**
- **VoiceDialer**

এই মডিউলগুলো বাস্তবায়ন বিস্তারিত নিয়ে কাজ করে এবং তাই এগুলোকে নিম্ন স্তরের মডিউল বলা হয়।

### আপেক্ষিকতা

একটি গুরুত্বপূর্ণ দিক হল যে, একটি মডিউল উচ্চ বা নিম্ন স্তরের কিনা তা আপেক্ষিক। উদাহরণস্বরূপ, `Communication` মডিউল কখনও কখনও উচ্চ স্তরের এবং কখনও নিম্ন স্তরের হতে পারে, নির্ভর করে এটি কোন কনটেক্সটে ব্যবহৃত হচ্ছে।

## DIP লঙ্ঘন চিহ্নিতকরণ

### বর্তমান সম্পর্ক

আমরা যদি `ProductCatalog` থেকে `SQLProductRepository` এর দিকে একটি নির্ভরতা দেখি, তাহলে এটি নির্দেশ করে যে `ProductCatalog` একটি উচ্চ স্তরের মডিউল যা `SQLProductRepository` এর ওপর নির্ভরশীল। 

এটি DIP এর বিরুদ্ধে যায় কারণ উচ্চ স্তরের মডিউলগুলোকে নিম্ন স্তরের মডিউলের ওপর নির্ভরশীল হওয়া উচিত নয়।

### কোড উদাহরণ

```java
class SQLProductRepository {
    public List<String> getAllProductNames() {
        // SQL SELECT statement to fetch product names
    }
}

class ProductCatalog {
    private SQLProductRepository repository = new SQLProductRepository();

    public void displayProducts() {
        List<String> productNames = repository.getAllProductNames();
        // Display products
    }
}
```

এখানে `ProductCatalog` সরাসরি `SQLProductRepository` এর ওপর নির্ভরশীল, যা DIP লঙ্ঘন করে।

## DIP সমাধান

### ইন্টারফেস তৈরি করা

আমরা একটি ইন্টারফেস তৈরি করব, যা `ProductRepository` নামে পরিচিত হবে। `SQLProductRepository` এই ইন্টারফেসটি বাস্তবায়ন করবে।

```java
interface ProductRepository {
    List<String> getAllProductNames();
}

class SQLProductRepository implements ProductRepository {
    public List<String> getAllProductNames() {
        // SQL SELECT statement to fetch product names
    }
}
```

### ফ্যাক্টরি প্যাটার্ন ব্যবহার করা

এখন আমরা `SQLProductRepository` অবজেক্টটি `ProductCatalog` ক্লাসে সরাসরি ইনস্ট্যানশিয়েট করব না। বরং, একটি ফ্যাক্টরি ক্লাস ব্যবহার করব যা এই অবজেক্টটি তৈরি করবে।

```java
class RepositoryFactory {
    public static ProductRepository create() {
        return new SQLProductRepository();
    }
}

class ProductCatalog {
    private ProductRepository repository = RepositoryFactory.create();

    public void displayProducts() {
        List<String> productNames = repository.getAllProductNames();
        // Display products
    }
}
```

এখন, `ProductCatalog` ইন্টারফেসের ওপর নির্ভরশীল, যা আমাদের DIP অনুসরণ করছে।

## দ্বিতীয় অংশ: abstraction এবং Details

### পরিবর্তিত সম্পর্ক

এখন, `SQLProductRepository` ইন্টারফেসের মাধ্যমে `ProductCatalog` এর সাথে যুক্ত হয়েছে। 

এখন নিম্ন স্তরের মডিউল (যেমন `SQLProductRepository`) আবstraction (যেমন `ProductRepository`) এর ওপর নির্ভরশীল, যা DIP এর দ্বিতীয় অংশের সাথে সঙ্গতিপূর্ণ।

## উপসংহার

আমরা শুরুতে DIP লঙ্ঘনের একটি উদাহরণ দেখেছি এবং পরে কিভাবে কোড পুনর্গঠন করে এই নীতিটি অনুসরণ করা যায় তা শিখেছি। 

DIP আমাদের সফটওয়্যার ডিজাইনে নমনীয়তা এবং বজায় রাখার ক্ষমতা বৃদ্ধি করতে সহায়ক। 

---

# 🔥05Bডিপেন্ডেন্সি ইনজেকশন বুঝতে

ডিপেন্ডেন্সি ইনজেকশন (DI) একটি সফটওয়্যার ডিজাইন প্যাটার্ন যা ক্লাস এবং তাদের নির্ভরতাগুলির মধ্যে বিচ্ছিন্নতা তৈরি করে, যা আরও নমনীয় এবং রক্ষণাবেক্ষণযোগ্য কোডের দিকে নিয়ে যায়। DI এবং ডিপেন্ডেন্সি ইনভার্সন প্রিন্সিপল (DIP) এর মধ্যে পার্থক্য করা গুরুত্বপূর্ণ, যা SOLID নীতিগুলির একটি এবং উচ্চ স্তরের এবং নিম্ন স্তরের মডিউলের মধ্যে নির্ভরতাগুলি কমাতে লক্ষ্য রাখে।

### Key Concepts of Dependency Injection

1. **সংজ্ঞা**: ডিপেন্ডেন্সি ইনজেকশন হল একটি কৌশল যেখানে একটি অবজেক্ট (ক্লায়েন্ট) তার নির্ভরতাগুলি বাইরের উৎস থেকে গ্রহণ করে, বরং অভ্যন্তরীণভাবে সেগুলি তৈরি করে। এটি শিথিল সংযোগ প্রচার করে এবং কোডের পরীক্ষাযোগ্যতা ও রক্ষণাবেক্ষণযোগ্যতা বাড়ায়।

2. **দায়িত্বের পৃথকীকরণ**: DI ব্যবহার করে, নির্ভরতাগুলি তৈরি করার দায়িত্ব সেই ক্লাস থেকে আলাদা হয় যা সেগুলি ব্যবহার করে। এর মানে হল যে একটি ক্লাসকে তার নির্ভরতাগুলি কীভাবে ইনস্ট্যানশিয়েট করতে হবে তা জানার প্রয়োজন নেই, ফলে এর জটিলতা কমে যায়।

3. **ডিপেন্ডেন্সি ইনজেকশনের প্রকার**:
   - **Constructor Injection** নির্ভরতাগুলি ক্লাসের কনস্ট্রাক্টরের মাধ্যমে প্রদান করা হয়।
   - **Setter Injection**: নির্ভরতাগুলি সেটার মেথডের মাধ্যমে প্রদান করা হয়।
   - **Interface Injection**: একটি ইন্টারফেস ক্লায়েন্টে নির্ভরতাগুলি ইনজেক্ট করার জন্য একটি পদ্ধতি প্রদান করে।


### উদাহরণ পরিস্থিতি

ধরি, আমাদের একটি `ProductCatalog` ক্লাস আছে যা কাজ করার জন্য একটি `ProductRepository` প্রয়োজন। DI ছাড়া, `ProductCatalog` সরাসরি `SQLProductRepository` তৈরি করতে পারে, যা টাইট কাপলিংয়ের দিকে নিয়ে যায়:

```java
public class ProductCatalog {
    private SQLProductRepository repository;

    public ProductCatalog() {
        this.repository = new SQLProductRepository(); // টাইট কাপলিং
    }
}
```

ডিপেন্ডেন্সি ইনজেকশনের সাথে, আমরা `ProductCatalog` কে তার কনস্ট্রাক্টরে একটি `ProductRepository` গ্রহণ করতে পরিবর্তন করতে পারি:

```java
public class ProductCatalog {
    private ProductRepository repository;

    public ProductCatalog(ProductRepository repository) {
        this.repository = repository; // লুজ কাপলিং
    }
}
```

এখন, যখন আমরা `ProductCatalog` এর একটি উদাহরণ তৈরি করি, আমরা নির্ভরতা ইনজেক্ট করি:

```java
public class Main {
    public static void main(String[] args) {
        ProductRepository repository = new SQLProductRepository();
        ProductCatalog catalog = new ProductCatalog(repository);
    }
}
```

### ডিপেন্ডেন্সি ইনজেকশনের সুবিধা

- **লুজ কাপলিং**: ক্লাসগুলি কংক্রিট বাস্তবায়নের উপর কম নির্ভরশীল হয়, যা বাস্তবায়নগুলি পরিবর্তন করা সহজ করে তোলে।
- **পরীক্ষাযোগ্যতা**: পরীক্ষার সময় সহজেই নির্ভরতাগুলিকে মক বা স্টাব করা যায়, ফলে ইউনিট টেস্টগুলি বাইরের ফ্যাক্টরগুলির (যেমন ডেটাবেস বা ওয়েব সার্ভিস) উপর নির্ভরশীল হয় না।
- **রক্ষণাবেক্ষণযোগ্যতা**: সিস্টেমের এক অংশে পরিবর্তন (যেমন SQL থেকে NoSQL এ পরিবর্তন) অন্য অংশগুলিতে ন্যূনতম প্রভাব ফেলতে পারে।

### উপসংহার

ডিপেন্ডেন্সি ইনজেকশন একটি শক্তিশালী কৌশল যা কোডের মডুলারিটি এবং নমনীয়তা বাড়ায়। নির্ভরতাগুলির ইনস্ট্যানশিয়েশন এবং তাদের ব্যবহারের মধ্যে বিচ্ছিন্নতা তৈরি করে, ডেভেলপাররা এমন সিস্টেম তৈরি করতে পারেন যা পরিচালনা করা সহজ, পরীক্ষা করা সহজ এবং সম্প্রসারণযোগ্য। DI বোঝা এবং কার্যকরভাবে বাস্তবায়ন করা ভাল সফটওয়্যার আর্কিটেকচার এবং উন্নত উন্নয়ন অনুশীলনের দিকে নিয়ে যেতে পারে।