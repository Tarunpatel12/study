# Study Notes: OOP (Object-Oriented Programming)

**Topic:** OOP Concepts  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What is OOP?
**Object-Oriented Programming (OOP)** is a programming paradigm that organizes code around objects rather than functions. It models real-world entities as objects that have:
- **State (Attributes/Properties):** Data stored in fields/variables
- **Behavior (Methods):** Actions the object can perform
- **Identity:** Unique reference to the object

### Key Concepts:
- **Objects:** Instances of classes that contain data and methods
- **Classes:** Blueprints/templates for creating objects
- **Encapsulation:** Bundling data and methods together, hiding internal details
- **Abstraction:** Hiding complex implementation, showing only essential features
- **Inheritance:** Creating new classes based on existing classes (code reuse)
- **Polymorphism:** Same interface, different implementations

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
**Before OOP (Procedural Programming):**
- Code was organized as functions/procedures
- Data and functions were separate
- Difficult to model real-world entities
- Code reusability was limited
- Hard to maintain and extend large programs
- No clear relationship between data and operations

**After OOP:**
- Code organized around real-world entities (objects)
- Data and methods bundled together
- Natural modeling of real-world scenarios
- Code reusability through inheritance
- Easier maintenance and extension
- Clear relationships between data and operations

### Real-World Analogy:
**Think of a Car:**
- **Class:** Blueprint for a car (design)
- **Object:** Actual car (instance)
- **Attributes:** Color, model, engine (state)
- **Methods:** Start, stop, accelerate (behavior)
- **Encapsulation:** Engine details hidden, only controls exposed
- **Inheritance:** Sports car inherits from Car class
- **Polymorphism:** Different cars, same "start" method

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Step-by-Step Process:

1. **Define a Class:**
   - Create a blueprint with attributes and methods
   - Example: `class Car { ... }`

2. **Create Objects:**
   - Instantiate the class to create objects
   - Example: `Car myCar = new Car();`

3. **Access Members:**
   - Use objects to access attributes and call methods
   - Example: `myCar.start();`

4. **Inheritance:**
   - Create child classes that inherit from parent
   - Example: `class SportsCar extends Car { ... }`

5. **Polymorphism:**
   - Same method name, different implementations
   - Example: `car.start()` works for all car types

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Classes and Objects

**Definition:**
- **Class:** Template/blueprint for creating objects
- **Object:** Instance of a class (real entity)

**Example:**
```java
// Class definition
public class Car {
    // Attributes (state)
    private String color;
    private String model;
    private int speed;
    
    // Constructor
    public Car(String color, String model) {
        this.color = color;
        this.model = model;
        this.speed = 0;
    }
    
    // Methods (behavior)
    public void start() {
        System.out.println("Car started");
    }
    
    public void accelerate(int increment) {
        this.speed += increment;
    }
    
    // Getters
    public String getColor() {
        return color;
    }
}

// Creating objects
Car myCar = new Car("Red", "Toyota");
myCar.start();
```

**When to Use:** When you need to model real-world entities or group related data and operations

---

### 4.2 Encapsulation

**Definition:**
Bundling data (attributes) and methods together in a class, and restricting direct access to internal data using access modifiers.

**Key Points:**
- Data hiding using `private` access modifier
- Controlled access through `public` getters/setters
- Prevents unauthorized access and modification
- Maintains data integrity

**Example:**
```java
public class BankAccount {
    // Private data - hidden from direct access
    private double balance;
    private String accountNumber;
    
    // Public methods - controlled access
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public double getBalance() {
        return balance;  // Can't modify directly
    }
    
    // Private helper method - hidden
    private void validateAmount(double amount) {
        if (amount < 0) {
            throw new IllegalArgumentException("Amount cannot be negative");
        }
    }
}
```

**When to Use:** Always! It's a fundamental OOP principle for data protection

---

### 4.3 Abstraction

**Definition:**
Hiding complex implementation details and showing only essential features to the user. Focus on "what" an object does, not "how".

**Key Points:**
- Achieved through abstract classes and interfaces
- Simplifies complex systems
- Reduces complexity for users
- Allows implementation flexibility

**Example:**
```java
// Interface - defines WHAT (abstraction)
public interface Payment {
    void pay(double amount);  // User knows what it does
}

// Implementation - hides HOW (abstraction)
public class CreditCardPayment implements Payment {
    private String cardNumber;
    private String cvv;
    
    @Override
    public void pay(double amount) {
        // Complex implementation hidden
        validateCard();
        processTransaction(amount);
        sendConfirmation();
    }
    
    // Private methods - hidden from user
    private void validateCard() { /* ... */ }
    private void processTransaction(double amount) { /* ... */ }
    private void sendConfirmation() { /* ... */ }
}

// Usage - user doesn't care about implementation
Payment payment = new CreditCardPayment();
payment.pay(100.0);  // Simple interface, complex logic hidden
```

**When to Use:** When you want to define contracts, hide complexity, or allow multiple implementations

---

### 4.4 Inheritance

**Definition:**
Creating a new class (child/subclass) based on an existing class (parent/superclass). Child class inherits attributes and methods from parent.

**Key Points:**
- Code reusability
- `extends` keyword in Java
- Child can override parent methods
- Child can add new methods/attributes
- Single inheritance in Java (one parent only)

**Example:**
```java
// Parent class
public class Vehicle {
    protected String brand;
    protected int year;
    
    public Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
    }
    
    public void start() {
        System.out.println("Vehicle started");
    }
    
    public void displayInfo() {
        System.out.println("Brand: " + brand + ", Year: " + year);
    }
}

// Child class - inherits from Vehicle
public class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String brand, int year, int doors) {
        super(brand, year);  // Call parent constructor
        this.numberOfDoors = doors;
    }
    
    // Override parent method
    @Override
    public void start() {
        System.out.println("Car engine started");
    }
    
    // New method specific to Car
    public void openTrunk() {
        System.out.println("Trunk opened");
    }
}

// Usage
Car myCar = new Car("Toyota", 2023, 4);
myCar.start();        // Uses overridden method
myCar.displayInfo();  // Inherited from Vehicle
myCar.openTrunk();    // Car-specific method
```

**When to Use:** When you have "is-a" relationship, need code reuse, or want to extend functionality

---

### 4.5 Polymorphism

**Definition:**
Same interface, different implementations. "Poly" = many, "morph" = forms. One name, multiple behaviors.

**Types:**
1. **Compile-time Polymorphism (Method Overloading):**
   - Same method name, different parameters
   - Resolved at compile time

2. **Runtime Polymorphism (Method Overriding):**
   - Same method signature in parent and child
   - Resolved at runtime based on object type

**Example - Method Overloading:**
```java
public class Calculator {
    // Same method name, different parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
}

Calculator calc = new Calculator();
calc.add(1, 2);        // Calls first method
calc.add(1, 2, 3);     // Calls second method
calc.add(1.5, 2.5);    // Calls third method
```

**Example - Method Overriding:**
```java
// Parent class
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes sound");
    }
}

// Child classes
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows");
    }
}

// Polymorphism in action
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.makeSound();  // Output: "Dog barks"
animal2.makeSound();   // Output: "Cat meows"
```

**When to Use:** 
- Overloading: When you need methods with same purpose but different parameters
- Overriding: When child class needs different implementation than parent

---

### 4.6 Access Modifiers

**Definition:**
Keywords that control visibility and accessibility of classes, methods, and variables.

**Types:**
1. **private:** Only accessible within the same class
2. **default (package-private):** Accessible within same package
3. **protected:** Accessible within same package and subclasses
4. **public:** Accessible from anywhere

**Example:**
```java
public class Example {
    private int privateVar = 1;        // Only in this class
    int defaultVar = 2;                // Same package
    protected int protectedVar = 3;    // Same package + subclasses
    public int publicVar = 4;          // Everywhere
    
    private void privateMethod() { }
    void defaultMethod() { }
    protected void protectedMethod() { }
    public void publicMethod() { }
}
```

**When to Use:**
- **private:** For internal implementation details (encapsulation)
- **default:** For package-level access
- **protected:** For subclass access
- **public:** For API/interfaces

---

### 4.7 Constructors

**Definition:**
Special methods called when an object is created. Used to initialize object state.

**Types:**
1. **Default Constructor:** No parameters, provided by Java if none defined
2. **Parameterized Constructor:** Takes parameters
3. **Constructor Overloading:** Multiple constructors with different parameters

**Example:**
```java
public class Student {
    private String name;
    private int age;
    
    // Default constructor
    public Student() {
        this.name = "Unknown";
        this.age = 0;
    }
    
    // Parameterized constructor
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Constructor overloading
    public Student(String name) {
        this.name = name;
        this.age = 18;  // Default age
    }
}

Student s1 = new Student();              // Uses default
Student s2 = new Student("John", 20);    // Uses parameterized
Student s3 = new Student("Jane");        // Uses overloaded
```

**When to Use:** Always! To initialize object state properly

---

### 4.8 this and super Keywords

**Definition:**
- **this:** Reference to current object instance
- **super:** Reference to parent class

**Example:**
```java
public class Parent {
    protected String name;
    
    public Parent(String name) {
        this.name = name;  // 'this' refers to current object
    }
}

public class Child extends Parent {
    private int age;
    
    public Child(String name, int age) {
        super(name);        // 'super' calls parent constructor
        this.age = age;     // 'this' refers to current object
    }
    
    public void display() {
        System.out.println(super.name);  // Access parent field
        System.out.println(this.age);     // Access current field
    }
}
```

**When to Use:**
- **this:** To distinguish instance variables from parameters, call other constructors
- **super:** To call parent constructors, access parent members

---

### 4.9 Static Keyword

**Definition:**
Belongs to the class, not to instances. Shared across all objects of the class.

**Key Points:**
- Static variables: Class-level, shared by all instances
- Static methods: Can be called without creating object
- Static methods cannot access instance variables directly
- Static block: Executes when class is loaded

**Example:**
```java
public class Counter {
    // Instance variable - each object has its own
    private int instanceCount = 0;
    
    // Static variable - shared by all objects
    private static int staticCount = 0;
    
    public Counter() {
        instanceCount++;
        staticCount++;
    }
    
    // Instance method
    public int getInstanceCount() {
        return instanceCount;
    }
    
    // Static method - can be called without object
    public static int getStaticCount() {
        return staticCount;
        // Cannot access instanceCount here (non-static)
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();

c1.getInstanceCount();      // 1 (instance-specific)
Counter.getStaticCount();   // 2 (shared by all)
```

**When to Use:** For utility methods, constants, shared data across instances

---

### 4.10 final Keyword

**Definition:**
Prevents modification:
- **final variable:** Cannot be reassigned (constant)
- **final method:** Cannot be overridden
- **final class:** Cannot be extended

**Example:**
```java
public class Example {
    // Final variable - constant
    private final int MAX_VALUE = 100;
    
    // Final method - cannot be overridden
    public final void importantMethod() {
        // Implementation
    }
}

// Final class - cannot be extended
public final class ImmutableClass {
    private final String data;
    
    public ImmutableClass(String data) {
        this.data = data;  // Can assign once in constructor
    }
    
    // this.data = "new";  // ERROR - cannot reassign
}
```

**When to Use:**
- **final variable:** For constants, immutable values
- **final method:** When implementation should not change
- **final class:** For immutable classes, security

---

### 4.11 Abstract Classes

**Definition:**
Classes that cannot be instantiated. Can have both abstract (no implementation) and concrete (with implementation) methods.

**Key Points:**
- Use `abstract` keyword
- Can have abstract methods (no body)
- Can have concrete methods
- Must be extended by child classes
- Child must implement all abstract methods

**Example:**
```java
// Abstract class
public abstract class Animal {
    protected String name;
    
    // Concrete method
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    // Abstract method - no implementation
    public abstract void makeSound();
}

// Concrete class extending abstract class
public class Dog extends Animal {
    public Dog(String name) {
        this.name = name;
    }
    
    // Must implement abstract method
    @Override
    public void makeSound() {
        System.out.println(name + " barks");
    }
}

// Animal animal = new Animal();  // ERROR - cannot instantiate
Animal dog = new Dog("Buddy");    // OK - using concrete class
dog.makeSound();  // "Buddy barks"
```

**When to Use:** When you want to provide common functionality but force subclasses to implement specific methods

---

### 4.12 Interfaces

**Definition:**
Contract that defines what a class must do, not how. All methods are abstract by default (Java 8+ allows default methods).

**Key Points:**
- Use `interface` keyword
- Methods are public and abstract by default
- Can implement multiple interfaces (multiple inheritance)
- Java 8+: Can have default and static methods
- Java 9+: Can have private methods

**Example:**
```java
// Interface
public interface Flyable {
    void fly();  // Abstract method (implicitly public abstract)
    
    // Default method (Java 8+)
    default void land() {
        System.out.println("Landing...");
    }
    
    // Static method (Java 8+)
    static void showInfo() {
        System.out.println("This is a flyable object");
    }
}

// Class implementing interface
public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
}

public class Airplane implements Flyable {
    @Override
    public void fly() {
        System.out.println("Airplane is flying");
    }
}

Bird bird = new Bird();
bird.fly();        // "Bird is flying"
bird.land();       // "Landing..." (default method)
Flyable.showInfo(); // Static method call
```

**When to Use:** To define contracts, achieve abstraction, enable multiple inheritance, create flexible designs

---

### 4.13 Interface vs Abstract Class

**Key Differences:**

| Aspect | Interface | Abstract Class |
|--------|-----------|----------------|
| **Keyword** | `interface` | `abstract class` |
| **Variables** | Only constants (public static final) | Any variables |
| **Methods** | Abstract by default (Java 8+ allows default) | Can have both abstract and concrete |
| **Inheritance** | Multiple interfaces | Single class only |
| **Constructor** | No constructor | Can have constructor |
| **Access Modifiers** | Public by default | Any access modifier |
| **When to Use** | Define contracts, multiple inheritance | Share code, partial implementation |

**When to Use Interface:**
- Define contracts/APIs
- Need multiple inheritance
- Want loose coupling
- Creating flexible designs

**When to Use Abstract Class:**
- Share common code
- Need constructors
- Want to provide partial implementation
- Have "is-a" relationship

---

### 4.14 Method Overriding Rules

**Rules for Overriding:**
1. Method signature must match exactly
2. Return type must be same or covariant (subtype)
3. Access modifier cannot be more restrictive
4. Cannot override static, final, or private methods
5. Must use `@Override` annotation (recommended)

**Example:**
```java
public class Parent {
    protected void method1() { }
    public void method2() { }
    private void method3() { }  // Cannot override
    public static void method4() { }  // Cannot override
    public final void method5() { }  // Cannot override
}

public class Child extends Parent {
    @Override
    public void method1() { }  // OK - can make more accessible
    
    @Override
    protected void method2() { }  // ERROR - cannot be more restrictive
    
    // Cannot override method3, method4, method5
}
```

---

### 4.15 Method Overloading Rules

**Rules for Overloading:**
1. Method name must be same
2. Parameters must be different (number, type, or order)
3. Return type can be different (but not enough alone)
4. Access modifier can be different

**Example:**
```java
public class Calculator {
    public int add(int a, int b) { return a + b; }
    public double add(double a, double b) { return a + b; }
    public int add(int a, int b, int c) { return a + b + c; }
    // All valid overloads
}
```

---

## 📝 5. Detailed Code Examples

### Example 1: Complete OOP Example - Bank Account

```java
// Abstraction: Interface defines contract
public interface Account {
    void deposit(double amount);
    void withdraw(double amount);
    double getBalance();
}

// Encapsulation: Data hidden, controlled access
public class BankAccount implements Account {
    // Private data - encapsulation
    private String accountNumber;
    private double balance;
    private String accountHolder;
    
    // Constructor
    public BankAccount(String accountNumber, String accountHolder) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = 0.0;
    }
    
    // Public methods - controlled access
    @Override
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: " + amount);
        }
    }
    
    @Override
    public void withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            System.out.println("Withdrawn: " + amount);
        } else {
            System.out.println("Insufficient balance");
        }
    }
    
    @Override
    public double getBalance() {
        return balance;
    }
    
    // Getters
    public String getAccountNumber() {
        return accountNumber;
    }
}

// Inheritance: SavingsAccount extends BankAccount
public class SavingsAccount extends BankAccount {
    private double interestRate;
    
    public SavingsAccount(String accountNumber, String holder, double rate) {
        super(accountNumber, holder);
        this.interestRate = rate;
    }
    
    // Polymorphism: Override method
    @Override
    public void deposit(double amount) {
        super.deposit(amount);
        // Add interest for savings account
        double interest = getBalance() * interestRate / 100;
        super.deposit(interest);
    }
    
    // New method specific to SavingsAccount
    public void calculateInterest() {
        double interest = getBalance() * interestRate / 100;
        System.out.println("Interest: " + interest);
    }
}

// Usage demonstrating all OOP concepts
public class Main {
    public static void main(String[] args) {
        // Polymorphism: Using interface reference
        Account account = new SavingsAccount("123", "John", 5.0);
        
        account.deposit(1000);  // Polymorphism - calls SavingsAccount method
        System.out.println("Balance: " + account.getBalance());
    }
}
```

---

## 🔄 6. Comparison with Related Concepts

### Encapsulation vs Abstraction

| Aspect | Encapsulation | Abstraction |
|--------|---------------|-------------|
| **What it hides** | Data/internal state | Implementation details |
| **Focus** | "What" data is stored | "How" something works |
| **Achieved by** | Access modifiers (private) | Interfaces, abstract classes |
| **Level** | Code level | Design level |
| **Example** | `private String password` | `interface Payment` |

**They work together:**
- Encapsulation protects data
- Abstraction simplifies interface
- Both used in real-world code

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **OOP is about modeling real-world entities** - Think in terms of objects
2. **Encapsulation protects data** - Always use private for internal state
3. **Abstraction simplifies complexity** - Hide implementation, show interface
4. **Inheritance enables code reuse** - Use "is-a" relationship
5. **Polymorphism provides flexibility** - Same interface, different behaviors

### Best Practices:
- ✅ **Do:** Use encapsulation (private fields, public methods)
- ✅ **Do:** Prefer composition over inheritance when possible
- ✅ **Do:** Use interfaces for contracts
- ✅ **Do:** Follow SOLID principles
- ❌ **Don't:** Expose internal implementation details
- ❌ **Don't:** Use inheritance just for code reuse (use composition)
- ❌ **Don't:** Create deep inheritance hierarchies

### Common Mistakes:
1. **Exposing internal data** - Using public fields instead of getters/setters
2. **Deep inheritance** - Too many levels of inheritance (max 2-3 levels)
3. **Using inheritance for "has-a"** - Should use composition
4. **Not using interfaces** - Missing abstraction opportunities
5. **Tight coupling** - Classes too dependent on each other

---

## 🎯 8. Interview Questions & Answers

### Q1: What are the four pillars of OOP?
**Answer:**
The four pillars are:
1. **Encapsulation:** Bundling data and methods, hiding internal details
2. **Abstraction:** Hiding complexity, showing only essential features
3. **Inheritance:** Creating new classes from existing ones (code reuse)
4. **Polymorphism:** Same interface, different implementations

**Example:** [Use code examples from above]

---

### Q2: What's the difference between abstraction and encapsulation?
**Answer:**
**Abstraction** hides "how" (implementation details), while **Encapsulation** hides "what" (data/internal state).

**Abstraction Example:**
```java
interface Payment {
    void pay(double amount);  // What it does
}
// Implementation hidden
```

**Encapsulation Example:**
```java
class User {
    private String password;  // Data hidden
    public String getPassword() { return password; }  // Controlled access
}
```

**They work together** - Abstraction simplifies interface, Encapsulation protects data.

---

### Q3: When would you use an interface vs abstract class?
**Answer:**
**Use Interface when:**
- Defining contracts/APIs
- Need multiple inheritance
- Want loose coupling
- All methods are abstract

**Use Abstract Class when:**
- Sharing common code
- Need constructors
- Partial implementation
- "Is-a" relationship

**Example:** [Use comparison table from above]

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Constructor Chaining
**What happens:** Calling one constructor from another
**How to handle:**
```java
public class Student {
    public Student() {
        this("Unknown", 0);  // Calls parameterized constructor
    }
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### Edge Case 2: Method Hiding (Static Methods)
**What happens:** Static methods cannot be overridden, only hidden
**How to handle:**
```java
class Parent {
    static void method() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void method() { System.out.println("Child"); }  // Hides, not overrides
}
```

---

## 📊 10. Performance & Complexity

### OOP Overhead:
- **Memory:** Objects have overhead (object header)
- **Method Calls:** Slight overhead vs direct function calls
- **Inheritance:** Method resolution has small cost

### Performance Considerations:
- OOP overhead is minimal in modern JVMs
- Benefits (maintainability, reusability) outweigh small overhead
- JVM optimizations make OOP very efficient

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: E-Commerce System
**Problem:** Model products, customers, orders
**Solution:** Use OOP to create Product, Customer, Order classes
**Code:** [Similar to BankAccount example above]

### Use Case 2: Game Development
**Problem:** Model game entities (players, enemies, items)
**Solution:** Use inheritance for game entities, polymorphism for behaviors

---

## 📚 12. Resources Used

### Articles/Websites:
1. Baeldung - Java OOP articles
2. GeeksforGeeks - OOP concepts
3. Oracle Java Tutorials - Official OOP guide

### Videos:
1. Java Brains - OOP series
2. Programming with Mosh - OOP basics

### Books:
1. Head First Java - OOP chapters
2. Effective Java - OOP best practices

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Explain all four OOP pillars clearly?
- [ ] Write OOP code examples without looking?
- [ ] Explain why OOP exists?
- [ ] Compare abstraction vs encapsulation?
- [ ] Answer interview questions about OOP?
- [ ] Handle edge cases (constructor chaining, method hiding)?
- [ ] Explain when to use interface vs abstract class?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. OOP models real-world entities naturally
2. Encapsulation and Abstraction work together
3. Inheritance should be used carefully (prefer composition)
4. Polymorphism provides flexibility

### Questions to Explore Further:
1. How does JVM handle method overriding internally?
2. What are design patterns that use OOP principles?

### Connections to Other Topics:
- Related to: SOLID Principles, Design Patterns
- Used with: Collections, Generics, Spring Framework

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

