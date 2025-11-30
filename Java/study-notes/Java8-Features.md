# Study Notes: Java 8 Features

**Topic:** Java 8 Features (Lambda, Streams, Optional, etc.)  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What is Java 8?
**Java 8** (released March 2014) is a major release that introduced functional programming features to Java. It's one of the most significant updates in Java history.

### Key Features:
1. **Lambda Expressions** - Anonymous functions
2. **Functional Interfaces** - Single abstract method interfaces
3. **Streams API** - Functional-style operations on collections
4. **Optional** - Null-safe container
5. **Method References** - Shorthand for lambdas
6. **Default Methods** - Methods in interfaces
7. **Date/Time API** - New java.time package
8. **Parallel Streams** - Parallel processing
9. **CompletableFuture** - Asynchronous programming
10. **Nashorn JavaScript Engine** - JavaScript on JVM

### Why Java 8?
**Before Java 8:**
- Verbose anonymous inner classes
- No functional programming support
- Difficult parallel processing
- Poor date/time handling
- Null pointer exceptions common

**After Java 8:**
- Concise lambda expressions
- Functional programming style
- Easy parallel processing
- Modern date/time API
- Optional for null safety

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
- **Verbosity:** Reduce boilerplate code
- **Functional Programming:** Support modern programming paradigms
- **Parallelism:** Make parallel processing easier
- **Null Safety:** Reduce NullPointerExceptions
- **Date/Time:** Fix date/time API issues

### Real-World Impact:
- **Code Reduction:** 50-70% less code for common operations
- **Readability:** More expressive and readable code
- **Performance:** Easy parallel processing
- **Modern Java:** Brought Java into modern era

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Java 8 Architecture:
```
Java 8 Features
├── Lambda Expressions (invokedynamic)
├── Functional Interfaces (@FunctionalInterface)
├── Streams API (Lazy evaluation, Pipeline)
├── Optional (Monadic container)
├── Method References (:: operator)
├── Default Methods (Interface evolution)
└── Date/Time API (Immutable, thread-safe)
```

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Lambda Expressions

**Definition:**
Anonymous functions that can be passed as arguments or stored in variables. Provide concise way to represent method implementations.

**Syntax:**
```java
(parameters) -> expression
(parameters) -> { statements; }
```

**Before Java 8 (Anonymous Inner Class):**
```java
// Verbose anonymous inner class
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};
```

**With Lambda:**
```java
// Concise lambda expression
Runnable r = () -> System.out.println("Hello");
```

**Examples:**
```java
// No parameters
Runnable r = () -> System.out.println("Hello");

// Single parameter (parentheses optional)
Function<String, Integer> length = s -> s.length();

// Multiple parameters
BinaryOperator<Integer> add = (a, b) -> a + b;

// With type declarations
BinaryOperator<Integer> add = (Integer a, Integer b) -> a + b;

// Multiple statements
Function<String, String> process = s -> {
    String trimmed = s.trim();
    return trimmed.toUpperCase();
};

// Return statement
Function<Integer, Integer> square = x -> {
    return x * x;
};
```

**Lambda with Collections:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Old way
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});

// Lambda way
Collections.sort(names, (a, b) -> a.compareTo(b));

// Even better with method reference
Collections.sort(names, String::compareTo);
```

**Variable Capture:**
```java
int multiplier = 10;
Function<Integer, Integer> multiply = x -> x * multiplier;  // Captures multiplier
// multiplier must be effectively final
```

**Key Points:**
- Lambda expressions are objects (instances of functional interfaces)
- Can capture variables from enclosing scope (must be effectively final)
- Type inference by compiler
- Can be stored in variables, passed as arguments, returned from methods

---

### 4.2 Functional Interfaces

**Definition:**
Interface with exactly one abstract method. Can have multiple default/static methods.

**@FunctionalInterface Annotation:**
```java
@FunctionalInterface
interface MyFunction {
    int apply(int x);
    // Only one abstract method allowed
}
```

**Built-in Functional Interfaces:**

**1. Function<T, R> - Takes T, returns R:**
```java
Function<String, Integer> length = s -> s.length();
Integer len = length.apply("Hello");  // 5

// Compose functions
Function<Integer, Integer> square = x -> x * x;
Function<Integer, Integer> addOne = x -> x + 1;
Function<Integer, Integer> composed = square.andThen(addOne);
Integer result = composed.apply(3);  // 10 (3*3+1)
```

**2. Predicate<T> - Takes T, returns boolean:**
```java
Predicate<String> isEmpty = s -> s.isEmpty();
boolean result = isEmpty.test("");  // true

// Combine predicates
Predicate<String> isLong = s -> s.length() > 5;
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> combined = isLong.and(startsWithA);
```

**3. Consumer<T> - Takes T, returns void:**
```java
Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello");  // Prints "Hello"

// Chain consumers
Consumer<String> print = System.out::print;
Consumer<String> printLn = print.andThen(s -> System.out.println());
```

**4. Supplier<T> - Takes nothing, returns T:**
```java
Supplier<String> supplier = () -> "Hello";
String value = supplier.get();  // "Hello"

Supplier<LocalDateTime> now = LocalDateTime::now;
LocalDateTime time = now.get();
```

**5. UnaryOperator<T> - Takes T, returns T:**
```java
UnaryOperator<String> upper = s -> s.toUpperCase();
String result = upper.apply("hello");  // "HELLO"
```

**6. BinaryOperator<T> - Takes two T, returns T:**
```java
BinaryOperator<Integer> add = (a, b) -> a + b;
Integer sum = add.apply(5, 3);  // 8
```

**7. BiFunction<T, U, R> - Takes T and U, returns R:**
```java
BiFunction<String, Integer, String> repeat = (s, n) -> s.repeat(n);
String result = repeat.apply("Hi", 3);  // "HiHiHi"
```

**8. BiPredicate<T, U> - Takes T and U, returns boolean:**
```java
BiPredicate<String, Integer> isLonger = (s, n) -> s.length() > n;
boolean result = isLonger.test("Hello", 3);  // true
```

**9. BiConsumer<T, U> - Takes T and U, returns void:**
```java
BiConsumer<String, Integer> printPair = (s, i) -> 
    System.out.println(s + ": " + i);
printPair.accept("Age", 25);  // Prints "Age: 25"
```

**Primitive Functional Interfaces:**
```java
// Avoid boxing/unboxing
IntFunction<String> intToString = i -> String.valueOf(i);
IntPredicate isEven = i -> i % 2 == 0;
IntConsumer printInt = System.out::println;
IntSupplier getInt = () -> 42;
IntUnaryOperator square = x -> x * x;
IntBinaryOperator add = (a, b) -> a + b;
```

---

### 4.3 Method References

**Definition:**
Shorthand syntax for lambda expressions that call existing methods.

**Types of Method References:**

**1. Static Method Reference:**
```java
// Lambda
Function<String, Integer> parseInt = s -> Integer.parseInt(s);

// Method reference
Function<String, Integer> parseInt = Integer::parseInt;
```

**2. Instance Method Reference (on specific instance):**
```java
String str = "Hello";
// Lambda
Supplier<Integer> length = () -> str.length();

// Method reference
Supplier<Integer> length = str::length;
```

**3. Instance Method Reference (on arbitrary instance):**
```java
// Lambda
Function<String, Integer> length = s -> s.length();

// Method reference
Function<String, Integer> length = String::length;
```

**4. Constructor Reference:**
```java
// Lambda
Supplier<List<String>> listSupplier = () -> new ArrayList<>();

// Constructor reference
Supplier<List<String>> listSupplier = ArrayList::new;

// With parameters
Function<Integer, List<String>> listWithSize = size -> new ArrayList<>(size);
Function<Integer, List<String>> listWithSize = ArrayList::new;
```

**Examples:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Static method reference
names.forEach(System.out::println);

// Instance method reference
names.sort(String::compareToIgnoreCase);

// Constructor reference
Supplier<List<String>> supplier = ArrayList::new;
List<String> newList = supplier.get();
```

---

### 4.4 Streams API

**Definition:**
Sequence of elements supporting functional-style operations. Not a data structure, but a view of data source.

**Key Characteristics:**
- **Not a data structure:** Doesn't store elements
- **Functional:** Operations don't modify source
- **Lazy:** Operations executed only when terminal operation called
- **Pipelined:** Operations can be chained
- **Consumable:** Stream can be consumed only once

**Creating Streams:**
```java
// From collection
List<String> list = Arrays.asList("A", "B", "C");
Stream<String> stream = list.stream();

// From array
String[] array = {"A", "B", "C"};
Stream<String> stream = Arrays.stream(array);

// Static factory methods
Stream<String> stream = Stream.of("A", "B", "C");
Stream<Integer> numbers = Stream.iterate(0, n -> n + 1).limit(10);
Stream<Double> random = Stream.generate(Math::random).limit(5);

// Empty stream
Stream<String> empty = Stream.empty();

// From builder
Stream<String> stream = Stream.<String>builder()
    .add("A")
    .add("B")
    .add("C")
    .build();
```

**Stream Operations:**

**1. Intermediate Operations (Lazy):**

**filter() - Filter elements:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());  // [2, 4]
```

**map() - Transform elements:**
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");
List<String> upper = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());  // [ALICE, BOB, CHARLIE]

// Map to different type
List<String> words = Arrays.asList("1", "2", "3");
List<Integer> numbers = words.stream()
    .map(Integer::parseInt)
    .collect(Collectors.toList());  // [1, 2, 3]
```

**flatMap() - Flatten nested structures:**
```java
List<List<String>> nested = Arrays.asList(
    Arrays.asList("A", "B"),
    Arrays.asList("C", "D")
);
List<String> flat = nested.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());  // [A, B, C, D]
```

**distinct() - Remove duplicates:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3);
List<Integer> unique = numbers.stream()
    .distinct()
    .collect(Collectors.toList());  // [1, 2, 3]
```

**sorted() - Sort elements:**
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());  // [Alice, Bob, Charlie]

// Custom comparator
List<String> sortedByLength = names.stream()
    .sorted(Comparator.comparing(String::length))
    .collect(Collectors.toList());
```

**peek() - Debug/perform side effects:**
```java
List<String> names = Arrays.asList("Alice", "Bob");
List<String> result = names.stream()
    .peek(System.out::println)  // Side effect
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

**limit() - Limit number of elements:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> first3 = numbers.stream()
    .limit(3)
    .collect(Collectors.toList());  // [1, 2, 3]
```

**skip() - Skip elements:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> skipped = numbers.stream()
    .skip(2)
    .collect(Collectors.toList());  // [3, 4, 5]
```

**2. Terminal Operations (Eager):**

**collect() - Collect to collection:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
List<String> list = names.stream()
    .filter(s -> s.length() > 3)
    .collect(Collectors.toList());

// To Set
Set<String> set = names.stream()
    .collect(Collectors.toSet());

// To Map
Map<String, Integer> map = names.stream()
    .collect(Collectors.toMap(s -> s, String::length));
```

**forEach() - Perform action on each element:**
```java
List<String> names = Arrays.asList("Alice", "Bob");
names.stream()
    .forEach(System.out::println);
```

**reduce() - Reduce to single value:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> sum = numbers.stream()
    .reduce((a, b) -> a + b);  // 15

// With identity
Integer sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);  // 15

// With accumulator and combiner (for parallel)
Integer sum = numbers.parallelStream()
    .reduce(0, (a, b) -> a + b, Integer::sum);
```

**count() - Count elements:**
```java
long count = names.stream()
    .filter(s -> s.length() > 3)
    .count();
```

**anyMatch() / allMatch() / noneMatch() - Boolean checks:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
boolean hasEven = numbers.stream().anyMatch(n -> n % 2 == 0);  // true
boolean allPositive = numbers.stream().allMatch(n -> n > 0);  // true
boolean noNegative = numbers.stream().noneMatch(n -> n < 0);  // true
```

**findFirst() / findAny() - Find element:**
```java
Optional<String> first = names.stream()
    .filter(s -> s.startsWith("A"))
    .findFirst();

Optional<String> any = names.parallelStream()
    .filter(s -> s.startsWith("A"))
    .findAny();
```

**min() / max() - Find min/max:**
```java
Optional<Integer> min = numbers.stream()
    .min(Integer::compareTo);

Optional<Integer> max = numbers.stream()
    .max(Integer::compareTo);
```

**toArray() - Convert to array:**
```java
String[] array = names.stream()
    .toArray(String[]::new);
```

**Collectors (Advanced):**
```java
// Grouping
Map<Integer, List<String>> byLength = names.stream()
    .collect(Collectors.groupingBy(String::length));

// Partitioning
Map<Boolean, List<String>> partitioned = names.stream()
    .collect(Collectors.partitioningBy(s -> s.length() > 3));

// Joining
String joined = names.stream()
    .collect(Collectors.joining(", "));  // "Alice, Bob, Charlie"

// Summarizing
IntSummaryStatistics stats = numbers.stream()
    .collect(Collectors.summarizingInt(Integer::intValue));
// stats.getCount(), stats.getSum(), stats.getAverage(), etc.
```

---

### 4.5 Optional

**Definition:**
Container object that may or may not contain a non-null value. Helps avoid NullPointerException.

**Creating Optional:**
```java
// Empty Optional
Optional<String> empty = Optional.empty();

// Optional with value
Optional<String> present = Optional.of("Hello");

// Optional that may be null
String nullable = null;
Optional<String> maybe = Optional.ofNullable(nullable);
```

**Checking Value:**
```java
Optional<String> opt = Optional.of("Hello");

// Check if present
if (opt.isPresent()) {
    String value = opt.get();
}

// Check if empty
if (opt.isEmpty()) {
    // Handle empty
}
```

**Getting Value:**
```java
Optional<String> opt = Optional.of("Hello");

// Get value (throws NoSuchElementException if empty)
String value = opt.get();

// Get with default
String value = opt.orElse("Default");

// Get with supplier (lazy)
String value = opt.orElseGet(() -> "Default");

// Get or throw exception
String value = opt.orElseThrow(() -> new IllegalArgumentException());

// Get or throw with supplier
String value = opt.orElseThrow(IllegalArgumentException::new);
```

**Transforming Optional:**
```java
Optional<String> opt = Optional.of("Hello");

// Map - transform if present
Optional<Integer> length = opt.map(String::length);

// FlatMap - transform to Optional
Optional<String> upper = opt.flatMap(s -> Optional.of(s.toUpperCase()));

// Filter
Optional<String> filtered = opt.filter(s -> s.length() > 3);
```

**If Present / If Empty:**
```java
Optional<String> opt = Optional.of("Hello");

// If present
opt.ifPresent(System.out::println);

// If present or else
opt.ifPresentOrElse(
    System.out::println,
    () -> System.out.println("Empty")
);
```

**Best Practices:**
```java
// BAD: Using Optional like null check
if (optional.isPresent()) {
    String value = optional.get();
    // Use value
}

// GOOD: Functional style
optional.ifPresent(value -> {
    // Use value
});

// BAD: Returning null from Optional method
public Optional<String> getName() {
    return name == null ? null : Optional.of(name);  // WRONG!
}

// GOOD: Return Optional.empty()
public Optional<String> getName() {
    return Optional.ofNullable(name);
}

// BAD: Using Optional as method parameter
public void process(Optional<String> name) { }  // Avoid

// GOOD: Use overloading or null
public void process(String name) { }
public void process() { }
```

**Common Patterns:**
```java
// Chain of null checks
String city = person.getAddress() != null 
    ? person.getAddress().getCity() 
    : null;

// With Optional
String city = Optional.ofNullable(person)
    .map(Person::getAddress)
    .map(Address::getCity)
    .orElse(null);
```

---

### 4.6 Default Methods in Interfaces

**Definition:**
Methods in interfaces with default implementation. Allows interface evolution without breaking existing implementations.

**Syntax:**
```java
interface MyInterface {
    // Abstract method
    void abstractMethod();
    
    // Default method
    default void defaultMethod() {
        System.out.println("Default implementation");
    }
    
    // Static method
    static void staticMethod() {
        System.out.println("Static method");
    }
}
```

**Why Default Methods?**
- **Interface Evolution:** Add methods to interfaces without breaking existing code
- **Multiple Inheritance:** Classes can inherit default implementations from multiple interfaces
- **Backward Compatibility:** Existing implementations continue to work

**Example:**
```java
interface Vehicle {
    void start();
    
    default void stop() {
        System.out.println("Vehicle stopped");
    }
}

class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car started");
    }
    // stop() method inherited from interface
}

// Usage
Car car = new Car();
car.start();  // Car started
car.stop();   // Vehicle stopped (default implementation)
```

**Multiple Inheritance:**
```java
interface A {
    default void method() {
        System.out.println("A");
    }
}

interface B {
    default void method() {
        System.out.println("B");
    }
}

class C implements A, B {
    // Must override to resolve conflict
    @Override
    public void method() {
        A.super.method();  // Call A's default method
        B.super.method();  // Call B's default method
    }
}
```

**Static Methods in Interfaces:**
```java
interface MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}

// Usage
int sum = MathUtils.add(5, 3);
```

---

### 4.7 Date/Time API (java.time)

**Definition:**
New date and time API introduced in Java 8. Replaces old java.util.Date and java.util.Calendar.

**Key Classes:**

**1. LocalDate - Date without time:**
```java
// Current date
LocalDate today = LocalDate.now();

// Specific date
LocalDate date = LocalDate.of(2024, 1, 15);
LocalDate date = LocalDate.of(2024, Month.JANUARY, 15);

// Parse from string
LocalDate date = LocalDate.parse("2024-01-15");

// Operations
LocalDate tomorrow = today.plusDays(1);
LocalDate nextWeek = today.plusWeeks(1);
LocalDate nextMonth = today.plusMonths(1);
LocalDate nextYear = today.plusYears(1);

// Comparison
boolean isAfter = date1.isAfter(date2);
boolean isBefore = date1.isBefore(date2);
boolean isEqual = date1.isEqual(date2);
```

**2. LocalTime - Time without date:**
```java
// Current time
LocalTime now = LocalTime.now();

// Specific time
LocalTime time = LocalTime.of(14, 30);
LocalTime time = LocalTime.of(14, 30, 45);
LocalTime time = LocalTime.of(14, 30, 45, 123);

// Parse
LocalTime time = LocalTime.parse("14:30:45");

// Operations
LocalTime later = time.plusHours(2);
LocalTime earlier = time.minusMinutes(30);
```

**3. LocalDateTime - Date and time:**
```java
// Current date-time
LocalDateTime now = LocalDateTime.now();

// Specific date-time
LocalDateTime dt = LocalDateTime.of(2024, 1, 15, 14, 30);
LocalDateTime dt = LocalDateTime.of(date, time);

// Parse
LocalDateTime dt = LocalDateTime.parse("2024-01-15T14:30:45");

// Operations
LocalDateTime later = dt.plusHours(2);
LocalDateTime earlier = dt.minusDays(1);
```

**4. ZonedDateTime - Date-time with timezone:**
```java
// Current date-time with timezone
ZonedDateTime now = ZonedDateTime.now();

// Specific timezone
ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("America/New_York"));

// Convert timezone
ZonedDateTime tokyo = zdt.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
```

**5. Instant - Point in time (UTC):**
```java
// Current instant
Instant now = Instant.now();

// From epoch seconds
Instant instant = Instant.ofEpochSecond(1609459200);

// Operations
Instant later = instant.plusSeconds(3600);
```

**6. Duration - Time-based amount:**
```java
Duration duration = Duration.between(start, end);
Duration oneHour = Duration.ofHours(1);
Duration oneDay = Duration.ofDays(1);

long seconds = duration.getSeconds();
long minutes = duration.toMinutes();
```

**7. Period - Date-based amount:**
```java
Period period = Period.between(startDate, endDate);
Period oneMonth = Period.ofMonths(1);
Period oneYear = Period.ofYears(1);

int days = period.getDays();
int months = period.getMonths();
```

**Formatting:**
```java
LocalDateTime dt = LocalDateTime.now();

// Format
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String formatted = dt.format(formatter);

// Parse
LocalDateTime parsed = LocalDateTime.parse("2024-01-15 14:30:45", formatter);
```

---

### 4.8 Parallel Streams

**Definition:**
Streams that can process elements in parallel using multiple threads.

**Creating Parallel Stream:**
```java
// From collection
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> parallel = numbers.parallelStream();

// Convert sequential to parallel
Stream<Integer> parallel = numbers.stream().parallel();
```

**When to Use:**
- Large datasets
- CPU-intensive operations
- Independent operations
- Sufficient cores available

**When NOT to Use:**
- Small datasets (overhead > benefit)
- Operations with dependencies
- I/O bound operations
- Order-dependent operations

**Example:**
```java
List<Integer> numbers = IntStream.range(1, 1000000)
    .boxed()
    .collect(Collectors.toList());

// Sequential
long start = System.currentTimeMillis();
long sum = numbers.stream()
    .mapToLong(Integer::longValue)
    .sum();
long sequentialTime = System.currentTimeMillis() - start;

// Parallel
start = System.currentTimeMillis();
sum = numbers.parallelStream()
    .mapToLong(Integer::longValue)
    .sum();
long parallelTime = System.currentTimeMillis() - start;
```

**Thread Safety:**
```java
// BAD: Not thread-safe
List<String> result = new ArrayList<>();
names.parallelStream()
    .forEach(result::add);  // ConcurrentModificationException possible

// GOOD: Use collect
List<String> result = names.parallelStream()
    .collect(Collectors.toList());
```

---

### 4.9 CompletableFuture

**Definition:**
Future that can be explicitly completed. Supports asynchronous programming and composition.

**Creating CompletableFuture:**
```java
// Completed future
CompletableFuture<String> future = CompletableFuture.completedFuture("Hello");

// Supply async
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
    return "Hello";
});

// Run async
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    System.out.println("Hello");
});
```

**Getting Result:**
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello");

// Blocking get
String result = future.get();  // Blocks until complete

// Get with timeout
String result = future.get(1, TimeUnit.SECONDS);

// Non-blocking
future.thenAccept(result -> System.out.println(result));
```

**Chaining:**
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> "Hello")
    .thenApply(s -> s + " World")
    .thenApply(String::toUpperCase)
    .thenAccept(System.out::println);
```

**Combining:**
```java
CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> "Hello");
CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> "World");

// Combine both
CompletableFuture<String> combined = future1.thenCombine(
    future2,
    (s1, s2) -> s1 + " " + s2
);
```

**Error Handling:**
```java
CompletableFuture<String> future = CompletableFuture
    .supplyAsync(() -> {
        if (true) throw new RuntimeException("Error");
        return "Hello";
    })
    .exceptionally(ex -> "Error occurred: " + ex.getMessage())
    .thenApply(String::toUpperCase);
```

---

### 4.10 Other Java 8 Features

**Base64 Encoding:**
```java
// Encode
String encoded = Base64.getEncoder().encodeToString("Hello".getBytes());

// Decode
byte[] decoded = Base64.getDecoder().decode(encoded);
String decodedStr = new String(decoded);
```

**String Joiner:**
```java
StringJoiner joiner = new StringJoiner(", ", "[", "]");
joiner.add("A").add("B").add("C");
String result = joiner.toString();  // "[A, B, C]"
```

**Nashorn JavaScript Engine:**
```java
ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");
engine.eval("print('Hello from JavaScript');");
```

---

## 📝 5. Detailed Code Examples

### Example 1: Complete Stream Processing

```java
class Employee {
    private String name;
    private int age;
    private double salary;
    private String department;
    
    // Constructors, getters, setters
}

List<Employee> employees = // ... employees list

// Find average salary by department
Map<String, Double> avgSalaryByDept = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.averagingDouble(Employee::getSalary)
    ));

// Find top 3 highest paid employees
List<Employee> top3 = employees.stream()
    .sorted(Comparator.comparing(Employee::getSalary).reversed())
    .limit(3)
    .collect(Collectors.toList());

// Group employees by age range
Map<String, List<Employee>> byAgeRange = employees.stream()
    .collect(Collectors.groupingBy(emp -> {
        int age = emp.getAge();
        if (age < 30) return "Young";
        if (age < 50) return "Middle";
        return "Senior";
    }));
```

---

### Example 2: Optional Chain

```java
class Person {
    private Optional<Address> address;
    // getters
}

class Address {
    private Optional<String> city;
    // getters
}

// Chain of null checks with Optional
String city = person.getAddress()
    .flatMap(Address::getCity)
    .orElse("Unknown");

// With default operations
String city = person.getAddress()
    .flatMap(Address::getCity)
    .filter(c -> !c.isEmpty())
    .map(String::toUpperCase)
    .orElse("UNKNOWN");
```

---

### Example 3: Parallel Processing

```java
// Process large dataset in parallel
List<String> largeList = // ... large list

// Parallel processing
Map<String, Long> wordCount = largeList.parallelStream()
    .flatMap(line -> Arrays.stream(line.split(" ")))
    .collect(Collectors.groupingBy(
        word -> word,
        Collectors.counting()
    ));

// Parallel reduce
int sum = IntStream.range(1, 1000000)
    .parallel()
    .filter(n -> n % 2 == 0)
    .sum();
```

---

## 🔄 6. Comparison with Related Concepts

### Lambda vs Anonymous Inner Class

| Aspect | Lambda | Anonymous Inner Class |
|--------|---------|----------------------|
| **Syntax** | Concise | Verbose |
| **Scope** | Lexical | Has its own scope |
| **`this`** | Refers to enclosing class | Refers to inner class |
| **Performance** | Better (invokedynamic) | Slightly slower |
| **Use Case** | Functional interfaces | Any interface/class |

---

### Stream vs Collection

| Aspect | Stream | Collection |
|--------|--------|------------|
| **Storage** | No storage | Stores elements |
| **Operations** | Functional | Imperative |
| **Lazy** | Yes | No |
| **Consumable** | Once | Multiple times |
| **Purpose** | Process data | Store data |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Lambdas** are instances of functional interfaces
2. **Streams** are lazy and consumable once
3. **Optional** should not be used as method parameters
4. **Parallel streams** need careful consideration
5. **Default methods** enable interface evolution

### Best Practices:
- ✅ **Do:** Use method references when possible
- ✅ **Do:** Use Optional for return types
- ✅ **Do:** Prefer functional style with streams
- ✅ **Do:** Use parallel streams for large datasets
- ✅ **Do:** Chain Optional operations
- ❌ **Don't:** Use Optional as method parameter
- ❌ **Don't:** Call get() without checking isPresent()
- ❌ **Don't:** Use parallel streams for small data
- ❌ **Don't:** Modify source collection in stream

---

## 🎯 8. Interview Questions & Answers

### Q1: What are lambda expressions?
**Answer:**
Lambda expressions are anonymous functions that can be passed as arguments or stored in variables. They provide concise syntax for implementing functional interfaces.

**Example:**
```java
// Before
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Hello");
    }
};

// With lambda
Runnable r = () -> System.out.println("Hello");
```

---

### Q2: What is the difference between map() and flatMap()?
**Answer:**
- **map():** Transforms each element to another element (1-to-1)
- **flatMap():** Transforms each element to a stream and flattens (1-to-many)

**Example:**
```java
// map - 1-to-1
List<String> upper = names.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// flatMap - 1-to-many
List<String> words = sentences.stream()
    .flatMap(s -> Arrays.stream(s.split(" ")))
    .collect(Collectors.toList());
```

---

### Q3: What is Optional and when to use it?
**Answer:**
Optional is a container that may or may not contain a value. Use it for:
- Return types that may be null
- Avoiding NullPointerException
- Expressing "may not have value" intent

**Don't use for:**
- Method parameters
- Collection elements
- Fields (use null checks)

---

### Q4: Explain PECS in context of streams.
**Answer:**
While PECS is for generics, similar concept applies:
- **Producer (extends):** Reading from source (upstream)
- **Consumer (super):** Writing to destination (downstream)

In streams, operations are chained where each is both consumer (of previous) and producer (for next).

---

### Q5: What is type erasure impact on lambdas?
**Answer:**
Lambdas use `invokedynamic` which is more efficient than anonymous inner classes. Type erasure doesn't affect lambdas directly, but generic type information is still erased at runtime.

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Stream Consumed Twice
```java
Stream<String> stream = names.stream();
stream.forEach(System.out::println);
stream.forEach(System.out::println);  // IllegalStateException!
```

### Edge Case 2: Optional of Null
```java
// ERROR
Optional<String> opt = Optional.of(null);  // NullPointerException

// OK
Optional<String> opt = Optional.ofNullable(null);  // Empty Optional
```

### Edge Case 3: Parallel Stream Ordering
```java
// Order not guaranteed in parallel
List<Integer> result = numbers.parallelStream()
    .map(n -> n * 2)
    .collect(Collectors.toList());  // Order may vary
```

---

## 📊 10. Performance & Complexity

### Performance Considerations:
- **Lambdas:** More efficient than anonymous inner classes
- **Streams:** Lazy evaluation improves performance
- **Parallel Streams:** Use for large datasets only
- **Optional:** Minimal overhead, significant safety benefit

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: Data Processing Pipeline
```java
// Process orders: filter, transform, group, aggregate
Map<String, Double> revenueByCategory = orders.stream()
    .filter(Order::isCompleted)
    .filter(o -> o.getAmount() > 100)
    .collect(Collectors.groupingBy(
        Order::getCategory,
        Collectors.summingDouble(Order::getAmount)
    ));
```

### Use Case 2: Null-Safe Property Access
```java
// Safe navigation with Optional
String city = Optional.ofNullable(person)
    .map(Person::getAddress)
    .map(Address::getCity)
    .orElse("Unknown");
```

### Use Case 3: Async Processing
```java
// Process multiple tasks asynchronously
CompletableFuture<List<String>> future = CompletableFuture
    .supplyAsync(() -> fetchData())
    .thenApply(data -> processData(data))
    .thenApply(processed -> formatData(processed));
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. **Oracle Java Tutorials** - Lambda Expressions, Streams
2. **Baeldung** - Java 8 Features
3. **GeeksforGeeks** - Java 8 Features

### Videos:
1. **Java Brains** - Java 8 Features
2. **Cave of Programming** - Java 8

### Books:
1. **Java 8 in Action** by Urma, Fusco, Mycroft
2. **Effective Java** by Joshua Bloch

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Write lambda expressions?
- [ ] Use all functional interfaces?
- [ ] Use method references?
- [ ] Process collections with streams?
- [ ] Use Optional correctly?
- [ ] Use default methods?
- [ ] Work with new Date/Time API?
- [ ] Use parallel streams appropriately?
- [ ] Answer interview questions confidently?
- [ ] Apply Java 8 features in real projects?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. **Lambdas** make code concise and readable
2. **Streams** enable functional programming style
3. **Optional** reduces NullPointerExceptions
4. **Default methods** enable interface evolution
5. **Date/Time API** is much better than old API

### Questions to Explore Further:
1. How does invokedynamic work?
2. How do parallel streams use ForkJoinPool?
3. How does CompletableFuture handle async operations?

### Connections to Other Topics:
- Related to: Functional Programming, Collections
- Used in: Spring Framework, Modern Java applications
- Foundation for: Reactive Programming (Java 9+)

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

