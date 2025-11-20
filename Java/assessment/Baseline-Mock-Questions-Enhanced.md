# Baseline Mock Assessment - Enhanced (Deep Testing)

## Instructions
- **Total Time:** 2 hours (or take it in parts - Part 1, Part 2, Part 3 separately)
- **No Google/Books/IDE** - Test your actual knowledge
- **Be honest** - Mark "Don't know" if unsure (this helps identify gaps)
- **Note gaps** - This is more valuable than guessing

---

## Part 1: Core Java Theory - Detailed (60 mins)

### Section A: Fundamental Concepts (10 questions)

#### 1. OOP Concepts
**Q:** What's the difference between abstraction and encapsulation? Give examples.

**Your Answer:**
->Hiding direct access to the user using an access modifier provide access using public method like getter and setter method it is encapulation.

For example user entity has id username and password  and they are not giving direct access uisng private access modifier instade of  thay are giving public getter and settesr method to user their 
property.

->Hiding implementation of the feature and providing only the necessary thing which only exposing thos feature which actualy require. what it is doing now how it is doing.

For example, if user tries to withdrow money than he goes to atm insert card pin and amount everything they need to do automaticly user don't need to worry about how atm is giving our money it's perfect example of abstraction.

-Encapulation is used at design level and abstraction is used at implementation level.
-In a real scenario they both are using together 



---

#### 2. Collections Deep Dive
**Q:** How does HashMap work internally in Java? Explain:
- Bucket structure
- Hash collision handling
- Time complexity for operations
- What happens when load factor is exceeded?

**Your Answer:**
Know the answer partially

---

#### 3. Exception Handling
**Q:** What's the difference between checked and unchecked exceptions? When would you use each? Also explain try-with-resources.

**Your Answer:**
checked exception is compile time and unchecked exception are runtine 
now the answer partially

---

#### 4. Generics & Type Erasure
**Q:** What is type erasure? Explain with an example. Also explain PECS (Producer Extends, Consumer Super).


**Your Answer:**
Don't know 
---

#### 5. String & Memory
**Q:** What's the difference between `==` and `.equals()` in Java? Explain String pool concept. What's the output?
```java
String s1 = "Java";
String s2 = new String("Java");
System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

**Your Answer:**
false
true

---

#### 6. Inheritance & Polymorphism
**Q:** What is method overriding vs overloading? Explain runtime vs compile-time polymorphism.

**Your Answer:**
same name with different paramanet is overloading 
base class have some method and child class is overridind it's implementation than it is method overriding 
Method overloading is compile time polymorphysm
and method overrriding is runtime polymorphysm 


---

#### 7. Threading Basics
**Q:** What's the difference between `Thread` and `Runnable`? How do you create threads? Explain thread lifecycle.

**Your Answer:**
Both are used to create thred but we can only extend onlt thread but when you want to use multiple implementation than you can use runnable interface 
Not sure properly but i know  that 
---

#### 8. JVM Memory Model
**Q:** What is the JVM? Explain heap vs stack memory. What goes where? Explain PermGen/Metaspace.

**Your Answer:**
Not sure need to understand ddeeply 

---

#### 9. Access Modifiers
**Q:** Explain all access modifiers (private, protected, default, public). When to use each?

**Your Answer:**
Know that but need to understnd and revised more to quick anser

---

#### 10. Design Patterns
**Q:** Name and explain 3 design patterns you've used or heard of. Give use cases.

**Your Answer:**
used lot's of design pater but don't know whic patter is that 

---

## Part 2: Rapid-Fire Round (20 mins - Quick Answers)

**Instructions:** Answer these quickly (1-2 lines max). Test your instant recall.

1. **What is the difference between `ArrayList` and `LinkedList`?**
   Your Answer: Linear index based collection is used when you want to access using 
   index and Linkedlist is storing previous and next node address 
   When you want to search index base than it is used 
   When you want to higlt insert and delete from first and end that it is used  

2. **What is the default size of ArrayList in Java?**
   Your Answer: 15

3. **Can we use `synchronized` keyword with variables?**
   Your Answer: No It's used with method

4. **What is the difference between `String`, `StringBuilder`, and `StringBuffer`?**
   Your Answer: string is immutable and string builder and string buffer is mutable 
   string builder is not thread safe 
   string buffer is thread safe 

5. **What is `final` keyword used for in Java?**
   Your Answer: when you use that than it variable cannot be change cannot overide function and cannot extend class when we use that with variable class and function 

6. **What is `static` keyword? Can static methods access instance variables?**
   Your Answer: you can access using clsaa don't require to create object of it it represent class not object

7. **What is `volatile` keyword? When to use it?**
   Your Answer: Don't know properly praitially know

8. **What is the difference between `==` and `.equals()` for objects?**
   Your Answer: == chak the reference  and equals check the value 

9. **What is `transient` keyword?**
   Your Answer: Don't know properly

10. **What is `this` and `super` in Java?**
    Your Answer: this is used to reference class property and super is use to reference baase class property mean parent class property

11. **What is the difference between `final`, `finally`, and `finalize`?**
    Your Answer: final is used wirh variable method and class to filen that value class and method cnnot change value cannot override value and cannot extend value 
    finallly is used with try catch block to execute block either exceptio occure or not
    finalize don't know  
12. **Can we override a static method?**
    Your Answer: no

13. **Can we override a private method?**
    Your Answer: no

14. **What is autoboxing and unboxing?**
    Your Answer: Don't know

15. **What is the difference between `HashMap` and `Hashtable`?**
    Your Answer: Don't know

16. **What is `ConcurrentHashMap`? How is it different from `HashMap`?**
    Your Answer: Concurrent HashMap is thread safe, and hashmap is not thread safe 

17. **What is `Comparator` vs `Comparable`?**
    Your Answer: both are functional interface Comparator is used to compare multiple value and comparable is used with to cocmpare single value 

18. **What is Java 8 Stream API? Give one example.**
    Your Answer: Know but need to practice 

19. **What is `Optional` in Java 8? When to use it?**
    Your Answer: it is used to avoid nul pointer excpeiton 

20. **What is the difference between `ExecutorService` and `ForkJoinPool`?**
    Your Answer: don't know 

---

## Part 3: Code Analysis & Deep Questions (30 mins)

### Question 1: Code Output Prediction
**What is the output of this code? Explain why.**

```java
class Parent {
    void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void display() {
        System.out.println("Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();
        p.display();
    }
}
```

**Your Answer:** 
Output: Child
Explanation: creation object of parent to referencing chilf class that's why

---

### Question 2: Memory & Garbage Collection
**Explain when these objects become eligible for GC:**

```java
public class Test {
    public static void main(String[] args) {
        Test t1 = new Test();
        Test t2 = new Test();
        t1 = t2;
        t2 = null;
        // When is t1 eligible for GC?
    }
}
```

**Your Answer:**
don't know 
---

### Question 3: Thread Safety
**Is this code thread-safe? If not, how would you fix it?**

```java
public class Counter {
    private int count = 0;
    
    public void increment() {
        count++;
    }
    
    public int getCount() {
        return count;
    }
}
```

**Your Answer:**
Thread-safe? Yes/No: no 
Fix: you can use synchronized key word

---

### Question 4: Exception Handling
**What happens in this code? Will finally block execute?**

```java
public class Test {
    public static void main(String[] args) {
        try {
            System.out.println("Try");
            return;
        } catch (Exception e) {
            System.out.println("Catch");
        } finally {
            System.out.println("Finally");
        }
    }
}
```

**Your Answer:**
Output: ___________Try finally
Explanation: first try block will execute and than finally 
if exxception occue that try catch andthan finally block execute

---

### Question 5: Collections Deep Dive
**What is the issue with this code? How to fix?**

```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
for (String item : list) {
    if (item.equals("A")) {
        list.remove(item);  // What happens?
    }
}
```

**Your Answer:**
Issue: Modification exception 
Fix: ___________

---

### Question 6: Generics Wildcards
**What is the output? Explain.**

```java
List<? extends Number> numbers = new ArrayList<Integer>();
numbers.add(10);  // Compile error?
```

**Your Answer:**
Error? Yes/No: ___________Dob't know 
Reason: ___________

---

## Part 4: Concurrency Deep Dive (20 mins)

1. **What is `synchronized` vs `ReentrantLock`? When to use which?**
   Your Answer: ___________

2. **What is deadlock? How to prevent it?**
   Your Answer: ___________

3. **Explain `wait()`, `notify()`, and `notifyAll()`. Which thread class do they belong to?**
   Your Answer: ___________

4. **What is `ExecutorService`? How is it better than creating threads manually?**
   Your Answer: ___________

5. **What is `CompletableFuture`? Give a use case.**
   Your Answer: ___________

6. **What is thread-local storage? Explain `ThreadLocal`.**
   Your Answer: ___________

7. **What is the difference between `CyclicBarrier` and `CountDownLatch`?**
   Your Answer: ___________

8. **What is race condition? Give an example.**
   Your Answer: ___________

---

## Part 5: Spring Boot & JPA (30 mins)

*Note: If you haven't used Spring Boot recently, mark "Need to revise"*

1. **What is Spring Boot? How is it different from Spring Framework?**
   Your Answer: ___________

2. **What is Dependency Injection? Explain with example.**
   Your Answer: ___________

3. **What are Spring Bean scopes? Explain singleton vs prototype.**
   Your Answer: ___________

4. **What is `@Autowired`? When does it fail?**
   Your Answer: ___________

5. **What is JPA? Difference between JPA and Hibernate?**
   Your Answer: ___________

6. **Explain `@Entity`, `@Table`, `@Id`, `@GeneratedValue`.**
   Your Answer: ___________

7. **What is N+1 problem in JPA? How to solve it?**
   Your Answer: ___________

8. **What is transaction propagation? Explain `REQUIRED` vs `REQUIRES_NEW`.**
   Your Answer: ___________

9. **What is `@RestController` vs `@Controller`?**
   Your Answer: ___________

10. **What is Spring Security? Explain authentication vs authorization.**
    Your Answer: ___________

---

## Part 6: DSA Problems - Extended (40 mins)

### Problem 1: Two Sum (Easy)
**Given:** Array of integers `nums` and target `target`, return indices of two numbers that add up to target.
- **Constraint:** Assume exactly one solution exists
- **Cannot use same element twice**

Example: `nums = [2,7,11,15]`, `target = 9` → Return `[0,1]`

**Your Solution:** (Write code with time complexity)

```java
public int[] twoSum(int[] nums, int target) {
    // Your code here
}
```

**Time Complexity:** ___________
**Space Complexity:** ___________

---

### Problem 2: Valid Anagram (Easy)
**Given:** Two strings `s` and `t`, determine if they are anagrams.

Example: `s = "anagram"`, `t = "nagaram"` → Return `true`

**Your Solution:**

```java
public boolean isAnagram(String s, String t) {
    // Your code here
}
```

**Time Complexity:** ___________

---

### Problem 3: Contains Duplicate (Easy)
**Given:** Array of integers, return `true` if any value appears at least twice.

Example: `[1,2,3,1]` → Return `true`

**Your Solution:**

```java
public boolean containsDuplicate(int[] nums) {
    // Your code here
}
```

**Time Complexity:** ___________

---

### Problem 4: Best Time to Buy and Sell Stock (Easy-Medium)
**Given:** Array `prices` where `prices[i]` is stock price on day `i`.
**Find:** Maximum profit by buying one day and selling on a later day.

Example: `prices = [7,1,5,3,6,4]` → Return `5` (buy on day 2, sell on day 5)

**Your Solution:**

```java
public int maxProfit(int[] prices) {
    // Your code here
}
```

**Time Complexity:** ___________

---

### Problem 5: Group Anagrams (Medium)
**Given:** Array of strings, group anagrams together.

Example: `["eat","tea","tan","ate","nat","bat"]` → `[["eat","tea","ate"],["tan","nat"],["bat"]]`

**Your Solution:**

```java
public List<List<String>> groupAnagrams(String[] strs) {
    // Your code here
}
```

**Time Complexity:** ___________

---

### Problem 6: Longest Substring Without Repeating Characters (Medium)
**Given:** String `s`, find length of longest substring without repeating characters.

Example: `s = "abcabcbb"` → Return `3` (substring is "abc")

**Your Solution:**

```java
public int lengthOfLongestSubstring(String s) {
    // Your code here
}
```

**Time Complexity:** ___________

---

## Part 7: Scenario-Based Questions (20 mins)

### Scenario 1: Performance Issue
**You have an API that returns a list of 10,000 products. It's slow. What could be the issues? How to optimize?**

Your Answer:
1. Possible issues: ___________
2. Optimization strategies: ___________

---

### Scenario 2: Memory Leak
**Your application's memory keeps growing. How would you identify and fix memory leaks in Java?**

Your Answer:
1. How to identify: ___________
2. Common causes: ___________
3. How to fix: ___________

---

### Scenario 3: Concurrent Access
**Multiple threads are updating a shared counter. Some updates are lost. How to fix?**

Your Answer:
1. Problem: ___________
2. Solution: ___________
3. Code example: ___________

---

### Scenario 4: Database Query Optimization
**A query taking 5 seconds. How would you optimize it?**

Your Answer:
1. Analysis steps: ___________
2. Optimization techniques: ___________

---

### Scenario 5: Design Decision
**You need to implement a logging system that should log to file, database, and console. How would you design it? Which pattern?**

Your Answer:
1. Design approach: ___________
2. Pattern used: ___________
3. Code structure: ___________

---

## Final Self-Assessment

After completing all parts, fill this:

### Strengths (Topics you answered confidently)
1. ___________
2. ___________
3. ___________
4. ___________
5. ___________

### Weaknesses (Topics that need study)
1. ___________
2. ___________
3. ___________
4. ___________
5. ___________

### Critical Gaps (Must learn immediately)
1. ___________
2. ___________
3. ___________

### Action Plan for Week 1
Based on gaps identified, what to prioritize:
1. ___________
2. ___________
3. ___________

### Confidence Level (Rate 1-10)
- Core Java Fundamentals: _____/10
- Collections & Data Structures: _____/10
- Concurrency: _____/10
- Spring Boot/JPA: _____/10
- DSA Problem Solving: _____/10

---

## Scoring Guide (Optional)

**How to evaluate yourself:**

- **9-10/10:** Strong - Focus on advanced topics
- **7-8/10:** Good - Review weak areas, continue practice
- **5-6/10:** Average - Need structured learning (follow Week 1 plan)
- **Below 5/10:** Need foundation - Start from basics in Week 1

---

**Note:** Save your answers and revisit after Week 4 to measure improvement!

**Time Taken:** _____ hours _____ minutes

