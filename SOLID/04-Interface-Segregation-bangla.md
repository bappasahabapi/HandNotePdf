# 🔥04A-Interface Segregation Principle (ISP) - পরিচিতি

## ভূমিকা
 আমরা SOLID নীতির 'I' অর্থাৎ ইন্টারফেস সেগ্রেগেশন প্রিন্সিপল (Interface Segregation Principle) আলোচনা করব। এই নীতি অনুযায়ী, **"কোন ক্লায়েন্টকে এমন মেথডের ওপর নির্ভর করতে বাধ্য করা উচিত নয় যা সে ব্যবহার করে না।"**


- The Interface Segregation Principle states: **"No client should be forced to depend on methods it does not use."**

## বাস্তব জীবনের উদাহরণ

ধরি, আপনি একটি অফিসে কাজ করছেন যেখানে প্রায় ২০০ জন কর্মচারী রয়েছে। অফিসে বিভিন্ন প্রিন্টার, স্ক্যানার এবং ফ্যাক্স মেশিন রয়েছে। 

### ডিভাইসের প্রতিনিধিত্ব

আপনাকে এই ডিভাইসগুলোকে অবজেক্ট ওরিয়েন্টেড ধারণায় উপস্থাপন করতে বলা হয়েছে। আপনি একটি মাল্টি-ফাংশনাল ডিভাইস, যেমন Xerox WorkCentre দেখতে পান, যা প্রিন্টার, স্ক্যানার, কপিয়ার এবং ফ্যাক্স মেশিন সবকিছু একসাথে করে।

### ইন্টারফেস ডিজাইন

আপনি একটি ইন্টারফেস `IMultiFunction` ডিজাইন করেন:

```java
interface IMultiFunction {
    void print();
    void getPrintSpoolDetails();
    void scan();
    void scanPhoto();
    void fax();
    void internetFax();
}
```

এখন আপনি `XeroxWorkCentre` ক্লাস তৈরি করেন যা এই ইন্টারফেসটি বাস্তবায়ন করে:

```java
class XeroxWorkCentre implements IMultiFunction {
    @Override
    public void print() { /* Implementation */ }
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }
    @Override
    public void scan() { /* Implementation */ }
    @Override
    public void scanPhoto() { /* Implementation */ }
    @Override
    public void fax() { /* Implementation */ }
    @Override
    public void internetFax() { /* Implementation */ }
}
```

### অন্য ডিভাইসের সমস্যা

এখন আপনি HP প্রিন্টার-স্ক্যানার দেখতে পান। এটি `IMultiFunction` ইন্টারফেসটি বাস্তবায়ন করে:

```java
class HPPrinterScanner implements IMultiFunction {
    @Override
    public void print() { /* Implementation */ }
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }
    @Override
    public void scan() { /* Implementation */ }
    @Override
    public void scanPhoto() { /* Implementation */ }
    
    // ফ্যাক্স সম্পর্কিত মেথডগুলোর জন্য খালি বাস্তবায়ন
    @Override
    public void fax() {}
    @Override
    public void internetFax() {}
}
```

এখন আপনি একটি Canon ডিভাইস দেখেন যা শুধুমাত্র প্রিন্টিং ফাংশন সমর্থন করে:

```java
class CanonPrinter implements IMultiFunction {
    @Override
    public void print() { /* Implementation */ }
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }

    // অন্যান্য মেথডগুলোর জন্য খালি বাস্তবায়ন
    @Override
    public void scan() {}
    @Override
    public void scanPhoto() {}
    @Override
    public void fax() {}
    @Override
    public void internetFax() {}
}
```

### সমস্যা চিহ্নিতকরণ

এখন আপনি দেখতে পাচ্ছেন যে খালি বাস্তবায়নগুলো একটি ডিজাইন ত্রুটি নির্দেশ করছে। 

#### খালি বাস্তবায়নের সমস্যা

ধরি, আপনার অফিসে একটি কর্মচারী পোর্টাল অ্যাপ্লিকেশন রয়েছে যা এই ডিভাইসগুলোতে সরাসরি অ্যাক্সেস করতে চায়। 

যদি একজন প্রোগ্রামার `CanonPrinter` ক্লাসের `fax()` মেথড কল করে, সে জানে না যে এটি খালি। সে ধরে নেয় যে এটি কাজ করবে এবং কোডটি ব্যর্থ হবে।

## উপসংহার

এই কারণে ইন্টারফেস সেগ্রিগেশন প্রিন্সিপল এমন ডিজাইনগুলো এড়াতে সুপারিশ করে। 


---

# 🔥04B-Interface Segregation Principle (ISP) - সমাধান

## ভূমিকা

স্বাগতম! আজকের সেশনে আমরা আগের সেশনে চিহ্নিত সমস্যাগুলো সমাধান করব। আমরা দেখব কীভাবে একটি বড় ইন্টারফেসকে ছোট ছোট ইন্টারফেসে বিভক্ত করে ডিজাইন উন্নত করা যায়।

## সমস্যা পুনরুদ্ধার

আমাদের কাছে একটি সাধারণ `IMultiFunction` ইন্টারফেস এবং তিনটি ক্লাস রয়েছে যা এই ইন্টারফেসটি বাস্তবায়ন করে। কিন্তু, কিছু মেথড সব ক্লাসের জন্য প্রাসঙ্গিক নয়, তাই কিছু মেথডের খালি বাস্তবায়ন রয়েছে।

### সমাধানের পদ্ধতি

এখন, এই সমস্যার সহজ সমাধান হল বড় ইন্টারফেসটিকে ছোট ছোট ইন্টারফেসে বিভক্ত করা। 

### নতুন ইন্টারফেস ডিজাইন

আমরা `IMultiFunction` ইন্টারফেসটিকে তিনটি আলাদা ইন্টারফেসে বিভক্ত করব:

1. **IPrint**: প্রিন্টিং সম্পর্কিত মেথডগুলি ধারণ করবে।
2. **IScan**: স্ক্যানিং সম্পর্কিত মেথডগুলি ধারণ করবে।
3. **IFax**: ফ্যাক্স সম্পর্কিত মেথডগুলি ধারণ করবে।

```java
interface IPrint {
    void print();
    void getPrintSpoolDetails();
}

interface IScan {
    void scan();
    void scanPhoto();
}

interface IFax {
    void fax();
    void internetFax();
}
```

### ক্লাসগুলোর পরিবর্তন

#### Xerox WorkCentre

`XeroxWorkCentre` ক্লাস এখন সব তিনটি ইন্টারফেস বাস্তবায়ন করবে:

```java
class XeroxWorkCentre implements IPrint, IScan, IFax {
    @Override
    public void print() { /* Implementation */ }
    
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }
    
    @Override
    public void scan() { /* Implementation */ }
    
    @Override
    public void scanPhoto() { /* Implementation */ }
    
    @Override
    public void fax() { /* Implementation */ }
    
    @Override
    public void internetFax() { /* Implementation */ }
}
```

#### HP Printer-Scanner

`HPPrinterScanner` ক্লাস এখন `IPrint` এবং `IScan` ইন্টারফেসগুলি বাস্তবায়ন করবে:

```java
class HPPrinterScanner implements IPrint, IScan {
    @Override
    public void print() { /* Implementation */ }
    
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }
    
    @Override
    public void scan() { /* Implementation */ }
    
    @Override
    public void scanPhoto() { /* Implementation */ }
}
```

#### Canon Printer

`CanonPrinter` ক্লাস শুধুমাত্র `IPrint` ইন্টারফেসটি বাস্তবায়ন করবে:

```java
class CanonPrinter implements IPrint {
    @Override
    public void print() { /* Implementation */ }
    
    @Override
    public void getPrintSpoolDetails() { /* Implementation */ }
}
```

### ফলাফল

এখন আপনি দেখতে পাচ্ছেন যে ক্লাসগুলোর ডিজাইন অনেক পরিষ্কার হয়েছে। আর কোনো খালি বাস্তবায়ন নেই। যদি আমরা এই ক্লাসগুলোকে একটি লাইব্রেরি হিসেবে প্যাকেজ করি এবং অন্য প্রোগ্রামারদের কাছে পাঠাই, তাহলে কোনো বিভ্রান্তি থাকবে না। 

প্রোগ্রামাররা কেবলমাত্র সংজ্ঞায়িত মেথডগুলোই কল করতে পারবে, যা সঠিকভাবে কাজ করবে।

## উপসংহার

আমরা ইন্টারফেসগুলোকে সেগ্রিগেট করে সমস্যার সমাধান করেছি, যা ইন্টারফেস সেগ্রিগেশন প্রিন্সিপলের একটি উদাহরণ। 

যদি আপনি মনে করেন যে প্রিন্টার, স্ক্যানার এবং ফ্যাক্স মেশিনের মধ্যে কিছু সাধারণ ফাংশন রয়েছে, তবে আপনি একটি প্যারেন্ট ইন্টারফেস তৈরি করতে পারেন যা সাধারণ মেথডগুলো ধারণ করবে এবং `IPrint`, `IScan`, এবং `IFax` ইন্টারফেসগুলো সেই প্যারেন্ট ইন্টারফেস থেকে সম্প্রসারিত হবে।

---



# 🔥04C-স্ট্যান্ডার্ড কৌশল যা ISP লঙ্ঘনের চিহ্নিতকরণে সহায়ক হবে এবং দেখব কিভাবে ISP অন্যান্য SOLID নীতির সাথে সম্পর্কিত। 


## ভূমিকা

স্বাগতম! আজকের সেশনে, আমরা ইন্টারফেস সেগ্রিগেশন প্রিন্সিপল (ISP) লঙ্ঘনের কিছু সাধারণ সনাক্তকরণ কৌশল আলোচনা করব এবং এই নীতির অন্যান্য SOLID নীতির সাথে সম্পর্ক দেখব।

## ISP লঙ্ঘনের সনাক্তকরণ কৌশল

### ১. ফ্যাট ইন্টারফেস (Fat Interfaces)

ফ্যাট ইন্টারফেস বলতে বোঝায় যে ইন্টারফেসগুলির মধ্যে অনেক বেশি মেথড থাকে। উদাহরণস্বরূপ, আমাদের `IMultiFunction` ইন্টারফেসে ৬টি মেথড ছিল যা বিভিন্ন কার্যক্রম যেমন প্রিন্টিং, স্ক্যানিং এবং ফ্যাক্সিং পরিচালনা করে। 

এটি প্রায়শই ইন্টারফেস সেগ্রিগেশন প্রিন্সিপলের লঙ্ঘনের একটি সূচক।

### ২. কম কোহেশন (Low Cohesion)

যদি ইন্টারফেসের মধ্যে বিভিন্ন ধরনের কার্যক্রম একত্রিত করা হয়, তবে এটি কম কোহেশন নির্দেশ করে। উদাহরণস্বরূপ, `IMultiFunction` ইন্টারফেসের মধ্যে ফ্যাক্স এবং ফটোস্ক্যানের মতো দুটি সম্পূর্ণ ভিন্ন কার্যক্রম ছিল। 

একটি ফ্যাক্স মেশিন একটি প্রিন্টেড কাগজ টেলিফোন লাইনের মাধ্যমে পাঠায়, যেখানে অন্যটি একটি ছবির রঙ ডিজিটালি ক্যাপচার করে। 

এটি নির্দেশ করে যে `IMultiFunction` ইন্টারফেসে কম কোহেশন রয়েছে এবং এটি ISP লঙ্ঘনের সম্ভাবনা নির্দেশ করে।

### ৩. খালি বাস্তবায়ন (Empty Implementations)

যখন কিছু ক্লাস নির্দিষ্ট মেথডের বাস্তবায়ন খালি রাখতে বাধ্য হয়, তখন এটি ISP লঙ্ঘনের একটি সুস্পষ্ট চিহ্ন। খালি বাস্তবায়নগুলি ডিজাইন ত্রুটি নির্দেশ করে এবং এটি এড়ানো উচিত।

## ISP এর অন্যান্য SOLID নীতির সাথে সম্পর্ক

### একক দায়িত্ব নীতি (Single Responsibility Principle)

যখন আমরা ইন্টারফেসগুলোকে বিভক্ত করি, তখন প্রতিটি ইন্টারফেসের একটি নির্দিষ্ট দায়িত্ব থাকে। উদাহরণস্বরূপ:

- `IPrint` ইন্টারফেস শুধুমাত্র প্রিন্টিং সম্পর্কিত মেথড ধারণ করে।
- `IScan` ইন্টারফেস শুধুমাত্র স্ক্যানিং সম্পর্কিত মেথড ধারণ করে।

এভাবে, আমরা অজান্তেই একক দায়িত্ব নীতির অনুসরণ করছি।

### লিস্কভ সাবস্টিটিউশন নীতি (Liskov Substitution Principle)

ইন্টারফেসগুলোকে সেগ্রিগেট করার ফলে, আমরা লিস্কভ সাবস্টিটিউশন নীতিরও অনুসরণ করছি। উদাহরণস্বরূপ, যেখানে `IPrint` ইন্টারফেস ব্যবহৃত হচ্ছে, সেখানে `CanonPrinter` ক্লাসকে প্রতিস্থাপন করা সম্ভব।

## উপসংহার

আপনি দেখতে পাচ্ছেন যে SOLID নীতিগুলো পরস্পর সংযুক্ত এবং একে অপরকে সমর্থন করে। এই নীতিগুলো একসাথে কাজ করে একটি ভাল ডিজাইনের উদ্দেশ্য অর্জনে সহায়তা করে।

\