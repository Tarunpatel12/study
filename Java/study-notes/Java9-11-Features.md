# Study Notes: Java 9, 10, and 11 Features

**Topic:** Java 9, 10, and 11 Features  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What are Java 9, 10, and 11?
**Java 9** (September 2017), **Java 10** (March 2018), and **Java 11** (September 2018) are major releases that introduced significant features including modules, JShell, var keyword, and HTTP Client.

### Key Features Overview:

**Java 9:**
- Java Platform Module System (JPMS)
- JShell (REPL)
- Factory methods for collections
- Private methods in interfaces
- Process API improvements
- Reactive Streams
- HTTP/2 Client (incubator)

**Java 10:**
- Local variable type inference (var)
- Application Class-Data Sharing
- Garbage Collector improvements

**Java 11 (LTS):**
- HTTP Client (standard)
- New String methods
- Files.readString() and writeString()
- Optional.isEmpty()
- Predicate.not()
- Nest-based access control

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
- **Modularity:** Better code organization and encapsulation
- **Developer Experience:** JShell for quick testing
- **Type Inference:** Less boilerplate with var
- **Modern APIs:** HTTP Client, improved String methods
- **Performance:** Better GC, class-data sharing

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Java 9-11 Architecture:
```
Java 9-11 Features
├── Module System (JPMS)
├── JShell (REPL)
├── var (Type Inference)
├── HTTP Client
├── Collection Factory Methods
├── Interface Enhancements
└── String & Files API Improvements
```

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Java Platform Module System (JPMS) - Java 9

**Definition:**
Module system that provides strong encapsulation and reliable configuration. Modules are self-describing collections of code and data.

**Key Concepts:**
- **Module:** Named, self-describing collection of code
- **Module Descriptor:** module-info.java file
- **Exports:** Makes packages available to other modules
- **Requires:** Declares dependency on another module
- **Provides/Uses:** Service provider pattern

**Module Descriptor (module-info.java):**
```java
module com.example.myapp {
    // Requires other modules
    requires java.base;  // Implicit, always required
    requires java.sql;
    requires java.logging;
    
    // Exports packages (makes them public)
    exports com.example.myapp.api;
    exports com.example.myapp.model;
    
    // Opens packages for reflection (runtime access)
    opens com.example.myapp.internal;
    
    // Provides services
    provides com.example.service.Service
        with com.example.service.ServiceImpl;
    
    // Uses services
    uses com.example.service.Service;
}
```

**Creating a Module:**
```
myapp/
├── src/
│   └── com.example.myapp/
│       ├── module-info.java
│       └── com/
│           └── example/
│               └── myapp/
│                   └── Main.java
└── module-info.java
```

**Example module-info.java:**
```java
module com.example.myapp {
    requires java.base;
    requires java.sql;
    
    exports com.example.myapp.api;
    exports com.example.myapp.model;
}
```

**Compiling and Running:**
```bash
# Compile
javac -d mods/com.example.myapp src/com.example.myapp/module-info.java src/com.example.myapp/com/example/myapp/*.java

# Run
java --module-path mods -m com.example.myapp/com.example.myapp.Main
```

**Benefits:**
- Strong encapsulation
- Reliable configuration
- Better performance (smaller runtime)
- Clear dependencies

**Key Points:**
- `java.base` is always required (implicit)
- Only exported packages are accessible
- Reflection requires `opens` directive
- Services use `provides` and `uses`

---

### 4.2 JShell (REPL) - Java 9

**Definition:**
Read-Eval-Print Loop (REPL) for Java. Interactive tool for learning Java and prototyping.

**Starting JShell:**
```bash
jshell
```

**Basic Usage:**
```java
jshell> int x = 10;
x ==> 10

jshell> String greeting = "Hello";
greeting ==> "Hello"

jshell> System.out.println(x + greeting);
10Hello

jshell> int add(int a, int b) { return a + b; }
|  created method add(int,int)

jshell> add(5, 3)
$4 ==> 8
```

**JShell Commands:**
```java
// List all variables
jshell> /vars

// List all methods
jshell> /methods

// List all classes
jshell> /types

// List all imports
jshell> /imports

// View history
jshell> /history

// Save session
jshell> /save session.jsh

// Load session
jshell> /open session.jsh

// Exit
jshell> /exit
```

**Creating Classes:**
```java
jshell> class Person {
   ...>     private String name;
   ...>     public Person(String name) { this.name = name; }
   ...>     public String getName() { return name; }
   ...> }
|  created class Person

jshell> Person p = new Person("Alice");
p ==> Person@2f92e0f4

jshell> p.getName()
$8 ==> "Alice"
```

**Use Cases:**
- Quick testing of code snippets
- Learning Java
- Prototyping
- API exploration

---

### 4.3 Factory Methods for Collections - Java 9

**Definition:**
Static factory methods to create immutable collections easily.

**List Factory Methods:**
```java
// Empty list
List<String> empty = List.of();

// List with elements
List<String> list = List.of("A", "B", "C");

// List with many elements (up to 10)
List<String> list = List.of("A", "B", "C", "D", "E", "F", "G", "H", "I", "J");

// More than 10 elements
List<String> list = List.of("A", "B", "C", /* ... many more */);
```

**Set Factory Methods:**
```java
// Empty set
Set<String> empty = Set.of();

// Set with elements
Set<String> set = Set.of("A", "B", "C");

// Duplicates not allowed
// Set<String> set = Set.of("A", "A");  // IllegalArgumentException
```

**Map Factory Methods:**
```java
// Empty map
Map<String, Integer> empty = Map.of();

// Map with key-value pairs (up to 10 pairs)
Map<String, Integer> map = Map.of("A", 1, "B", 2, "C", 3);

// More than 10 pairs
Map<String, Integer> map = Map.ofEntries(
    Map.entry("A", 1),
    Map.entry("B", 2),
    Map.entry("C", 3)
);
```

**Characteristics:**
- **Immutable:** Cannot be modified after creation
- **Null-free:** Cannot contain null elements/keys/values
- **Space-efficient:** Optimized implementations
- **Serializable:** Can be serialized

**Examples:**
```java
// Old way
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");
list = Collections.unmodifiableList(list);

// New way
List<String> list = List.of("A", "B", "C");

// Modifying throws exception
// list.add("D");  // UnsupportedOperationException
// list.set(0, "X");  // UnsupportedOperationException
```

---

### 4.4 Private Methods in Interfaces - Java 9

**Definition:**
Interfaces can now have private methods for code reuse between default methods.

**Example:**
```java
interface Calculator {
    default int add(int a, int b) {
        return performOperation(a, b, (x, y) -> x + y);
    }
    
    default int multiply(int a, int b) {
        return performOperation(a, b, (x, y) -> x * y);
    }
    
    // Private method - shared by default methods
    private int performOperation(int a, int b, BinaryOperator<Integer> op) {
        validate(a, b);
        return op.apply(a, b);
    }
    
    // Private static method
    private static void validate(int a, int b) {
        if (a < 0 || b < 0) {
            throw new IllegalArgumentException("Negative numbers not allowed");
        }
    }
}
```

**Benefits:**
- Code reuse in interfaces
- Better organization
- Encapsulation within interface

---

### 4.5 Process API Improvements - Java 9

**Definition:**
Enhanced Process API for better process management and information.

**Getting Process Information:**
```java
// Current process
ProcessHandle current = ProcessHandle.current();
long pid = current.pid();
ProcessHandle.Info info = current.info();

// Process info
Optional<String> command = info.command();
Optional<String[]> arguments = info.arguments();
Optional<String> commandLine = info.commandLine();
Optional<Instant> startTime = info.startInstant();
Optional<Duration> cpuTime = info.totalCpuDuration();
Optional<String> user = info.user();

// All processes
Stream<ProcessHandle> allProcesses = ProcessHandle.allProcesses();
allProcesses.forEach(ph -> {
    System.out.println("PID: " + ph.pid());
    System.out.println("Info: " + ph.info());
});
```

**Process Control:**
```java
ProcessHandle process = // ... get process handle

// Check if alive
boolean isAlive = process.isAlive();

// Destroy process
process.destroy();

// Force destroy
process.destroyForcibly();

// Wait for completion
CompletableFuture<ProcessHandle> future = process.onExit();
future.thenRun(() -> System.out.println("Process terminated"));
```

**Starting Processes:**
```java
// Start process
ProcessBuilder pb = new ProcessBuilder("java", "-version");
Process process = pb.start();

// Get process handle
ProcessHandle handle = process.toHandle();

// Wait for completion
int exitCode = process.waitFor();
```

---

### 4.6 Reactive Streams - Java 9

**Definition:**
API for asynchronous stream processing with non-blocking backpressure.

**Key Interfaces:**
- `Flow.Publisher<T>` - Produces items
- `Flow.Subscriber<T>` - Consumes items
- `Flow.Subscription` - Controls flow
- `Flow.Processor<T, R>` - Transforms items

**Example:**
```java
import java.util.concurrent.Flow;
import java.util.concurrent.SubmissionPublisher;

// Publisher
SubmissionPublisher<String> publisher = new SubmissionPublisher<>();

// Subscriber
Flow.Subscriber<String> subscriber = new Flow.Subscriber<String>() {
    private Flow.Subscription subscription;
    
    @Override
    public void onSubscribe(Flow.Subscription subscription) {
        this.subscription = subscription;
        subscription.request(1);  // Request one item
    }
    
    @Override
    public void onNext(String item) {
        System.out.println("Received: " + item);
        subscription.request(1);  // Request next item
    }
    
    @Override
    public void onError(Throwable throwable) {
        System.err.println("Error: " + throwable);
    }
    
    @Override
    public void onComplete() {
        System.out.println("Completed");
    }
};

// Subscribe
publisher.subscribe(subscriber);

// Publish items
publisher.submit("Item 1");
publisher.submit("Item 2");
publisher.submit("Item 3");

// Close
publisher.close();
```

---

### 4.7 HTTP/2 Client (Incubator) - Java 9

**Definition:**
New HTTP client API (incubator in Java 9, standard in Java 11).

**Java 9 (Incubator):**
```java
import jdk.incubator.http.HttpClient;
import jdk.incubator.http.HttpRequest;
import jdk.incubator.http.HttpResponse;

HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .build();

HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
String body = response.body();
```

**Note:** HTTP Client became standard in Java 11 (see section 4.15).

---

### 4.8 Stream API Improvements - Java 9

**New Methods:**

**1. takeWhile() - Take elements while predicate is true:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> result = numbers.stream()
    .takeWhile(n -> n < 5)
    .collect(Collectors.toList());  // [1, 2, 3, 4]
```

**2. dropWhile() - Drop elements while predicate is true:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<Integer> result = numbers.stream()
    .dropWhile(n -> n < 5)
    .collect(Collectors.toList());  // [5, 6, 7, 8, 9, 10]
```

**3. iterate() with predicate:**
```java
// Old: Infinite stream
Stream.iterate(0, n -> n + 1).limit(10);

// New: With predicate (stop condition)
Stream.iterate(0, n -> n < 10, n -> n + 1)
    .forEach(System.out::println);  // 0, 1, 2, ..., 9
```

**4. ofNullable() - Create stream from nullable:**
```java
String value = null;
Stream<String> stream = Stream.ofNullable(value);  // Empty stream if null

String value = "Hello";
Stream<String> stream = Stream.ofNullable(value);  // Stream with one element
```

---

### 4.9 Optional Improvements - Java 9

**New Methods:**

**1. stream() - Convert Optional to Stream:**
```java
Optional<String> opt = Optional.of("Hello");
Stream<String> stream = opt.stream();  // Stream with one element

Optional<String> empty = Optional.empty();
Stream<String> stream = empty.stream();  // Empty stream

// Useful for chaining
List<String> result = list.stream()
    .map(this::getOptionalValue)
    .flatMap(Optional::stream)
    .collect(Collectors.toList());
```

**2. ifPresentOrElse() - Execute action or else:**
```java
Optional<String> opt = Optional.of("Hello");
opt.ifPresentOrElse(
    value -> System.out.println("Value: " + value),
    () -> System.out.println("No value")
);

Optional<String> empty = Optional.empty();
empty.ifPresentOrElse(
    value -> System.out.println("Value: " + value),
    () -> System.out.println("No value")  // This executes
);
```

**3. or() - Return alternative Optional:**
```java
Optional<String> opt = Optional.empty();
Optional<String> alternative = opt.or(() -> Optional.of("Default"));
String value = alternative.get();  // "Default"
```

---

### 4.10 Local Variable Type Inference (var) - Java 10

**Definition:**
`var` keyword allows compiler to infer the type of local variables.

**Syntax:**
```java
// Instead of explicit type
String message = "Hello";
List<String> names = new ArrayList<>();
Map<String, Integer> map = new HashMap<>();

// With var
var message = "Hello";  // Inferred as String
var names = new ArrayList<String>();  // Inferred as ArrayList<String>
var map = new HashMap<String, Integer>();  // Inferred as HashMap<String, Integer>
```

**Rules:**
- Only for local variables
- Must be initialized at declaration
- Cannot be used for method parameters
- Cannot be used for return types
- Cannot be used for fields
- Cannot be used for catch variables (Java 10), but allowed in Java 11+

**Examples:**
```java
// Simple cases
var name = "Alice";
var age = 25;
var list = new ArrayList<String>();

// With generics
var map = new HashMap<String, List<Integer>>();

// In loops
for (var name : names) {
    System.out.println(name);
}

for (var i = 0; i < 10; i++) {
    System.out.println(i);
}

// In try-with-resources
try (var reader = new FileReader("file.txt")) {
    // Use reader
}

// Lambda parameters (Java 11+)
var list = List.of(1, 2, 3);
list.forEach((var item) -> System.out.println(item));
```

**When to Use:**
- ✅ Complex generic types: `var map = new HashMap<String, List<Map<Integer, String>>>();`
- ✅ Obvious from context: `var name = "Hello";`
- ✅ Reduce boilerplate

**When NOT to Use:**
- ❌ When type is not obvious: `var result = process();` (what type?)
- ❌ When it reduces readability
- ❌ For public APIs

**Best Practices:**
```java
// GOOD: Type is obvious
var names = new ArrayList<String>();
var count = names.size();

// BAD: Type not obvious
var result = process();  // What type is result?

// GOOD: Explicit type for clarity
List<String> result = process();  // Clear what type
```

---

### 4.11 Application Class-Data Sharing - Java 10

**Definition:**
Extension of Class-Data Sharing (CDS) to application classes. Improves startup time.

**How It Works:**
1. Archive application classes at build time
2. Share archive across JVM instances
3. Faster startup

**Usage:**
```bash
# Create archive
java -Xshare:dump

# Use archive
java -Xshare:on MyApp
```

**Benefits:**
- Faster application startup
- Reduced memory footprint
- Better performance

---

### 4.12 Garbage Collector Improvements - Java 10

**Parallel Full GC for G1:**
- G1 garbage collector now uses parallel threads for full GC
- Improves performance for full GC operations

**Other Improvements:**
- Better GC tuning options
- Improved performance

---

### 4.13 HTTP Client (Standard) - Java 11

**Definition:**
HTTP Client API becomes standard (was incubator in Java 9). Supports HTTP/1.1 and HTTP/2.

**Key Classes:**
- `HttpClient` - HTTP client
- `HttpRequest` - HTTP request
- `HttpResponse` - HTTP response

**Basic Usage:**
```java
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.URI;

// Create client
HttpClient client = HttpClient.newHttpClient();

// Create request
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .GET()
    .build();

// Send request (synchronous)
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());

// Get response
int statusCode = response.statusCode();
String body = response.body();
HttpHeaders headers = response.headers();
```

**Async Request:**
```java
// Async request
CompletableFuture<HttpResponse<String>> future = client.sendAsync(
    request,
    HttpResponse.BodyHandlers.ofString()
);

// Handle response
future.thenApply(HttpResponse::body)
    .thenAccept(System.out::println)
    .join();
```

**POST Request:**
```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .POST(HttpRequest.BodyPublishers.ofString("{\"key\":\"value\"}"))
    .header("Content-Type", "application/json")
    .build();

HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
```

**Request with Timeout:**
```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .timeout(Duration.ofSeconds(10))
    .build();
```

**Custom Client:**
```java
HttpClient client = HttpClient.newBuilder()
    .connectTimeout(Duration.ofSeconds(10))
    .followRedirects(HttpClient.Redirect.NORMAL)
    .version(HttpClient.Version.HTTP_2)
    .build();
```

**Response Handlers:**
```java
// String response
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());

// Byte array response
HttpResponse<byte[]> response = client.send(request, 
    HttpResponse.BodyHandlers.ofByteArray());

// File response
HttpResponse<Path> response = client.send(request, 
    HttpResponse.BodyHandlers.ofFile(Paths.get("output.txt")));

// Discard response
HttpResponse<Void> response = client.send(request, 
    HttpResponse.BodyHandlers.discarding());
```

---

### 4.14 New String Methods - Java 11

**1. isBlank() - Check if string is blank:**
```java
String str1 = "";
String str2 = "   ";
String str3 = "Hello";

str1.isBlank();  // true
str2.isBlank();  // true (whitespace only)
str3.isBlank();  // false
```

**2. lines() - Get stream of lines:**
```java
String text = "Line 1\nLine 2\nLine 3";
text.lines()
    .forEach(System.out::println);

// Count lines
long lineCount = text.lines().count();
```

**3. strip() - Remove leading/trailing whitespace:**
```java
String str = "  Hello  ";
str.strip();  // "Hello"

// stripLeading() - Remove leading whitespace
str.stripLeading();  // "Hello  "

// stripTrailing() - Remove trailing whitespace
str.stripTrailing();  // "  Hello"
```

**4. repeat() - Repeat string:**
```java
String str = "Hello";
str.repeat(3);  // "HelloHelloHello"
```

---

### 4.15 Files.readString() and writeString() - Java 11

**Definition:**
Convenient methods to read and write entire file as string.

**readString():**
```java
// Read entire file as string
String content = Files.readString(Paths.get("file.txt"));

// With charset
String content = Files.readString(Paths.get("file.txt"), 
    StandardCharsets.UTF_8);
```

**writeString():**
```java
// Write string to file
String content = "Hello World";
Files.writeString(Paths.get("file.txt"), content);

// With charset and options
Files.writeString(Paths.get("file.txt"), content, 
    StandardCharsets.UTF_8, 
    StandardOpenOption.CREATE, 
    StandardOpenOption.WRITE);
```

**Before Java 11:**
```java
// Verbose way
String content = new String(Files.readAllBytes(Paths.get("file.txt")));
Files.write(Paths.get("file.txt"), content.getBytes());
```

---

### 4.16 Optional.isEmpty() - Java 11

**Definition:**
Method to check if Optional is empty (opposite of isPresent()).

**Example:**
```java
Optional<String> opt = Optional.empty();

// Old way
if (!opt.isPresent()) {
    // Handle empty
}

// New way
if (opt.isEmpty()) {
    // Handle empty
}
```

**Benefits:**
- More readable
- Clearer intent

---

### 4.17 Predicate.not() - Java 11

**Definition:**
Static method to negate a predicate.

**Example:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "", "Charlie");

// Old way
names.stream()
    .filter(s -> !s.isEmpty())
    .collect(Collectors.toList());

// New way
names.stream()
    .filter(Predicate.not(String::isEmpty))
    .collect(Collectors.toList());
```

**Benefits:**
- More readable
- Method reference friendly

---

### 4.18 Nest-Based Access Control - Java 11

**Definition:**
JVM improvement for nested classes. Better access control for nested classes.

**Example:**
```java
public class Outer {
    private int outerField = 10;
    
    class Inner {
        void accessOuter() {
            System.out.println(outerField);  // Can access private field
        }
    }
}
```

**Benefits:**
- Better performance
- Cleaner bytecode
- Proper access control

---

### 4.19 Epsilon Garbage Collector - Java 11

**Definition:**
No-op garbage collector for performance testing. Does not reclaim memory.

**Usage:**
```bash
java -XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC MyApp
```

**Use Cases:**
- Performance testing
- Short-lived applications
- Memory allocation testing

---

### 4.20 Flight Recorder - Java 11

**Definition:**
Low-overhead profiling tool (commercial feature in Java 8, open-source in Java 11).

**Usage:**
```bash
# Start with Flight Recorder
java -XX:+FlightRecorder MyApp

# Record for specific duration
java -XX:StartFlightRecording=duration=60s,filename=recording.jfr MyApp
```

**Benefits:**
- Low overhead profiling
- Detailed JVM metrics
- Application performance insights

---

## 📝 5. Detailed Code Examples

### Example 1: Complete Module System

```java
// module-info.java
module com.example.app {
    requires java.base;
    requires java.sql;
    requires java.logging;
    
    exports com.example.app.api;
    exports com.example.app.model;
    
    provides com.example.service.Service
        with com.example.service.ServiceImpl;
}

// Service interface
package com.example.service;
public interface Service {
    void doWork();
}

// Service implementation
package com.example.service;
public class ServiceImpl implements Service {
    @Override
    public void doWork() {
        System.out.println("Working...");
    }
}

// Using service
package com.example.app;
import com.example.service.Service;
import java.util.ServiceLoader;

ServiceLoader<Service> loader = ServiceLoader.load(Service.class);
Service service = loader.findFirst().orElseThrow();
service.doWork();
```

---

### Example 2: HTTP Client Complete Example

```java
import java.net.http.*;
import java.net.URI;
import java.time.Duration;
import java.util.concurrent.CompletableFuture;

public class HttpClientExample {
    public static void main(String[] args) throws Exception {
        // Create client
        HttpClient client = HttpClient.newBuilder()
            .connectTimeout(Duration.ofSeconds(10))
            .followRedirects(HttpClient.Redirect.NORMAL)
            .version(HttpClient.Version.HTTP_2)
            .build();
        
        // GET request
        HttpRequest getRequest = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .GET()
            .timeout(Duration.ofSeconds(10))
            .build();
        
        // Synchronous
        HttpResponse<String> response = client.send(getRequest, 
            HttpResponse.BodyHandlers.ofString());
        System.out.println("Status: " + response.statusCode());
        System.out.println("Body: " + response.body());
        
        // Asynchronous
        CompletableFuture<HttpResponse<String>> future = client.sendAsync(
            getRequest,
            HttpResponse.BodyHandlers.ofString()
        );
        
        future.thenApply(HttpResponse::body)
            .thenAccept(System.out::println)
            .join();
        
        // POST request
        String json = "{\"key\":\"value\"}";
        HttpRequest postRequest = HttpRequest.newBuilder()
            .uri(URI.create("https://api.example.com/data"))
            .POST(HttpRequest.BodyPublishers.ofString(json))
            .header("Content-Type", "application/json")
            .timeout(Duration.ofSeconds(10))
            .build();
        
        HttpResponse<String> postResponse = client.send(postRequest, 
            HttpResponse.BodyHandlers.ofString());
    }
}
```

---

### Example 3: Stream API with Java 9 Methods

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// takeWhile - take while condition is true
List<Integer> lessThan5 = numbers.stream()
    .takeWhile(n -> n < 5)
    .collect(Collectors.toList());  // [1, 2, 3, 4]

// dropWhile - drop while condition is true
List<Integer> from5 = numbers.stream()
    .dropWhile(n -> n < 5)
    .collect(Collectors.toList());  // [5, 6, 7, 8, 9, 10]

// iterate with predicate
Stream.iterate(0, n -> n < 10, n -> n + 1)
    .forEach(System.out::println);  // 0 to 9

// ofNullable
String value = getValue();  // May return null
List<String> result = Stream.ofNullable(value)
    .collect(Collectors.toList());  // Empty if null, one element if not null
```

---

### Example 4: var with Complex Types

```java
// Complex generic types
var map = new HashMap<String, List<Map<Integer, String>>>();

// Method chaining
var result = getData()
    .stream()
    .filter(this::isValid)
    .map(this::transform)
    .collect(Collectors.toList());

// Try-with-resources
try (var reader = new BufferedReader(new FileReader("file.txt"));
     var writer = new BufferedWriter(new FileWriter("output.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        writer.write(line);
    }
}
```

---

## 🔄 6. Comparison with Related Concepts

### Java 8 vs Java 9-11 Features

| Feature | Java 8 | Java 9-11 |
|---------|--------|-----------|
| **Collections** | Stream API | Factory methods, Stream improvements |
| **HTTP** | URLConnection | HTTP Client |
| **Modules** | No | JPMS |
| **REPL** | No | JShell |
| **Type Inference** | Limited | var keyword |
| **String** | Basic methods | isBlank(), lines(), strip(), repeat() |

---

### var vs Explicit Types

| Aspect | var | Explicit Type |
|--------|-----|---------------|
| **Readability** | Depends on context | Always clear |
| **Boilerplate** | Less | More |
| **Complex Types** | Better | Verbose |
| **API Clarity** | May reduce | Always clear |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Modules** provide strong encapsulation
2. **var** is for local variables only
3. **HTTP Client** supports async operations
4. **Factory methods** create immutable collections
5. **JShell** is great for learning and prototyping

### Best Practices:
- ✅ **Do:** Use factory methods for immutable collections
- ✅ **Do:** Use var for complex generic types
- ✅ **Do:** Use HTTP Client for modern HTTP operations
- ✅ **Do:** Use JShell for quick testing
- ✅ **Do:** Use new String methods (isBlank, strip, etc.)
- ❌ **Don't:** Use var when type is not obvious
- ❌ **Don't:** Modify factory-created collections
- ❌ **Don't:** Use var for public APIs

---

## 🎯 8. Interview Questions & Answers

### Q1: What is JPMS (Java Platform Module System)?
**Answer:**
JPMS is a module system introduced in Java 9 that provides:
- **Strong encapsulation:** Only exported packages are accessible
- **Reliable configuration:** Explicit module dependencies
- **Better performance:** Smaller runtime, faster startup

**Key concepts:**
- `module-info.java` - Module descriptor
- `exports` - Makes packages public
- `requires` - Declares dependencies
- `provides/uses` - Service provider pattern

---

### Q2: What is the difference between var and explicit type?
**Answer:**
- **var:** Compiler infers type, less boilerplate
- **Explicit type:** Type is clear, more verbose

**Use var when:**
- Type is obvious from context
- Complex generic types
- Reduces boilerplate

**Don't use var when:**
- Type is not obvious
- Reduces readability
- Public APIs

---

### Q3: What are factory methods for collections?
**Answer:**
Static methods in List, Set, and Map to create immutable collections easily.

**Examples:**
```java
List<String> list = List.of("A", "B", "C");
Set<String> set = Set.of("A", "B", "C");
Map<String, Integer> map = Map.of("A", 1, "B", 2);
```

**Characteristics:**
- Immutable
- Null-free
- Space-efficient

---

### Q4: What is JShell?
**Answer:**
JShell is a REPL (Read-Eval-Print Loop) for Java introduced in Java 9. Allows interactive Java programming.

**Use cases:**
- Quick testing
- Learning Java
- Prototyping
- API exploration

---

### Q5: What are the new String methods in Java 11?
**Answer:**
- `isBlank()` - Check if blank (empty or whitespace)
- `lines()` - Get stream of lines
- `strip()` - Remove leading/trailing whitespace
- `repeat()` - Repeat string

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Module Reflection
```java
// Module must be opened for reflection
module com.example.app {
    opens com.example.app.internal;  // Required for reflection
}
```

### Edge Case 2: var with Method Calls
```java
// Type may not be obvious
var result = process();  // What type?

// Better
List<String> result = process();  // Clear type
```

### Edge Case 3: Factory Methods with Null
```java
// ERROR: Null not allowed
List<String> list = List.of("A", null, "B");  // NullPointerException
```

---

## 📊 10. Performance & Complexity

### Performance Considerations:
- **Modules:** Faster startup, smaller runtime
- **HTTP Client:** Better performance than URLConnection
- **Factory methods:** Space-efficient
- **var:** No runtime impact (compile-time only)

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: Modular Application
```java
// Split application into modules
module com.example.app {
    requires com.example.core;
    requires com.example.ui;
    exports com.example.app;
}
```

### Use Case 2: HTTP API Client
```java
// Modern HTTP client
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/data"))
    .GET()
    .build();
HttpResponse<String> response = client.send(request, 
    HttpResponse.BodyHandlers.ofString());
```

### Use Case 3: Quick Prototyping with JShell
```java
// Test code quickly
jshell> var list = List.of(1, 2, 3);
jshell> list.stream().map(n -> n * 2).forEach(System.out::println);
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. **Oracle Java Documentation** - Java 9, 10, 11 Features
2. **Baeldung** - Java 9-11 Features
3. **GeeksforGeeks** - Java 9-11

### Videos:
1. **Java Brains** - Java 9-11 Features
2. **Cave of Programming** - Java 9-11

### Books:
1. **Java 9 Modularity** by Sander Mak & Paul Bakker
2. **Java 11 Cookbook** by Mohamed Sanaulla

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Explain JPMS and create modules?
- [ ] Use JShell effectively?
- [ ] Use var keyword appropriately?
- [ ] Use HTTP Client API?
- [ ] Use factory methods for collections?
- [ ] Use new String methods?
- [ ] Use Stream API improvements?
- [ ] Answer interview questions confidently?
- [ ] Apply Java 9-11 features in real projects?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. **Modules** provide better code organization
2. **var** reduces boilerplate for complex types
3. **HTTP Client** is modern and efficient
4. **Factory methods** simplify immutable collections
5. **JShell** is great for learning and testing

### Questions to Explore Further:
1. How does module system improve performance?
2. How does HTTP Client handle HTTP/2?
3. How does var type inference work?

### Connections to Other Topics:
- Related to: Java 8 Features, Modern Java
- Used in: Spring Framework, Modern Java applications
- Foundation for: Java 12+ features

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

