# Study Notes: Java 18 to 21 Features

**Topic:** Java 18, 19, 20, and 21 Features  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What are Java 18-21?
**Java 18** (March 2022), **Java 19** (September 2022), **Java 20** (March 2023), and **Java 21** (September 2023 - LTS) introduced revolutionary features including virtual threads, record patterns, pattern matching improvements, and sequenced collections.

### Key Features Overview:

**Java 18:**
- Simple Web Server
- UTF-8 by default
- Pattern matching for switch (second preview)
- Code snippets in API documentation

**Java 19:**
- Virtual Threads (preview)
- Record Patterns (preview)
- Pattern matching for switch (third preview)
- Foreign Function & Memory API (preview)
- Structured Concurrency (incubator)

**Java 20:**
- Scoped Values (preview)
- Record Patterns (second preview)
- Pattern matching for switch (fourth preview)
- Virtual Threads (second preview)
- Structured Concurrency (second incubator)

**Java 21 (LTS):**
- Virtual Threads (standard)
- Record Patterns (standard)
- Pattern matching for switch (standard)
- Sequenced Collections
- String Templates (preview)
- Unnamed Patterns and Variables (preview)
- Unnamed Classes and Instance Main Methods (preview)

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
- **Concurrency:** Virtual threads enable massive concurrency
- **Pattern Matching:** Simplify data extraction
- **Collections:** Better ordered collection APIs
- **String Processing:** Template-based string formatting
- **Simplicity:** Easier entry point for beginners

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Java 18-21 Architecture:
```
Java 18-21 Features
├── Virtual Threads
├── Record Patterns
├── Pattern Matching
├── Sequenced Collections
├── String Templates
└── Modern Java Features
```

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Virtual Threads - Java 19 (Preview), Java 21 (Standard)

**Definition:**
Lightweight threads managed by the JVM. Enable massive concurrency with minimal overhead.

**Traditional Threads (Platform Threads):**
```java
// Limited by OS thread count (~1000s)
Thread thread = new Thread(() -> {
    // Blocking I/O operation
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
thread.start();
```

**Virtual Threads:**
```java
// Can create millions of virtual threads
Thread virtualThread = Thread.ofVirtual().start(() -> {
    // Blocking I/O operation
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
});
```

**Creating Virtual Threads:**
```java
// Method 1: Thread.ofVirtual()
Thread virtualThread = Thread.ofVirtual()
    .name("virtual-thread-1")
    .start(() -> {
        System.out.println("Running in virtual thread");
    });

// Method 2: Executors.newVirtualThreadPerTaskExecutor()
ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();
executor.submit(() -> {
    System.out.println("Task in virtual thread");
});

// Method 3: ExecutorService with virtual threads
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 10_000; i++) {
        executor.submit(() -> {
            // Can create thousands of virtual threads
            processRequest();
        });
    }
}
```

**Virtual Threads vs Platform Threads:**
```java
// Platform threads - limited
ExecutorService platformExecutor = Executors.newFixedThreadPool(100);
// Can only handle ~100 concurrent tasks

// Virtual threads - unlimited
ExecutorService virtualExecutor = Executors.newVirtualThreadPerTaskExecutor();
// Can handle millions of concurrent tasks
```

**Blocking Operations:**
```java
// Virtual threads are perfect for blocking I/O
Thread.ofVirtual().start(() -> {
    // HTTP request
    HttpClient client = HttpClient.newHttpClient();
    HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://api.example.com/data"))
        .build();
    HttpResponse<String> response = client.send(request, 
        HttpResponse.BodyHandlers.ofString());
    
    // Database query
    Connection conn = DriverManager.getConnection(url);
    Statement stmt = conn.createStatement();
    ResultSet rs = stmt.executeQuery("SELECT * FROM users");
    
    // File I/O
    String content = Files.readString(Paths.get("file.txt"));
});
```

**Benefits:**
- Massive concurrency (millions of threads)
- Low memory footprint
- Simple programming model
- Perfect for I/O-bound tasks

**When to Use:**
- ✅ High concurrency applications
- ✅ I/O-bound operations
- ✅ Server applications
- ❌ CPU-intensive tasks (use platform threads)

---

### 4.2 Record Patterns - Java 19 (Preview), Java 21 (Standard)

**Definition:**
Pattern matching for records. Deconstruct records and extract components.

**Basic Record Pattern:**
```java
public record Point(int x, int y) { }

Point point = new Point(5, 10);

// Old way
if (point instanceof Point) {
    Point p = (Point) point;
    int x = p.x();
    int y = p.y();
    System.out.println("x: " + x + ", y: " + y);
}

// New way - Record pattern
if (point instanceof Point(int x, int y)) {
    System.out.println("x: " + x + ", y: " + y);
}
```

**Nested Record Patterns:**
```java
public record Point(int x, int y) { }
public record Rectangle(Point topLeft, Point bottomRight) { }

Rectangle rect = new Rectangle(new Point(0, 0), new Point(10, 10));

// Nested pattern matching
if (rect instanceof Rectangle(Point(int x1, int y1), Point(int x2, int y2))) {
    int width = x2 - x1;
    int height = y2 - y1;
    System.out.println("Width: " + width + ", Height: " + height);
}
```

**Record Patterns in Switch:**
```java
public record Circle(double radius) implements Shape { }
public record Rectangle(double width, double height) implements Shape { }

Shape shape = new Circle(5.0);

double area = switch (shape) {
    case Circle(double r) -> Math.PI * r * r;
    case Rectangle(double w, double h) -> w * h;
};
```

**Type Patterns with Records:**
```java
Object obj = new Point(5, 10);

if (obj instanceof Point(int x, int y) && x > 0 && y > 0) {
    System.out.println("Positive coordinates: (" + x + ", " + y + ")");
}
```

**Guarded Patterns:**
```java
Shape shape = new Circle(5.0);

String description = switch (shape) {
    case Circle(double r) when r > 10 -> "Large circle";
    case Circle(double r) when r > 5 -> "Medium circle";
    case Circle(double r) -> "Small circle";
    case Rectangle(double w, double h) when w == h -> "Square";
    case Rectangle(double w, double h) -> "Rectangle";
};
```

**Benefits:**
- Deconstruct records easily
- Extract components directly
- More readable code
- Type-safe

---

### 4.3 Pattern Matching for Switch - Java 18 (Second Preview), Java 21 (Standard)

**Definition:**
Pattern matching in switch expressions using type patterns, record patterns, and guards.

**Type Patterns:**
```java
Object obj = getObject();

String result = switch (obj) {
    case String s -> "String: " + s;
    case Integer i -> "Integer: " + i;
    case Double d -> "Double: " + d;
    case null -> "Null";
    default -> "Unknown";
};
```

**Record Patterns in Switch:**
```java
public record Person(String name, int age) { }

Object obj = new Person("Alice", 30);

String info = switch (obj) {
    case Person(String name, int age) when age >= 18 -> 
        name + " is an adult";
    case Person(String name, int age) -> 
        name + " is a minor";
    default -> "Not a person";
};
```

**Sealed Classes with Pattern Matching:**
```java
public sealed interface Expr permits ConstantExpr, AddExpr, MultiplyExpr { }
public record ConstantExpr(int value) implements Expr { }
public record AddExpr(Expr left, Expr right) implements Expr { }
public record MultiplyExpr(Expr left, Expr right) implements Expr { }

Expr expr = new AddExpr(
    new ConstantExpr(5),
    new ConstantExpr(3)
);

int result = switch (expr) {
    case ConstantExpr(int v) -> v;
    case AddExpr(Expr left, Expr right) -> 
        evaluate(left) + evaluate(right);
    case MultiplyExpr(Expr left, Expr right) -> 
        evaluate(left) * evaluate(right);
};
```

**Guarded Patterns:**
```java
Object obj = getObject();

String result = switch (obj) {
    case String s when s.length() > 10 -> "Long string";
    case String s when s.isEmpty() -> "Empty string";
    case String s -> "Short string: " + s;
    case Integer i when i > 0 -> "Positive: " + i;
    case Integer i -> "Non-positive: " + i;
    default -> "Other";
};
```

**Null Handling:**
```java
String str = getString();  // May return null

String result = switch (str) {
    case null -> "Null string";
    case "empty" -> "Empty";
    case String s when s.length() > 10 -> "Long: " + s;
    case String s -> "String: " + s;
};
```

---

### 4.4 Sequenced Collections - Java 21

**Definition:**
New interfaces for collections with defined encounter order.

**SequencedCollection:**
```java
// New interface
interface SequencedCollection<E> extends Collection<E> {
    // Get first element
    E getFirst();
    
    // Get last element
    E getLast();
    
    // Add first
    void addFirst(E e);
    
    // Add last
    void addLast(E e);
    
    // Remove first
    E removeFirst();
    
    // Remove last
    E removeLast();
    
    // Reversed view
    SequencedCollection<E> reversed();
}

// Implementations
SequencedCollection<String> list = new ArrayList<>();
list.add("First");
list.add("Second");
list.addLast("Third");

String first = list.getFirst();  // "First"
String last = list.getLast();    // "Third"

// Reversed view
SequencedCollection<String> reversed = list.reversed();
```

**SequencedSet:**
```java
interface SequencedSet<E> extends Set<E>, SequencedCollection<E> {
    SequencedSet<E> reversed();
}

SequencedSet<String> set = new LinkedHashSet<>();
set.add("A");
set.add("B");
set.add("C");

String first = set.getFirst();  // "A"
String last = set.getLast();    // "C"
```

**SequencedMap:**
```java
interface SequencedMap<K, V> extends Map<K, V> {
    // First entry
    Map.Entry<K, V> firstEntry();
    
    // Last entry
    Map.Entry<K, V> lastEntry();
    
    // Poll first
    Map.Entry<K, V> pollFirstEntry();
    
    // Poll last
    Map.Entry<K, V> pollLastEntry();
    
    // Reversed view
    SequencedMap<K, V> reversed();
    
    // Sequenced views
    SequencedSet<K> sequencedKeySet();
    SequencedCollection<V> sequencedValues();
    SequencedSet<Map.Entry<K, V>> sequencedEntrySet();
}

SequencedMap<String, Integer> map = new LinkedHashMap<>();
map.put("First", 1);
map.put("Second", 2);
map.putLast("Third", 3);

Map.Entry<String, Integer> first = map.firstEntry();
Map.Entry<String, Integer> last = map.lastEntry();
```

**Benefits:**
- Consistent API for ordered collections
- Easy access to first/last elements
- Reversed views
- Better than manual index access

---

### 4.5 String Templates (Preview) - Java 21

**Definition:**
Template-based string interpolation for safer and more readable string formatting.

**STR Template Processor:**
```java
import static java.lang.StringTemplate.STR;

String name = "Alice";
int age = 30;

// String template
String message = STR."Hello, \{name}! You are \{age} years old.";
// "Hello, Alice! You are 30 years old."

// Expressions
int x = 10;
int y = 20;
String result = STR."Sum: \{x + y}";
// "Sum: 30"

// Multi-line
String html = STR."""
    <html>
      <body>
        <h1>Hello, \{name}!</h1>
        <p>Age: \{age}</p>
      </body>
    </html>
    """;
```

**FMT Template Processor:**
```java
import static java.lang.StringTemplate.FMT;

String name = "Alice";
double price = 99.99;

// Formatted string
String message = FMT."Name: %-10s\{name} | Price: $%8.2f\{price}";
// "Name: Alice      | Price: $   99.99"
```

**Custom Template Processor:**
```java
import java.lang.StringTemplate.Processor;

// Custom processor
Processor<String, RuntimeException> UPPER = (StringTemplate st) -> {
    StringBuilder sb = new StringBuilder();
    List<String> fragments = st.fragments();
    List<Object> values = st.values();
    
    for (int i = 0; i < fragments.size(); i++) {
        sb.append(fragments.get(i));
        if (i < values.size()) {
            sb.append(values.get(i).toString().toUpperCase());
        }
    }
    return sb.toString();
};

String name = "Alice";
String result = UPPER."Hello, \{name}!";
// "Hello, ALICE!"
```

**Template Syntax:**
```java
// Simple interpolation
STR."Value: \{value}"

// Expressions
STR."Sum: \{a + b}"

// Method calls
STR."Length: \{str.length()}"

// Nested templates
STR."Outer: \{STR."Inner: \{value}"}"
```

**Benefits:**
- Safer than string concatenation
- More readable
- Compile-time validation
- Custom processors

---

### 4.6 Unnamed Patterns and Variables (Preview) - Java 21

**Definition:**
Use `_` to indicate unused patterns or variables.

**Unnamed Variables:**
```java
// Old way - unused variable warning
try {
    process();
} catch (Exception e) {
    // e is not used
    logger.error("Error occurred");
}

// New way - unnamed variable
try {
    process();
} catch (Exception _) {
    // No warning
    logger.error("Error occurred");
}

// In loops
for (int i = 0; i < 10; i++) {
    System.out.println("Hello");
    // i is not used
}

// With unnamed variable
for (int _ = 0; _ < 10; _++) {
    System.out.println("Hello");
}
```

**Unnamed Patterns:**
```java
public record Point(int x, int y, int z) { }

Point point = new Point(5, 10, 15);

// Only need x, ignore y and z
if (point instanceof Point(int x, int _, int _)) {
    System.out.println("X coordinate: " + x);
}

// In switch
String result = switch (point) {
    case Point(int x, int _, int _) when x > 0 -> "Positive X";
    case Point(int _, int y, int _) when y > 0 -> "Positive Y";
    case Point(int _, int _, int z) when z > 0 -> "Positive Z";
    default -> "All negative";
};
```

**Benefits:**
- No unused variable warnings
- Clear intent (intentionally unused)
- Cleaner code

---

### 4.7 Unnamed Classes and Instance Main Methods (Preview) - Java 21

**Definition:**
Simplified entry point for Java programs. No need for explicit class declaration.

**Traditional Java Program:**
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

**With Unnamed Classes:**
```java
// No class declaration needed
void main() {
    System.out.println("Hello, World!");
}

// Or with String[] parameter
void main(String[] args) {
    System.out.println("Hello, " + args[0] + "!");
}
```

**Instance Main Methods:**
```java
// Instance method (not static)
void main() {
    System.out.println("Hello from instance method!");
    // Can access instance fields
    System.out.println("Name: " + name);
}

String name = "Alice";
```

**Benefits:**
- Simpler for beginners
- Less boilerplate
- Easier learning curve

---

### 4.8 Simple Web Server - Java 18

**Definition:**
Simple HTTP server for development and testing.

**Starting Server:**
```bash
# Command line
jwebserver

# With options
jwebserver -p 8080 -d /path/to/files
```

**Programmatic Usage:**
```java
import com.sun.net.httpserver.SimpleFileServer;
import java.net.InetSocketAddress;
import java.nio.file.Path;

// Create server
var server = SimpleFileServer.createFileServer(
    new InetSocketAddress(8080),
    Path.of("/path/to/files"),
    SimpleFileServer.OutputLevel.INFO
);

// Start server
server.start();

// Stop server
server.stop(0);
```

**Use Cases:**
- Development testing
- Quick file serving
- Prototyping
- Local development

---

### 4.9 UTF-8 by Default - Java 18

**Definition:**
UTF-8 is now the default charset for all Java APIs.

**Impact:**
```java
// Before Java 18 - platform dependent
String text = new String(bytes);  // Uses platform charset

// Java 18+ - always UTF-8
String text = new String(bytes);  // Uses UTF-8
```

**Benefits:**
- Consistent behavior across platforms
- No charset issues
- Better internationalization

---

### 4.10 Structured Concurrency (Incubator) - Java 19, 20

**Definition:**
API for structured concurrent programming. Treats related tasks as a unit.

**Basic Usage:**
```java
import jdk.incubator.concurrent.StructuredTaskScope;

try (var scope = new StructuredTaskScope<String>()) {
    // Submit tasks
    Future<String> task1 = scope.fork(() -> fetchData1());
    Future<String> task2 = scope.fork(() -> fetchData2());
    
    // Wait for all
    scope.join();
    
    // Get results
    String result1 = task1.resultNow();
    String result2 = task2.resultNow();
}
```

**Shutdown on Failure:**
```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Future<String> task1 = scope.fork(() -> fetchData1());
    Future<String> task2 = scope.fork(() -> fetchData2());
    
    scope.join();
    scope.throwIfFailed();  // Throw if any failed
    
    String result1 = task1.resultNow();
    String result2 = task2.resultNow();
}
```

**Shutdown on Success:**
```java
try (var scope = new StructuredTaskScope.ShutdownOnSuccess<String>()) {
    Future<String> task1 = scope.fork(() -> fetchData1());
    Future<String> task2 = scope.fork(() -> fetchData2());
    
    String result = scope.join().resultNow();  // First successful result
}
```

**Benefits:**
- Better error handling
- Automatic cancellation
- Clear task relationships

---

### 4.11 Scoped Values (Preview) - Java 20

**Definition:**
Immutable, thread-local data that can be shared within a scope.

**Basic Usage:**
```java
import jdk.incubator.concurrent.ScopedValue;

// Define scoped value
final ScopedValue<String> USER = ScopedValue.newInstance();

// Set value
ScopedValue.runWhere(USER, "Alice", () -> {
    // Access value
    String user = USER.get();  // "Alice"
    
    // Value is available to all code in this scope
    processRequest();
});

// Value is not available outside scope
// String user = USER.get();  // Error
```

**Nested Scopes:**
```java
final ScopedValue<String> USER = ScopedValue.newInstance();
final ScopedValue<String> REQUEST_ID = ScopedValue.newInstance();

ScopedValue.runWhere(USER, "Alice", () -> {
    ScopedValue.runWhere(REQUEST_ID, "req-123", () -> {
        String user = USER.get();        // "Alice"
        String requestId = REQUEST_ID.get();  // "req-123"
    });
});
```

**Benefits:**
- Thread-safe
- Immutable
- Scoped access
- Better than ThreadLocal

---

### 4.12 Foreign Function & Memory API (Preview) - Java 19, 20

**Definition:**
API for interacting with code and data outside the JVM.

**Memory API:**
```java
import jdk.incubator.foreign.*;

// Allocate native memory
try (MemorySegment segment = MemorySegment.allocateNative(100)) {
    // Use memory
    segment.set(ValueLayout.JAVA_INT, 0, 42);
    int value = segment.get(ValueLayout.JAVA_INT, 0);
}
```

**Function API:**
```java
import jdk.incubator.foreign.*;

// Link to native functions
Linker linker = Linker.nativeLinker();
SymbolLookup lookup = SymbolLookup.loaderLookup();

FunctionDescriptor descriptor = FunctionDescriptor.of(
    ValueLayout.JAVA_INT,
    ValueLayout.JAVA_INT,
    ValueLayout.JAVA_INT
);

MethodHandle add = linker.downcallHandle(
    lookup.find("add").orElseThrow(),
    descriptor
);

int result = (int) add.invoke(5, 3);  // Call native function
```

---

### 4.13 Vector API (Second Incubator) - Java 19, 20

**Definition:**
API for expressing vector computations that compile to optimal vector instructions.

**Example:**
```java
import jdk.incubator.vector.*;

var species = IntVector.SPECIES_256;
int[] a = new int[256];
int[] b = new int[256];
int[] c = new int[256];

for (int i = 0; i < a.length; i += species.length()) {
    var va = IntVector.fromArray(species, a, i);
    var vb = IntVector.fromArray(species, b, i);
    var vc = va.add(vb);
    vc.intoArray(c, i);
}
```

---

## 📝 5. Detailed Code Examples

### Example 1: Virtual Threads for HTTP Server

```java
import java.net.http.*;
import java.util.concurrent.*;

public class VirtualThreadServer {
    public static void main(String[] args) {
        // Create virtual thread executor
        try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
            // Handle 10,000 concurrent requests
            for (int i = 0; i < 10_000; i++) {
                final int requestId = i;
                executor.submit(() -> {
                    // Each request runs in its own virtual thread
                    processRequest(requestId);
                });
            }
        }
    }
    
    private static void processRequest(int requestId) {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .build();
        
        try {
            HttpResponse<String> response = client.send(request, 
                HttpResponse.BodyHandlers.ofString());
            System.out.println("Request " + requestId + " completed");
        } catch (Exception e) {
            System.err.println("Request " + requestId + " failed: " + e);
        }
    }
}
```

---

### Example 2: Record Patterns with Sealed Classes

```java
// Sealed interface
public sealed interface Expr
    permits ConstantExpr, AddExpr, MultiplyExpr, VariableExpr {
}

// Records implementing interface
public record ConstantExpr(int value) implements Expr { }
public record AddExpr(Expr left, Expr right) implements Expr { }
public record MultiplyExpr(Expr left, Expr right) implements Expr { }
public record VariableExpr(String name) implements Expr { }

// Pattern matching with record patterns
public int evaluate(Expr expr, Map<String, Integer> vars) {
    return switch (expr) {
        case ConstantExpr(int v) -> v;
        case VariableExpr(String name) -> vars.getOrDefault(name, 0);
        case AddExpr(Expr left, Expr right) -> 
            evaluate(left, vars) + evaluate(right, vars);
        case MultiplyExpr(Expr left, Expr right) -> 
            evaluate(left, vars) * evaluate(right, vars);
    };
}

// Usage
Expr expr = new AddExpr(
    new ConstantExpr(5),
    new MultiplyExpr(
        new VariableExpr("x"),
        new ConstantExpr(2)
    )
);

Map<String, Integer> vars = Map.of("x", 10);
int result = evaluate(expr, vars);  // 5 + (10 * 2) = 25
```

---

### Example 3: Sequenced Collections

```java
// SequencedCollection
SequencedCollection<String> list = new ArrayList<>();
list.add("First");
list.add("Second");
list.addLast("Third");
list.addFirst("Zero");

String first = list.getFirst();  // "Zero"
String last = list.getLast();    // "Third"

// Reversed view
SequencedCollection<String> reversed = list.reversed();
// Iterates in reverse order

// SequencedMap
SequencedMap<String, Integer> map = new LinkedHashMap<>();
map.put("First", 1);
map.put("Second", 2);
map.putLast("Third", 3);

Map.Entry<String, Integer> firstEntry = map.firstEntry();
Map.Entry<String, Integer> lastEntry = map.lastEntry();

// Sequenced views
SequencedSet<String> keys = map.sequencedKeySet();
SequencedCollection<Integer> values = map.sequencedValues();
```

---

### Example 4: String Templates

```java
import static java.lang.StringTemplate.STR;

// Basic usage
String name = "Alice";
int age = 30;
String message = STR."Hello, \{name}! You are \{age} years old.";

// With expressions
int x = 10;
int y = 20;
String result = STR."Sum: \{x + y}, Product: \{x * y}";

// Multi-line
String html = STR."""
    <html>
      <body>
        <h1>Welcome, \{name}!</h1>
        <p>Age: \{age}</p>
        <p>Status: \{age >= 18 ? "Adult" : "Minor"}</p>
      </body>
    </html>
    """;

// Formatted
import static java.lang.StringTemplate.FMT;
double price = 99.99;
String formatted = FMT."Price: $%8.2f\{price}";
// "Price: $   99.99"
```

---

## 🔄 6. Comparison with Related Concepts

### Virtual Threads vs Platform Threads

| Aspect | Virtual Threads | Platform Threads |
|--------|-----------------|-------------------|
| **Concurrency** | Millions | Thousands |
| **Memory** | Low (~KB) | High (~MB) |
| **Use Case** | I/O-bound | CPU-bound |
| **Blocking** | Efficient | Expensive |
| **Creation** | Fast | Slower |

---

### Record Patterns vs Manual Extraction

| Aspect | Record Patterns | Manual Extraction |
|--------|-----------------|-------------------|
| **Boilerplate** | Minimal | More code |
| **Type Safety** | Compile-time | Runtime |
| **Readability** | High | Lower |
| **Nested** | Easy | Complex |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Virtual Threads** enable massive concurrency
2. **Record Patterns** simplify data extraction
3. **Sequenced Collections** provide ordered APIs
4. **String Templates** are safer than concatenation
5. **Pattern Matching** is now standard

### Best Practices:
- ✅ **Do:** Use virtual threads for I/O-bound tasks
- ✅ **Do:** Use record patterns with sealed classes
- ✅ **Do:** Use sequenced collections for ordered data
- ✅ **Do:** Use string templates for interpolation
- ✅ **Do:** Use unnamed variables for intentionally unused values
- ❌ **Don't:** Use virtual threads for CPU-intensive tasks
- ❌ **Don't:** Use platform threads when virtual threads suffice
- ❌ **Don't:** Ignore pattern matching exhaustiveness

---

## 🎯 8. Interview Questions & Answers

### Q1: What are virtual threads?
**Answer:**
Virtual threads are lightweight threads managed by the JVM, introduced in Java 19 (preview) and standardized in Java 21. They enable massive concurrency (millions of threads) with minimal memory overhead.

**Key points:**
- Managed by JVM (not OS)
- Low memory footprint (~KB vs ~MB)
- Perfect for I/O-bound operations
- Can create millions of threads

**Example:**
```java
Thread virtualThread = Thread.ofVirtual().start(() -> {
    // I/O operation
});
```

---

### Q2: What are record patterns?
**Answer:**
Record patterns allow deconstructing records and extracting components directly in pattern matching.

**Example:**
```java
if (point instanceof Point(int x, int y)) {
    // x and y extracted directly
}
```

**Benefits:**
- Less boilerplate
- Type-safe
- Works with nested records

---

### Q3: What are sequenced collections?
**Answer:**
Sequenced collections are new interfaces (SequencedCollection, SequencedSet, SequencedMap) that provide consistent APIs for ordered collections with methods like getFirst(), getLast(), and reversed().

**Example:**
```java
SequencedCollection<String> list = new ArrayList<>();
list.addFirst("First");
String first = list.getFirst();
```

---

### Q4: What are string templates?
**Answer:**
String templates provide template-based string interpolation using STR or FMT processors.

**Example:**
```java
String message = STR."Hello, \{name}!";
```

**Benefits:**
- Safer than concatenation
- More readable
- Compile-time validation

---

### Q5: What is structured concurrency?
**Answer:**
Structured concurrency treats related tasks as a unit, providing better error handling and automatic cancellation.

**Example:**
```java
try (var scope = new StructuredTaskScope<String>()) {
    Future<String> task1 = scope.fork(() -> fetchData1());
    Future<String> task2 = scope.fork(() -> fetchData2());
    scope.join();
}
```

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Virtual Thread Pinning
```java
// Virtual threads may be pinned to platform threads
// when calling native code or synchronized blocks
synchronized (lock) {
    // Virtual thread may be pinned here
}
```

### Edge Case 2: Record Pattern Exhaustiveness
```java
// Compiler checks exhaustiveness
sealed interface Expr permits Add, Multiply { }
record Add(Expr left, Expr right) implements Expr { }
record Multiply(Expr left, Expr right) implements Expr { }

// Must cover all cases
int eval(Expr e) {
    return switch (e) {
        case Add(Expr l, Expr r) -> eval(l) + eval(r);
        case Multiply(Expr l, Expr r) -> eval(l) * eval(r);
    };
}
```

### Edge Case 3: String Template Escaping
```java
// Escape braces
String text = STR."Value: \{value}";  // Interpolation
String text = STR."Value: \\{literal}";  // Literal braces
```

---

## 📊 10. Performance & Complexity

### Performance Considerations:
- **Virtual Threads:** Low overhead, high concurrency
- **Record Patterns:** Compile-time optimization
- **Sequenced Collections:** Minimal overhead
- **String Templates:** Compile-time processing

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: High-Concurrency Server
```java
// Handle thousands of concurrent requests
try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
    for (int i = 0; i < 10_000; i++) {
        executor.submit(() -> handleRequest());
    }
}
```

### Use Case 2: Data Processing with Records
```java
// Process records with pattern matching
List<Person> people = getPeople();
people.stream()
    .filter(p -> p instanceof Person(String name, int age) && age >= 18)
    .forEach(p -> processAdult(p));
```

### Use Case 3: Configuration with String Templates
```java
// Generate configuration
String config = STR."""
    server.port=\{port}
    server.host=\{host}
    db.url=\{dbUrl}
    """;
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. **Oracle Java Documentation** - Java 18-21 Features
2. **Baeldung** - Java 18-21 Features
3. **GeeksforGeeks** - Java 18-21

### Videos:
1. **Java Brains** - Java 18-21 Features
2. **Inside Java** - Project Loom (Virtual Threads)

### Books:
1. **Java Concurrency in Practice** - Updated for Virtual Threads
2. **Modern Java in Action** - Java 8-21

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Use virtual threads effectively?
- [ ] Apply record patterns?
- [ ] Use pattern matching in switch?
- [ ] Work with sequenced collections?
- [ ] Use string templates?
- [ ] Understand structured concurrency?
- [ ] Answer interview questions confidently?
- [ ] Apply Java 18-21 features in real projects?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. **Virtual Threads** revolutionize concurrency
2. **Record Patterns** simplify data extraction
3. **Pattern Matching** is now standard
4. **Sequenced Collections** provide better APIs
5. **String Templates** improve string handling

### Questions to Explore Further:
1. How do virtual threads work internally?
2. How does pattern matching compile?
3. How do string templates process?

### Connections to Other Topics:
- Related to: Java 12-17 Features, Concurrency
- Used in: Spring Framework, Modern Java applications
- Foundation for: Future Java features

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

