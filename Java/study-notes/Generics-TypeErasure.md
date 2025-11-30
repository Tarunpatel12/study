# Study Notes: Generics, Wildcards, PECS, Type Erasure

**Topic:** Java Generics (Wildcards, PECS, Type Erasure)  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What are Generics?
**Generics** enable types (classes and interfaces) to be parameters when defining classes, interfaces, and methods. They provide type safety at compile time and eliminate the need for type casting.

### Key Concepts:
- **Type Parameter:** Placeholder for a type (e.g., `T`, `E`, `K`, `V`)
- **Generic Class:** Class that uses type parameters
- **Generic Method:** Method that uses type parameters
- **Wildcard:** Unknown type represented by `?`
- **Type Erasure:** Process where generic type information is removed at runtime
- **PECS:** Producer Extends, Consumer Super principle

### Why Generics?
**Before Generics (Java 1.4 and earlier):**
```java
List list = new ArrayList();
list.add("Hello");
list.add(123);  // No type safety!
String str = (String) list.get(0);  // Need casting
Integer num = (Integer) list.get(1);  // Runtime error if wrong type!
```

**With Generics (Java 5+):**
```java
List<String> list = new ArrayList<>();
list.add("Hello");
// list.add(123);  // Compile-time error!
String str = list.get(0);  // No casting needed
```

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
**Without Generics:**
- No type safety at compile time
- Need explicit type casting
- Runtime ClassCastException errors
- Code is less readable
- No way to enforce type constraints

**With Generics:**
- Type safety at compile time
- No need for type casting
- Catches errors at compile time
- Better code readability
- Reusable code with type parameters

### Real-World Analogy:
**Think of a Container:**
- **Without Generics:** A box that can hold anything (unsafe, need to check what's inside)
- **With Generics:** A labeled box that only holds specific type (safe, compiler enforces)

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Generic Type System:
```
Generic Class/Interface
    ↓
Type Parameter (T, E, K, V)
    ↓
Type Argument (String, Integer, etc.)
    ↓
Type Erasure (at runtime)
```

### Type Erasure Process:
1. **Compile Time:** Generic types are checked
2. **Type Erasure:** Generic type information removed
3. **Runtime:** Only raw types exist
4. **Bridge Methods:** Generated to maintain polymorphism

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Basic Generics

**Generic Class:**
```java
// Generic class with type parameter T
class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// Usage
Box<String> stringBox = new Box<>();
stringBox.setItem("Hello");
String value = stringBox.getItem();  // No casting needed

Box<Integer> intBox = new Box<>();
intBox.setItem(123);
Integer num = intBox.getItem();
```

**Generic Interface:**
```java
interface Container<T> {
    void add(T item);
    T get(int index);
    int size();
}

class ListContainer<T> implements Container<T> {
    private List<T> items = new ArrayList<>();
    
    @Override
    public void add(T item) {
        items.add(item);
    }
    
    @Override
    public T get(int index) {
        return items.get(index);
    }
    
    @Override
    public int size() {
        return items.size();
    }
}
```

**Generic Method:**
```java
class Utils {
    // Generic method
    public static <T> T getFirst(List<T> list) {
        return list.isEmpty() ? null : list.get(0);
    }
    
    // Generic method with multiple type parameters
    public static <T, U> U convert(T input, Function<T, U> converter) {
        return converter.apply(input);
    }
}

// Usage
List<String> strings = Arrays.asList("A", "B", "C");
String first = Utils.getFirst(strings);  // Type inferred

List<Integer> numbers = Arrays.asList(1, 2, 3);
Integer firstNum = Utils.getFirst(numbers);  // Type inferred
```

---

### 4.2 Bounded Type Parameters

**Upper Bounded (extends):**
```java
// T must be Number or its subclass
class NumberBox<T extends Number> {
    private T number;
    
    public void setNumber(T number) {
        this.number = number;
    }
    
    public double getDoubleValue() {
        return number.doubleValue();  // Can call Number methods
    }
}

// Usage
NumberBox<Integer> intBox = new NumberBox<>();  // OK
NumberBox<Double> doubleBox = new NumberBox<>();  // OK
// NumberBox<String> stringBox = new NumberBox<>();  // ERROR!

// Multiple bounds
class MultiBound<T extends Number & Comparable<T>> {
    // T must be Number AND implement Comparable
}
```

**Lower Bounded (super):**
```java
// Used in method parameters (see PECS section)
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

---

### 4.3 Wildcards

**Definition:**
Wildcard (`?`) represents an unknown type. Used when you don't know or don't care about the specific type.

**Unbounded Wildcard (`?`):**
```java
// Accepts any type
public void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}

// Usage
List<String> strings = Arrays.asList("A", "B");
List<Integer> numbers = Arrays.asList(1, 2, 3);
printList(strings);   // OK
printList(numbers);  // OK
```

**Key Point:** Can only read as Object, cannot add (except null)

---

### 4.4 Upper Bounded Wildcard (`? extends T`)

**Definition:**
Wildcard with upper bound. Accepts type T or any of its subtypes.

**Example:**
```java
// Accepts List of Number or any subclass
public void processNumbers(List<? extends Number> numbers) {
    for (Number num : numbers) {
        System.out.println(num.doubleValue());
    }
}

// Usage
List<Integer> integers = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);
List<Long> longs = Arrays.asList(1L, 2L, 3L);

processNumbers(integers);  // OK
processNumbers(doubles);   // OK
processNumbers(longs);      // OK
// processNumbers(Arrays.asList("A", "B"));  // ERROR!
```

**Important Rules:**
- **Can read:** Yes, as the upper bound type
- **Cannot add:** No (except null) - don't know exact type

**Why Cannot Add?**
```java
List<? extends Number> numbers = new ArrayList<Integer>();
// numbers.add(new Integer(1));  // ERROR!
// numbers.add(new Double(1.0)); // ERROR!

// Why? Compiler doesn't know if list is List<Integer> or List<Double>
// Adding Double to List<Integer> would be wrong!
```

---

### 4.5 Lower Bounded Wildcard (`? super T`)

**Definition:**
Wildcard with lower bound. Accepts type T or any of its supertypes.

**Example:**
```java
// Accepts List of Integer or any supertype
public void addIntegers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

// Usage
List<Number> numbers = new ArrayList<>();
List<Object> objects = new ArrayList<>();
List<Integer> integers = new ArrayList<>();

addIntegers(numbers);   // OK - Number is supertype of Integer
addIntegers(objects);   // OK - Object is supertype of Integer
addIntegers(integers);  // OK - Integer is Integer
// addIntegers(new ArrayList<Double>());  // ERROR!
```

**Important Rules:**
- **Can add:** Yes, elements of type T or its subtypes
- **Can read:** Only as Object (don't know exact type)

**Why Can Add?**
```java
List<? super Integer> list = new ArrayList<Number>();
list.add(1);  // OK - Integer is subtype of Number
list.add(2);  // OK - Integer is subtype of Number
// Can safely add Integer to any supertype of Integer
```

---

### 4.6 PECS Principle (Producer Extends, Consumer Super)

**Definition:**
**PECS** = **P**roducer **E**xtends, **C**onsumer **S**uper

- **Producer:** Produces/reads data → Use `? extends T`
- **Consumer:** Consumes/writes data → Use `? super T`

**Producer Example (extends):**
```java
// Producer - produces/reads elements
public void printNumbers(List<? extends Number> numbers) {
    for (Number num : numbers) {  // Reading/producing
        System.out.println(num);
    }
    // Cannot add - don't know exact type
}

// Usage
List<Integer> integers = Arrays.asList(1, 2, 3);
printNumbers(integers);  // OK - reading from list
```

**Consumer Example (super):**
```java
// Consumer - consumes/writes elements
public void addNumbers(List<? super Integer> list) {
    list.add(1);  // Writing/consuming
    list.add(2);
    list.add(3);
}

// Usage
List<Number> numbers = new ArrayList<>();
addNumbers(numbers);  // OK - writing to list
```

**Combined Example:**
```java
// Copy from producer to consumer
public static <T> void copy(List<? extends T> source, List<? super T> dest) {
    for (T item : source) {  // Read from source (producer)
        dest.add(item);      // Write to dest (consumer)
    }
}

// Usage
List<Integer> source = Arrays.asList(1, 2, 3);
List<Number> dest = new ArrayList<>();
copy(source, dest);  // Copy Integer list to Number list
```

**Memory Aid:**
- **PECS:** "Get from Producer, Put into Consumer"
- **Producer Extends:** Get elements (read)
- **Consumer Super:** Put elements (write)

---

### 4.7 Type Erasure

**Definition:**
Process where generic type information is removed at runtime. All generic types become their raw types or upper bounds.

**How It Works:**
1. **Compile Time:** Generic types are checked
2. **Type Erasure:** Remove type parameters
3. **Replace with bounds:** Replace with Object or upper bound
4. **Add casts:** Insert type casts where needed
5. **Bridge methods:** Generate bridge methods for polymorphism

**Example 1: Unbounded Type Parameter**
```java
// Source code
class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// After type erasure (what JVM sees)
class Box {
    private Object item;  // T replaced with Object
    
    public void setItem(Object item) {
        this.item = item;
    }
    
    public Object getItem() {
        return item;
    }
}

// Usage
Box<String> box = new Box<>();
box.setItem("Hello");
String value = box.getItem();  // Compiler inserts: (String) box.getItem()
```

**Example 2: Bounded Type Parameter**
```java
// Source code
class NumberBox<T extends Number> {
    private T number;
    
    public T getNumber() {
        return number;
    }
}

// After type erasure
class NumberBox {
    private Number number;  // T replaced with Number (upper bound)
    
    public Number getNumber() {
        return number;
    }
}
```

**Example 3: Generic Methods**
```java
// Source code
public static <T> T getFirst(List<T> list) {
    return list.isEmpty() ? null : list.get(0);
}

// After type erasure
public static Object getFirst(List list) {
    return list.isEmpty() ? null : list.get(0);
}

// Usage
String first = getFirst(stringList);
// Compiler inserts: (String) getFirst(stringList)
```

**Implications:**
1. **No runtime type information:** Cannot check `instanceof` with generic types
2. **Cannot create arrays:** `new T[]` is not allowed
3. **Cannot instantiate:** `new T()` is not allowed
4. **Overloading restrictions:** Cannot overload based on type parameters

---

### 4.8 Type Erasure Limitations

**1. Cannot Use instanceof with Generic Types:**
```java
// ERROR: Cannot use instanceof with parameterized types
if (obj instanceof List<String>) {  // Compile error!
    // ...
}

// Workaround: Use raw type
if (obj instanceof List) {
    List<?> list = (List<?>) obj;
    // Work with wildcard
}
```

**2. Cannot Create Generic Arrays:**
```java
// ERROR: Cannot create generic array
T[] array = new T[10];  // Compile error!

// Workaround: Use ArrayList or Object array with casting
List<T> list = new ArrayList<>();
// or
Object[] array = new Object[10];
T[] typedArray = (T[]) array;  // Unchecked cast warning
```

**3. Cannot Instantiate Type Parameter:**
```java
// ERROR: Cannot instantiate type parameter
T item = new T();  // Compile error!

// Workaround: Use factory pattern or reflection
public static <T> T createInstance(Class<T> clazz) throws Exception {
    return clazz.newInstance();
}
```

**4. Cannot Overload Based on Type Parameters:**
```java
// ERROR: Both methods have same erasure
public void method(List<String> list) { }
public void method(List<Integer> list) { }  // Compile error!

// After erasure, both become: method(List list)
```

**5. Cannot Use Primitive Types:**
```java
// ERROR: Cannot use primitives
List<int> numbers;  // Compile error!

// Must use wrapper classes
List<Integer> numbers;  // OK
```

---

### 4.9 Raw Types

**Definition:**
Raw type is a generic class or interface used without type arguments. Legacy code compatibility.

**Example:**
```java
// Raw type (no type parameter)
List list = new ArrayList();
list.add("Hello");
list.add(123);  // No type safety!

// Parameterized type
List<String> stringList = new ArrayList<>();
stringList.add("Hello");
// stringList.add(123);  // Compile error!
```

**Why Raw Types Exist:**
- Backward compatibility with pre-Java 5 code
- Allows mixing raw and parameterized types (with warnings)

**Unchecked Warnings:**
```java
List rawList = new ArrayList();
List<String> stringList = rawList;  // Unchecked warning
```

---

### 4.10 Generic Classes and Inheritance

**Parameterized Types Are Not Covariant:**
```java
// Arrays are covariant
Integer[] integers = new Integer[10];
Number[] numbers = integers;  // OK - arrays are covariant
numbers[0] = 1.5;  // Runtime error! (ArrayStoreException)

// Generic types are invariant
List<Integer> integers = new ArrayList<>();
// List<Number> numbers = integers;  // ERROR! Generic types are invariant

// Why? Prevents type safety issues
// If allowed: numbers.add(1.5) would add Double to List<Integer>!
```

**Wildcards Provide Flexibility:**
```java
// Upper bounded wildcard - covariant behavior
List<Integer> integers = new ArrayList<>();
List<? extends Number> numbers = integers;  // OK
// Cannot add to numbers (type safety)

// Lower bounded wildcard - contravariant behavior
List<Number> numbers = new ArrayList<>();
List<? super Integer> integers = numbers;  // OK
// Can add Integer to integers
```

---

### 4.11 Multiple Type Parameters

**Example:**
```java
// Class with multiple type parameters
class Pair<K, V> {
    private K key;
    private V value;
    
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    
    public K getKey() {
        return key;
    }
    
    public V getValue() {
        return value;
    }
}

// Usage
Pair<String, Integer> pair = new Pair<>("Age", 25);
String key = pair.getKey();
Integer value = pair.getValue();
```

**Common Type Parameter Names:**
- `T` - Type
- `E` - Element (collections)
- `K` - Key (maps)
- `V` - Value (maps)
- `N` - Number
- `S`, `U`, `V` - Second, third, fourth types

---

### 4.12 Generic Methods vs Wildcards

**When to Use Generic Methods:**
```java
// Generic method - when you need type parameter in multiple places
public static <T> void swap(List<T> list, int i, int j) {
    T temp = list.get(i);
    list.set(i, list.get(j));
    list.set(j, temp);
}
// Type T used in multiple places, need to maintain same type
```

**When to Use Wildcards:**
```java
// Wildcard - when you don't need to refer to type parameter
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
// Don't need to know or use the specific type
```

**Rule of Thumb:**
- Use **generic method** when you need to use type parameter multiple times
- Use **wildcard** when you only need to accept any type

---

### 4.13 Recursive Type Bounds

**Definition:**
Type parameter that is bounded by itself.

**Example:**
```java
// T must be Comparable to itself
class MaxFinder<T extends Comparable<T>> {
    public T findMax(T[] array) {
        if (array.length == 0) {
            return null;
        }
        T max = array[0];
        for (T item : array) {
            if (item.compareTo(max) > 0) {
                max = item;
            }
        }
        return max;
    }
}

// Usage
Integer[] numbers = {3, 1, 4, 1, 5};
MaxFinder<Integer> finder = new MaxFinder<>();
Integer max = finder.findMax(numbers);  // 5
```

**Common Use Case:**
- `Comparable<T>` - class implements Comparable to itself
- `Enum<E extends Enum<E>>` - enum type definition

---

### 4.14 Generic Constructors

**Example:**
```java
class Container<T> {
    private T item;
    
    // Generic constructor
    public <U extends T> Container(U item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// Usage
Container<Number> container = new Container<>(10);  // Integer extends Number
// Type inference: U = Integer, T = Number
```

---

### 4.15 Generic Static Methods

**Example:**
```java
class Utils {
    // Generic static method
    public static <T> List<T> createList(T... items) {
        List<T> list = new ArrayList<>();
        for (T item : items) {
            list.add(item);
        }
        return list;
    }
    
    // Generic static method with bounds
    public static <T extends Comparable<T>> T max(T a, T b) {
        return a.compareTo(b) > 0 ? a : b;
    }
}

// Usage
List<String> strings = Utils.createList("A", "B", "C");
Integer max = Utils.max(10, 20);  // 20
```

---

### 4.16 Wildcard Capture

**Definition:**
When compiler needs to infer a type for a wildcard, it "captures" it as a type variable.

**Example:**
```java
// Wildcard capture helper method
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);
}

// Helper method captures wildcard
private static <T> void swapHelper(List<T> list, int i, int j) {
    T temp = list.get(i);
    list.set(i, list.get(j));
    list.set(j, temp);
}

// Why needed? Cannot directly write to List<?>
// Helper method captures ? as T, allowing writes
```

---

### 4.17 Generic Arrays Workaround

**Problem:**
Cannot create generic arrays directly.

**Solutions:**

**1. Use ArrayList:**
```java
// Best solution
List<T> list = new ArrayList<>();
```

**2. Use Object Array with Casting:**
```java
@SuppressWarnings("unchecked")
T[] array = (T[]) new Object[size];
```

**3. Use Reflection:**
```java
@SuppressWarnings("unchecked")
T[] array = (T[]) Array.newInstance(clazz, size);
```

**Example:**
```java
class GenericArray<T> {
    private T[] array;
    
    @SuppressWarnings("unchecked")
    public GenericArray(Class<T> clazz, int size) {
        array = (T[]) Array.newInstance(clazz, size);
    }
    
    public void set(int index, T item) {
        array[index] = item;
    }
    
    public T get(int index) {
        return array[index];
    }
}
```

---

### 4.18 Type Erasure and Reflection

**Getting Generic Type Information:**
```java
// Type erasure removes generic info, but can use reflection
class StringList extends ArrayList<String> { }

// Get actual type argument
Type type = StringList.class.getGenericSuperclass();
// type = java.util.ArrayList<java.lang.String>

// Using ParameterizedType
if (type instanceof ParameterizedType) {
    ParameterizedType pt = (ParameterizedType) type;
    Type[] actualTypes = pt.getActualTypeArguments();
    // actualTypes[0] = String.class
}
```

**Gson Example (Real-world use):**
```java
// Gson uses TypeToken to preserve generic type
Type listType = new TypeToken<List<String>>() {}.getType();
List<String> list = gson.fromJson(json, listType);
```

---

## 📝 5. Detailed Code Examples

### Example 1: Complete Generic Class

```java
// Generic stack implementation
class Stack<T> {
    private List<T> elements = new ArrayList<>();
    
    public void push(T item) {
        elements.add(item);
    }
    
    public T pop() {
        if (elements.isEmpty()) {
            throw new EmptyStackException();
        }
        return elements.remove(elements.size() - 1);
    }
    
    public T peek() {
        if (elements.isEmpty()) {
            throw new EmptyStackException();
        }
        return elements.get(elements.size() - 1);
    }
    
    public boolean isEmpty() {
        return elements.isEmpty();
    }
    
    public int size() {
        return elements.size();
    }
}

// Usage
Stack<String> stringStack = new Stack<>();
stringStack.push("First");
stringStack.push("Second");
String top = stringStack.pop();  // "Second"

Stack<Integer> intStack = new Stack<>();
intStack.push(1);
intStack.push(2);
Integer num = intStack.pop();  // 2
```

---

### Example 2: PECS in Collections

```java
// Copy method using PECS
public static <T> void copy(List<? extends T> source, List<? super T> dest) {
    for (T item : source) {  // Producer - read from source
        dest.add(item);      // Consumer - write to dest
    }
}

// Usage
List<Integer> source = Arrays.asList(1, 2, 3);
List<Number> dest = new ArrayList<>();
copy(source, dest);  // Copy Integer list to Number list

// max method using Producer Extends
public static <T extends Comparable<? super T>> T max(List<? extends T> list) {
    if (list.isEmpty()) {
        throw new IllegalArgumentException("List is empty");
    }
    T max = list.get(0);
    for (T item : list) {  // Producer - reading from list
        if (item.compareTo(max) > 0) {
            max = item;
        }
    }
    return max;
}
```

---

### Example 3: Type Erasure Demonstration

```java
// Source code
class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// What compiler generates (after type erasure)
class Box {
    private Object item;
    
    public void setItem(Object item) {
        this.item = item;
    }
    
    public Object getItem() {
        return item;
    }
}

// Usage
Box<String> box = new Box<>();
box.setItem("Hello");
String value = box.getItem();

// What actually happens:
// 1. box.setItem("Hello") -> box.setItem((Object) "Hello")
// 2. String value = box.getItem() -> String value = (String) box.getItem()
// Compiler inserts casts automatically
```

---

## 🔄 6. Comparison with Related Concepts

### Generic Types vs Raw Types

| Aspect | Generic Types | Raw Types |
|--------|---------------|-----------|
| **Type Safety** | Compile-time | None |
| **Casting** | Not needed | Required |
| **Compile Errors** | Yes | No (runtime errors) |
| **Code Clarity** | High | Low |

---

### Upper Bound vs Lower Bound Wildcards

| Aspect | `? extends T` | `? super T` |
|--------|---------------|-------------|
| **Accepts** | T and subtypes | T and supertypes |
| **Can Read** | As T | As Object |
| **Can Write** | No (except null) | Yes (T and subtypes) |
| **Use Case** | Producer (read) | Consumer (write) |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Type erasure** removes generic info at runtime
2. **PECS:** Producer Extends, Consumer Super
3. **Wildcards** provide flexibility without type parameters
4. **Generic types are invariant** (not covariant like arrays)
5. **Cannot use primitives** with generics

### Best Practices:
- ✅ **Do:** Use generics for type safety
- ✅ **Do:** Follow PECS principle
- ✅ **Do:** Use meaningful type parameter names (T, E, K, V)
- ✅ **Do:** Use upper bounds when needed
- ✅ **Do:** Use wildcards when you don't need type parameter
- ❌ **Don't:** Use raw types (except legacy code)
- ❌ **Don't:** Use `?` when you need type parameter
- ❌ **Don't:** Mix raw and parameterized types

### Common Mistakes:
1. **Forgetting PECS:** Using wrong wildcard
2. **Raw types:** Using List instead of List<String>
3. **Type erasure confusion:** Trying to use instanceof with generics
4. **Array creation:** Trying to create generic arrays
5. **Overloading:** Trying to overload based on type parameters

---

## 🎯 8. Interview Questions & Answers

### Q1: What is type erasure in Java generics?
**Answer:**
Type erasure is the process where generic type information is removed at runtime. The compiler removes all generic type information and replaces type parameters with their bounds (Object if unbounded).

**Example:**
```java
// Source
List<String> list = new ArrayList<>();

// After erasure (runtime)
List list = new ArrayList();  // Generic info removed
```

**Implications:**
- Cannot use `instanceof` with generic types
- Cannot create generic arrays
- Cannot instantiate type parameters
- No runtime type information

---

### Q2: Explain PECS principle.
**Answer:**
**PECS** = **P**roducer **E**xtends, **C**onsumer **S**uper

- **Producer Extends (`? extends T`):** Use when you're reading/producing elements
  - Can read as T
  - Cannot add (except null)

- **Consumer Super (`? super T`):** Use when you're writing/consuming elements
  - Can add T and its subtypes
  - Can only read as Object

**Example:**
```java
// Producer - reading
public void print(List<? extends Number> numbers) {
    for (Number n : numbers) { }  // Reading
}

// Consumer - writing
public void add(List<? super Integer> list) {
    list.add(1);  // Writing
}
```

---

### Q3: What is the difference between `List<?>` and `List<Object>`?
**Answer:**
- **`List<?>`:** Unbounded wildcard, can hold any type, but cannot add (except null)
- **`List<Object>`:** Specific type, can hold only Object, can add any Object

**Example:**
```java
List<?> wildcard = new ArrayList<String>();
// wildcard.add("Hello");  // ERROR - cannot add

List<Object> objectList = new ArrayList<Object>();
objectList.add("Hello");  // OK
objectList.add(123);      // OK
```

**Key Difference:**
- `List<?>` is more flexible (accepts any List)
- `List<Object>` is specific (only List<Object>)

---

### Q4: Why can't we create generic arrays?
**Answer:**
Because of type erasure and array covariance issues.

**Problem:**
```java
// If allowed:
List<String>[] array = new List<String>[10];
Object[] objArray = array;  // Arrays are covariant
objArray[0] = new ArrayList<Integer>();  // Would add wrong type!
String str = array[0].get(0);  // ClassCastException!
```

**Solution:** Use `ArrayList` instead of arrays.

---

### Q5: What is wildcard capture?
**Answer:**
When compiler needs to work with a wildcard type, it "captures" it as a type variable in a helper method.

**Example:**
```java
// Cannot directly swap List<?>
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);  // Delegate to helper
}

// Helper captures wildcard as T
private static <T> void swapHelper(List<T> list, int i, int j) {
    T temp = list.get(i);
    list.set(i, list.get(j));
    list.set(j, temp);
}
```

---

### Q6: Can you use primitives with generics?
**Answer:**
No, generics only work with reference types. Must use wrapper classes.

**Example:**
```java
// ERROR
List<int> numbers;

// OK
List<Integer> numbers;
```

**Why?** Type erasure replaces with Object, and primitives cannot be Object.

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: Multiple Bounds
**Problem:** Need type to extend multiple classes/interfaces
**Solution:** Use `&` to combine bounds (only one class allowed)

```java
class MultiBound<T extends Number & Comparable<T> & Serializable> {
    // T must be Number AND Comparable AND Serializable
}
```

---

### Edge Case 2: Generic Method Type Inference
**Problem:** Type inference fails in some cases
**Solution:** Explicitly specify type

```java
// Type inference
String result = Utils.<String>getFirst(list);  // Explicit type
// vs
String result = Utils.getFirst(list);  // Inferred type
```

---

### Edge Case 3: Wildcard in Return Types
**Problem:** Returning wildcard types
**Solution:** Usually avoid, use type parameter instead

```java
// Avoid
public List<?> getList() { }

// Better
public <T> List<T> getList() { }
```

---

## 📊 10. Performance & Complexity

### Performance Considerations:
- **No runtime overhead:** Type erasure means no performance cost
- **Compile-time only:** All type checking at compile time
- **Casts inserted:** Compiler adds casts, minimal overhead
- **Same as raw types:** Runtime performance identical to raw types

### Complexity:
- **Compile-time complexity:** More complex type checking
- **Code clarity:** Generics improve readability
- **Maintenance:** Easier to maintain with type safety

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: Generic Repository Pattern
```java
interface Repository<T, ID> {
    T findById(ID id);
    List<T> findAll();
    void save(T entity);
    void delete(ID id);
}

class UserRepository implements Repository<User, Long> {
    @Override
    public User findById(Long id) {
        // Implementation
    }
    // ...
}
```

---

### Use Case 2: Generic Builder Pattern
```java
class Builder<T> {
    private T item;
    
    public Builder<T> setItem(T item) {
        this.item = item;
        return this;
    }
    
    public T build() {
        return item;
    }
}

// Usage
String result = new Builder<String>()
    .setItem("Hello")
    .build();
```

---

### Use Case 3: Generic Utility Methods
```java
class CollectionUtils {
    // Find max using PECS
    public static <T extends Comparable<? super T>> T max(Collection<? extends T> coll) {
        Iterator<? extends T> it = coll.iterator();
        T max = it.next();
        while (it.hasNext()) {
            T next = it.next();
            if (next.compareTo(max) > 0) {
                max = next;
            }
        }
        return max;
    }
    
    // Copy using PECS
    public static <T> void copy(List<? extends T> source, List<? super T> dest) {
        for (T item : source) {
            dest.add(item);
        }
    }
}
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. **Oracle Java Tutorials** - Generics
2. **Baeldung** - Java Generics
3. **GeeksforGeeks** - Generics in Java
4. **Angelika Langer** - Java Generics FAQ

### Videos:
1. **Java Brains** - Generics
2. **Cave of Programming** - Generics

### Books:
1. **Effective Java** by Joshua Bloch - Generics chapter
2. **Java Generics and Collections** by Naftalin & Wadler

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Explain type erasure and its implications?
- [ ] Apply PECS principle correctly?
- [ ] Use wildcards appropriately?
- [ ] Create generic classes and methods?
- [ ] Understand bounded type parameters?
- [ ] Explain why generic arrays are not allowed?
- [ ] Use generic methods vs wildcards correctly?
- [ ] Handle type erasure limitations?
- [ ] Answer interview questions confidently?
- [ ] Apply generics in real projects?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. **Type erasure** is fundamental - understand it deeply
2. **PECS** is crucial for collections - memorize it
3. **Wildcards** provide flexibility - use when appropriate
4. **Generic types are invariant** - unlike arrays
5. **No runtime generic info** - all compile-time

### Questions to Explore Further:
1. How do frameworks like Spring use generics?
2. How does Hibernate handle generic types?
3. What are reified generics (future Java feature)?

### Connections to Other Topics:
- Related to: Collections Framework, Type System
- Used in: Spring Framework, Hibernate, Streams API
- Foundation for: Type-safe programming

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

