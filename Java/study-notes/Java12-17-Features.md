# Study Notes: Java 12 to 17 Features

**Topic:** Java 12, 13, 14, 15, 16, and 17 Features  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What are Java 12-17?
**Java 12** (March 2019), **Java 13** (September 2019), **Java 14** (March 2020), **Java 15** (September 2020), **Java 16** (March 2021), and **Java 17** (September 2021 - LTS) introduced significant features including switch expressions, records, sealed classes, pattern matching, and text blocks.

### Key Features Overview:

**Java 12:**
- Switch expressions (preview)
- Compact number formatting
- String methods improvements

**Java 13:**
- Text blocks (preview)
- Switch expressions improvements
- Dynamic CDS archives

**Java 14:**
- Switch expressions (standard)
- Pattern matching for instanceof (preview)
- Records (preview)
- Helpful NullPointerExceptions
- Packaging tool (jpackage)

**Java 15:**
- Text blocks (standard)
- Sealed classes (preview)
- Pattern matching for instanceof (second preview)
- Records (second preview)
- Hidden classes

**Java 16:**
- Records (standard)
- Pattern matching for instanceof (standard)
- Sealed classes (second preview)
- Vector API (incubator)
- Foreign Linker API (incubator)

**Java 17 (LTS):**
- Sealed classes (standard)
- Pattern matching for switch (preview)
- Foreign Function & Memory API (incubator)
- Strong encapsulation of JDK internals

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
- **Verbosity:** Records reduce boilerplate
- **Type Safety:** Sealed classes restrict inheritance
- **Pattern Matching:** Simplify instanceof checks
- **Text Blocks:** Better multi-line strings
- **Switch Expressions:** More expressive switch statements

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Java 12-17 Architecture:
```
Java 12-17 Features
├── Switch Expressions
├── Records
├── Sealed Classes
├── Pattern Matching
├── Text Blocks
└── Modern Java Features
```

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Switch Expressions - Java 12 (Preview), Java 14 (Standard)

**Definition:**
Switch expressions allow switch to be used as an expression that returns a value, with improved syntax and exhaustiveness checking.

**Traditional Switch (Statement):**
```java
String day = "MONDAY";
String result;
switch (day) {
    case "MONDAY":
    case "TUESDAY":
    case "WEDNESDAY":
    case "THURSDAY":
    case "FRIDAY":
        result = "Weekday";
        break;
    case "SATURDAY":
    case "SUNDAY":
        result = "Weekend";
        break;
    default:
        result = "Unknown";
}
```

**Switch Expression (Java 14+):**
```java
String day = "MONDAY";
String result = switch (day) {
    case "MONDAY", "TUESDAY", "WEDNESDAY", "THURSDAY", "FRIDAY" -> "Weekday";
    case "SATURDAY", "SUNDAY" -> "Weekend";
    default -> "Unknown";
};
```

**Arrow Syntax (->):**
```java
int day = 3;
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    case 4 -> "Thursday";
    case 5 -> "Friday";
    case 6 -> "Saturday";
    case 7 -> "Sunday";
    default -> "Invalid day";
};
```

**Yield Statement (for complex logic):**
```java
int day = 3;
String result = switch (day) {
    case 1, 2, 3, 4, 5 -> {
        String prefix = "Weekday: ";
        yield prefix + getDayName(day);
    }
    case 6, 7 -> {
        String prefix = "Weekend: ";
        yield prefix + getDayName(day);
    }
    default -> "Invalid";
};
```

**Returning Values:**
```java
// Return int
int dayNumber = switch (day) {
    case "MONDAY" -> 1;
    case "TUESDAY" -> 2;
    // ...
    default -> 0;
};

// Return enum
DayType type = switch (day) {
    case "MONDAY", "TUESDAY", "WEDNESDAY", "THURSDAY", "FRIDAY" -> DayType.WEEKDAY;
    case "SATURDAY", "SUNDAY" -> DayType.WEEKEND;
};
```

**Exhaustiveness:**
```java
// Compiler checks all cases are covered
enum Day { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY }

String type = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "Weekday";
    case SATURDAY, SUNDAY -> "Weekend";
    // No default needed - compiler knows all cases covered
};
```

**Benefits:**
- More concise
- Exhaustiveness checking
- No fall-through
- Can return values
- Less error-prone

---

### 4.2 Text Blocks - Java 13 (Preview), Java 15 (Standard)

**Definition:**
Multi-line string literals that preserve formatting and reduce escape sequences.

**Before Text Blocks:**
```java
String html = "<html>\n" +
    "  <body>\n" +
    "    <p>Hello, World</p>\n" +
    "  </body>\n" +
    "</html>";

// Or with escape sequences
String json = "{\n" +
    "  \"name\": \"John\",\n" +
    "  \"age\": 30\n" +
    "}";
```

**With Text Blocks:**
```java
String html = """
    <html>
      <body>
        <p>Hello, World</p>
      </body>
    </html>
    """;

String json = """
    {
      "name": "John",
      "age": 30
    }
    """;
```

**Syntax:**
- Starts with `"""` (three double quotes)
- Ends with `"""` on separate line
- Preserves indentation
- Automatically handles line breaks

**Escape Sequences:**
```java
// Newline
String text = """
    Line 1\nLine 2
    """;

// Quotes
String text = """
    He said "Hello"
    """;

// Backslash
String text = """
    Path: C:\\Users\\Name
    """;
```

**Indentation:**
```java
// Common indentation is removed
String html = """
    <html>
      <body>
        <p>Hello</p>
      </body>
    </html>
    """;
// Leading spaces on each line are removed based on closing """
```

**String Methods:**
```java
String text = """
    Hello
    World
    """;

text.stripIndent();  // Remove common indentation
text.translateEscapes();  // Translate escape sequences
```

**Use Cases:**
- SQL queries
- JSON/XML
- HTML templates
- Multi-line messages
- Code generation

---

### 4.3 Records - Java 14 (Preview), Java 16 (Standard)

**Definition:**
Immutable data carriers that automatically generate constructors, getters, equals(), hashCode(), and toString().

**Traditional Class:**
```java
public class Person {
    private final String name;
    private final int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```

**With Records:**
```java
public record Person(String name, int age) {
    // All methods automatically generated!
}
```

**Usage:**
```java
Person person = new Person("Alice", 30);
String name = person.name();  // Accessor method (not getter)
int age = person.age();

// Equals and hashCode
Person person1 = new Person("Alice", 30);
Person person2 = new Person("Alice", 30);
boolean equal = person1.equals(person2);  // true

// toString
System.out.println(person);  // Person[name=Alice, age=30]
```

**Custom Methods:**
```java
public record Person(String name, int age) {
    // Custom constructor
    public Person {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
    
    // Custom methods
    public boolean isAdult() {
        return age >= 18;
    }
    
    // Static methods
    public static Person of(String name, int age) {
        return new Person(name, age);
    }
}
```

**Compact Constructor:**
```java
public record Person(String name, int age) {
    // Compact constructor - no parameters, just validation
    public Person {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

**Records with Generics:**
```java
public record Pair<T, U>(T first, U second) {
    public Pair {
        if (first == null || second == null) {
            throw new IllegalArgumentException("Cannot be null");
        }
    }
}

// Usage
Pair<String, Integer> pair = new Pair<>("Key", 100);
```

**Records Can Implement Interfaces:**
```java
public interface Drawable {
    void draw();
}

public record Circle(double radius) implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing circle with radius: " + radius);
    }
}
```

**Records Cannot:**
- Extend classes (implicitly extends Record)
- Be abstract
- Have instance fields (only record components)
- Have instance initializers

**Benefits:**
- Less boilerplate
- Immutable by default
- Value semantics
- Clear intent (data carrier)

---

### 4.4 Pattern Matching for instanceof - Java 14 (Preview), Java 16 (Standard)

**Definition:**
Simplifies instanceof checks by automatically casting and binding the variable.

**Before Pattern Matching:**
```java
Object obj = "Hello";

if (obj instanceof String) {
    String str = (String) obj;  // Manual cast
    int length = str.length();
    System.out.println("Length: " + length);
}
```

**With Pattern Matching:**
```java
Object obj = "Hello";

if (obj instanceof String str) {  // Automatic cast and binding
    int length = str.length();  // Use str directly
    System.out.println("Length: " + length);
}
```

**Complex Examples:**
```java
Object obj = getObject();

if (obj instanceof String str && str.length() > 5) {
    // str is available in the condition
    System.out.println("Long string: " + str);
}

// With else
if (obj instanceof String str) {
    System.out.println("String: " + str);
} else if (obj instanceof Integer num) {
    System.out.println("Number: " + num);
} else {
    System.out.println("Other: " + obj);
}
```

**Scope:**
```java
Object obj = "Hello";

if (obj instanceof String str) {
    // str is available here
    System.out.println(str);
}
// str is NOT available here (out of scope)

// But with &&, it's available in the condition
if (obj instanceof String str && str.length() > 0) {
    // str available here
}
```

**Benefits:**
- Less boilerplate
- No manual casting
- More readable
- Type-safe

---

### 4.5 Sealed Classes - Java 15 (Preview), Java 17 (Standard)

**Definition:**
Classes and interfaces that restrict which classes can extend or implement them.

**Sealed Class:**
```java
public sealed class Shape
    permits Circle, Rectangle, Triangle {
    // Base class
}

// Permitted subclasses
public final class Circle extends Shape {
    private double radius;
    // ...
}

public final class Rectangle extends Shape {
    private double width, height;
    // ...
}

public final class Triangle extends Shape {
    private double base, height;
    // ...
}
```

**Sealed Interface:**
```java
public sealed interface Expr
    permits ConstantExpr, AddExpr, MultiplyExpr {
    // Interface
}

public record ConstantExpr(int value) implements Expr { }
public record AddExpr(Expr left, Expr right) implements Expr { }
public record MultiplyExpr(Expr left, Expr right) implements Expr { }
```

**Permits Clause:**
```java
// Explicit permits
public sealed class Shape permits Circle, Rectangle { }

// In same file - no permits needed
public sealed class Shape { }
final class Circle extends Shape { }  // Same file
final class Rectangle extends Shape { }  // Same file
```

**Non-sealed Classes:**
```java
public sealed class Shape permits Circle, Rectangle, Polygon { }

public final class Circle extends Shape { }
public final class Rectangle extends Shape { }
public non-sealed class Polygon extends Shape { }  // Can be extended further

// Now Polygon can have subclasses
class Hexagon extends Polygon { }
class Octagon extends Polygon { }
```

**With Switch Expressions:**
```java
Shape shape = new Circle(5.0);

double area = switch (shape) {
    case Circle c -> Math.PI * c.radius() * c.radius();
    case Rectangle r -> r.width() * r.height();
    case Triangle t -> 0.5 * t.base() * t.height();
    // Exhaustive - compiler knows all cases covered
};
```

**Benefits:**
- Controlled inheritance
- Exhaustiveness in switch
- Better type safety
- Clear API design

---

### 4.6 Pattern Matching for Switch - Java 17 (Preview)

**Definition:**
Pattern matching in switch expressions using type patterns.

**Basic Pattern Matching:**
```java
Object obj = getObject();

String result = switch (obj) {
    case String s -> "String: " + s;
    case Integer i -> "Integer: " + i;
    case Double d -> "Double: " + d;
    case null -> "Null";
    default -> "Unknown: " + obj;
};
```

**With Sealed Classes:**
```java
Shape shape = getShape();

double area = switch (shape) {
    case Circle c -> Math.PI * c.radius() * c.radius();
    case Rectangle r -> r.width() * r.height();
    case Triangle t -> 0.5 * t.base() * t.height();
};
```

**Guarded Patterns:**
```java
Object obj = getObject();

String result = switch (obj) {
    case String s when s.length() > 10 -> "Long string: " + s;
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
    case String s -> "String: " + s;
};
```

---

### 4.7 Helpful NullPointerExceptions - Java 14

**Definition:**
Improved NullPointerException messages that show which variable was null.

**Before Java 14:**
```java
String name = null;
int length = name.length();  // NullPointerException
// Error: NullPointerException at line 2
// Doesn't tell which variable was null
```

**Java 14+:**
```java
String name = null;
int length = name.length();  // NullPointerException
// Error: NullPointerException: Cannot invoke "String.length()" because "name" is null
// Clearly shows "name" was null
```

**Complex Examples:**
```java
Person person = null;
String city = person.getAddress().getCity();
// Error: Cannot invoke "Person.getAddress()" because "person" is null

Address address = person.getAddress();  // address is null
String city = address.getCity();
// Error: Cannot invoke "Address.getCity()" because "address" is null
```

**Benefits:**
- Faster debugging
- Clear error messages
- No code changes needed

---

### 4.8 Compact Number Formatting - Java 12

**Definition:**
Format numbers in compact, human-readable form (e.g., 1K, 1M).

**Example:**
```java
import java.text.NumberFormat;
import java.util.Locale;

NumberFormat compactFormat = NumberFormat.getCompactNumberInstance(
    Locale.US,
    NumberFormat.Style.SHORT
);

compactFormat.format(1000);    // "1K"
compactFormat.format(1000000); // "1M"
compactFormat.format(1500);    // "1.5K"

// Long style
NumberFormat longFormat = NumberFormat.getCompactNumberInstance(
    Locale.US,
    NumberFormat.Style.LONG
);

longFormat.format(1000);    // "1 thousand"
longFormat.format(1000000); // "1 million"
```

---

### 4.9 String Methods Improvements - Java 12

**New Methods:**

**1. indent() - Add indentation:**
```java
String text = "Hello\nWorld";
String indented = text.indent(4);
// "    Hello\n    World\n"
```

**2. transform() - Transform string:**
```java
String result = "hello"
    .transform(String::toUpperCase)
    .transform(s -> s + " WORLD");
// "HELLO WORLD"
```

**3. describeConstable() and resolveConstantDesc():**
```java
// For constant description
Optional<String> desc = "Hello".describeConstable();
```

---

### 4.10 Packaging Tool (jpackage) - Java 14

**Definition:**
Tool to create native application packages (installers).

**Usage:**
```bash
# Create installer
jpackage --input target/ \
    --name MyApp \
    --main-jar myapp.jar \
    --main-class com.example.Main \
    --type app-image

# Platform-specific
jpackage --input target/ \
    --name MyApp \
    --main-jar myapp.jar \
    --type msi  # Windows
    --type dmg  # macOS
    --type deb  # Linux
```

---

### 4.11 Hidden Classes - Java 15

**Definition:**
Classes that cannot be used directly by bytecode of other classes.

**Use Cases:**
- Framework-generated classes
- Dynamic code generation
- Lambda expressions (internal use)

**Example:**
```java
import java.lang.invoke.MethodHandles;

MethodHandles.Lookup lookup = MethodHandles.lookup();
Class<?> hiddenClass = lookup.defineHiddenClass(
    bytecode,
    true  // initialize
);
```

---

### 4.12 Vector API (Incubator) - Java 16

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

### 4.13 Foreign Linker API (Incubator) - Java 16

**Definition:**
API for statically-typed, pure-Java access to native code.

**Note:** Evolved into Foreign Function & Memory API in Java 17.

---

### 4.14 Foreign Function & Memory API (Incubator) - Java 17

**Definition:**
API for interacting with code and data outside the JVM.

**Memory API:**
```java
import jdk.incubator.foreign.*;

try (MemorySegment segment = MemorySegment.allocateNative(100)) {
    // Use native memory
}
```

**Function API:**
```java
import jdk.incubator.foreign.*;

// Link to native functions
```

---

### 4.15 Strong Encapsulation - Java 17

**Definition:**
Strong encapsulation of JDK internals. Access to internal APIs restricted.

**Impact:**
- `--illegal-access=permit` removed
- Internal APIs not accessible
- Use public APIs only

**Migration:**
```bash
# Java 8-16
java --illegal-access=permit MyApp

# Java 17+
# No workaround - use public APIs
```

---

## 📝 5. Detailed Code Examples

### Example 1: Complete Record Usage

```java
// Record definition
public record Person(String name, int age, String email) {
    // Compact constructor with validation
    public Person {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email");
        }
    }
    
    // Custom method
    public boolean isAdult() {
        return age >= 18;
    }
    
    // Static factory method
    public static Person of(String name, int age, String email) {
        return new Person(name, age, email);
    }
}

// Usage
Person person = new Person("Alice", 30, "alice@example.com");
String name = person.name();  // Accessor
boolean adult = person.isAdult();  // Custom method
System.out.println(person);  // Auto-generated toString
```

---

### Example 2: Sealed Classes with Pattern Matching

```java
// Sealed interface
public sealed interface Expr
    permits ConstantExpr, AddExpr, MultiplyExpr {
}

// Permitted implementations
public record ConstantExpr(int value) implements Expr { }
public record AddExpr(Expr left, Expr right) implements Expr { }
public record MultiplyExpr(Expr left, Expr right) implements Expr { }

// Pattern matching in switch
public int evaluate(Expr expr) {
    return switch (expr) {
        case ConstantExpr c -> c.value();
        case AddExpr a -> evaluate(a.left()) + evaluate(a.right());
        case MultiplyExpr m -> evaluate(m.left()) * evaluate(m.right());
    };
}

// Usage
Expr expr = new AddExpr(
    new ConstantExpr(5),
    new MultiplyExpr(
        new ConstantExpr(2),
        new ConstantExpr(3)
    )
);
int result = evaluate(expr);  // 5 + (2 * 3) = 11
```

---

### Example 3: Text Blocks for SQL

```java
public String buildQuery(String tableName, String condition) {
    return """
        SELECT id, name, email
        FROM %s
        WHERE %s
        ORDER BY name
        """.formatted(tableName, condition);
}

// Multi-line JSON
public String createUserJson(String name, int age) {
    return """
        {
          "name": "%s",
          "age": %d,
          "active": true
        }
        """.formatted(name, age);
}
```

---

### Example 4: Pattern Matching with Guards

```java
public String process(Object obj) {
    return switch (obj) {
        case String s when s.length() > 10 -> 
            "Long string: " + s.substring(0, 10) + "...";
        case String s -> "String: " + s;
        case Integer i when i > 0 -> "Positive: " + i;
        case Integer i when i < 0 -> "Negative: " + i;
        case Integer i -> "Zero";
        case Double d when d > 0 -> "Positive double: " + d;
        case null -> "Null value";
        default -> "Unknown: " + obj;
    };
}
```

---

## 🔄 6. Comparison with Related Concepts

### Records vs Classes

| Aspect | Records | Classes |
|--------|---------|---------|
| **Purpose** | Data carrier | General purpose |
| **Immutability** | By default | Manual |
| **Boilerplate** | Minimal | More code |
| **Inheritance** | Cannot extend | Can extend |
| **Fields** | Record components | Instance fields |

---

### Switch Expression vs Switch Statement

| Aspect | Switch Expression | Switch Statement |
|--------|-------------------|-------------------|
| **Returns Value** | Yes | No |
| **Arrow Syntax** | Yes | Optional |
| **Exhaustiveness** | Checked | Not checked |
| **Fall-through** | No | Yes (with break) |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Records** are immutable data carriers
2. **Sealed classes** restrict inheritance
3. **Pattern matching** simplifies instanceof
4. **Switch expressions** return values
5. **Text blocks** preserve formatting

### Best Practices:
- ✅ **Do:** Use records for data carriers
- ✅ **Do:** Use sealed classes for controlled inheritance
- ✅ **Do:** Use pattern matching with instanceof
- ✅ **Do:** Use switch expressions for exhaustive checks
- ✅ **Do:** Use text blocks for multi-line strings
- ❌ **Don't:** Use records for mutable data
- ❌ **Don't:** Bypass sealed class restrictions
- ❌ **Don't:** Use switch expressions when statement is sufficient

---

## 🎯 8. Interview Questions & Answers

### Q1: What are records in Java?
**Answer:**
Records are immutable data carriers introduced in Java 14 (preview) and standardized in Java 16. They automatically generate:
- Constructor
- Accessor methods (not getters)
- equals(), hashCode(), toString()

**Example:**
```java
public record Person(String name, int age) { }

Person person = new Person("Alice", 30);
String name = person.name();  // Accessor
```

**Benefits:**
- Less boilerplate
- Immutable by default
- Value semantics

---

### Q2: What are sealed classes?
**Answer:**
Sealed classes restrict which classes can extend or implement them. They provide controlled inheritance.

**Example:**
```java
public sealed class Shape permits Circle, Rectangle { }
public final class Circle extends Shape { }
public final class Rectangle extends Shape { }
```

**Benefits:**
- Controlled inheritance
- Exhaustiveness in switch
- Better type safety

---

### Q3: What is pattern matching for instanceof?
**Answer:**
Pattern matching simplifies instanceof checks by automatically casting and binding the variable.

**Example:**
```java
// Before
if (obj instanceof String) {
    String str = (String) obj;
    // Use str
}

// After
if (obj instanceof String str) {
    // Use str directly
}
```

---

### Q4: What are switch expressions?
**Answer:**
Switch expressions allow switch to return a value with improved syntax and exhaustiveness checking.

**Example:**
```java
String result = switch (day) {
    case "MONDAY", "TUESDAY" -> "Weekday";
    case "SATURDAY", "SUNDAY" -> "Weekend";
    default -> "Unknown";
};
```

---

### Q5: What are text blocks?
**Answer:**
Text blocks are multi-line string literals that preserve formatting and reduce escape sequences.

**Example:**
```java
String html = """
    <html>
      <body>
        <p>Hello</p>
      </body>
    </html>
    """;
```

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Records with Validation
```java
public record Person(String name, int age) {
    public Person {
        if (age < 0) throw new IllegalArgumentException();
        // Validation in compact constructor
    }
}
```

### Edge Case 2: Sealed Classes in Same File
```java
// No permits clause needed if in same file
public sealed class Shape { }
final class Circle extends Shape { }
final class Rectangle extends Shape { }
```

### Edge Case 3: Pattern Matching Scope
```java
if (obj instanceof String str && str.length() > 0) {
    // str available here
}
// str NOT available here
```

---

## 📊 10. Performance & Complexity

### Performance Considerations:
- **Records:** Minimal overhead, value semantics
- **Pattern Matching:** Compile-time optimization
- **Switch Expressions:** Better than if-else chains
- **Text Blocks:** Compile-time processing

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: Data Transfer Objects (DTOs)
```java
// Use records for DTOs
public record UserDTO(String username, String email, int age) { }
```

### Use Case 2: Algebraic Data Types
```java
// Sealed classes for ADTs
public sealed interface Result<T> permits Success, Failure { }
public record Success<T>(T value) implements Result<T> { }
public record Failure<T>(String error) implements Result<T> { }
```

### Use Case 3: Configuration
```java
// Text blocks for configuration
String config = """
    server.port=8080
    server.host=localhost
    db.url=jdbc:mysql://localhost:3306/mydb
    """;
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. **Oracle Java Documentation** - Java 12-17 Features
2. **Baeldung** - Java 12-17 Features
3. **GeeksforGeeks** - Java 12-17

### Videos:
1. **Java Brains** - Java 12-17 Features
2. **Cave of Programming** - Java 12-17

### Books:
1. **Modern Java in Action** - Java 8-17
2. **Java 17 for Absolute Beginners**

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Use records effectively?
- [ ] Create and use sealed classes?
- [ ] Apply pattern matching?
- [ ] Use switch expressions?
- [ ] Use text blocks?
- [ ] Understand helpful NullPointerExceptions?
- [ ] Answer interview questions confidently?
- [ ] Apply Java 12-17 features in real projects?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. **Records** eliminate boilerplate for data classes
2. **Sealed classes** provide controlled inheritance
3. **Pattern matching** simplifies type checks
4. **Switch expressions** are more expressive
5. **Text blocks** improve multi-line strings

### Questions to Explore Further:
1. How do records work internally?
2. How does pattern matching compile?
3. How do sealed classes enable exhaustiveness?

### Connections to Other Topics:
- Related to: Java 8-11 Features, Modern Java
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

