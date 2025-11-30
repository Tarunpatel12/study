# Study Notes: Collections Framework

**Topic:** Java Collections Framework  
**Date Created:** 2025-11-15  
**Last Updated:** 2025-11-15  
**Status:** ⏳ Not Started  
**Time Spent:** _____ hours

---

## 📖 1. Definition & Core Concept

### What is Collections Framework?
**Java Collections Framework** is a unified architecture for representing and manipulating collections (groups of objects). It provides:
- **Interfaces:** Define contracts (List, Set, Map, Queue)
- **Implementations:** Concrete classes (ArrayList, HashSet, HashMap, etc.)
- **Algorithms:** Methods for searching, sorting, etc.
- **Utilities:** Helper classes (Collections, Arrays)

### Key Concepts:
- **Collection:** Root interface for all collections (except Map)
- **List:** Ordered collection that allows duplicates
- **Set:** Collection that doesn't allow duplicates
- **Map:** Key-value pairs (not a Collection, but part of framework)
- **Queue:** FIFO (First In First Out) collection
- **Iterator:** Traverse through collections

---

## 🎯 2. Why It Exists? (Purpose & Pain Point)

### Problem It Solves:
**Before Collections Framework:**
- Arrays had fixed size
- No standard way to store groups of objects
- Each developer created custom data structures
- No reusable algorithms
- Difficult to work with dynamic data

**After Collections Framework:**
- Dynamic size collections
- Standard interfaces and implementations
- Reusable algorithms (sort, search, etc.)
- Type-safe (with generics)
- Optimized implementations
- Easy to use and maintain

### Real-World Analogy:
**Think of a Library:**
- **Collection:** All books in library
- **List:** Books on a shelf (ordered, can have duplicates)
- **Set:** Unique book titles (no duplicates)
- **Map:** Book catalog (ISBN → Book details)
- **Queue:** Books waiting to be checked out (FIFO)

---

## 🔧 3. How It Works? (Mechanism & Internals)

### Collections Framework Hierarchy:

```
Collection (Interface)
├── List (Interface)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set (Interface)
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue (Interface)
    ├── PriorityQueue
    └── ArrayDeque

Map (Interface - separate from Collection)
├── HashMap
├── LinkedHashMap
├── TreeMap
└── Hashtable
```

### Key Design Principles:
1. **Interface-based:** Program to interfaces, not implementations
2. **Generics:** Type-safe collections
3. **Polymorphism:** Same interface, different implementations
4. **Fail-fast iterators:** Detect concurrent modifications

---

## 💡 4. All Possible Concepts & Sub-Topics

### 4.1 Collection Interface

**Definition:**
Root interface for all collections (except Map). Defines basic operations like add, remove, contains, size, etc.

**Key Methods:**
```java
public interface Collection<E> {
    boolean add(E e);
    boolean remove(Object o);
    boolean contains(Object o);
    int size();
    boolean isEmpty();
    void clear();
    Iterator<E> iterator();
    // ... more methods
}
```

**Example:**
```java
Collection<String> collection = new ArrayList<>();
collection.add("Java");
collection.add("Python");
collection.add("Java");  // Duplicates allowed in Collection

System.out.println(collection.size());  // 3
System.out.println(collection.contains("Java"));  // true
```

**When to Use:** As a reference type when you don't need specific List/Set behavior

---

### 4.2 List Interface

**Definition:**
Ordered collection that allows duplicate elements. Maintains insertion order. Elements can be accessed by index.

**Key Characteristics:**
- Ordered (maintains insertion order)
- Allows duplicates
- Index-based access
- Can have null elements

**Example:**
```java
List<String> list = new ArrayList<>();
list.add("First");
list.add("Second");
list.add("First");  // Duplicate allowed

System.out.println(list.get(0));  // "First" - index access
System.out.println(list.indexOf("First"));  // 0
System.out.println(list.lastIndexOf("First"));  // 2
```

**When to Use:** When you need ordered collection with duplicates and index access

---

### 4.3 ArrayList

**Definition:**
Resizable array implementation of List. Uses dynamic array internally. Fast random access.

**Internal Structure:**
- Uses array internally: `Object[] elementData`
- Default initial capacity: 10
- Grows by 50% when capacity exceeded: `newCapacity = oldCapacity + (oldCapacity >> 1)`
- Not thread-safe

**Key Points:**
- **Time Complexity:**
  - get(index): O(1)
  - add(element): O(1) amortized
  - add(index, element): O(n)
  - remove(index): O(n)
  - contains(): O(n)

**Example:**
```java
ArrayList<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add(0, "First");  // Insert at index 0

System.out.println(list.get(1));  // "A" - O(1) access
list.remove(0);  // Remove by index - O(n)
```

**When to Use:**
- Frequent random access by index
- Mostly adding at end
- Don't need thread-safety

**Internal Working:**
```java
// Simplified ArrayList internals
public class ArrayList<E> {
    private Object[] elementData;  // Internal array
    private int size;
    
    public boolean add(E e) {
        ensureCapacity(size + 1);  // Check if array needs to grow
        elementData[size++] = e;
        return true;
    }
    
    private void ensureCapacity(int minCapacity) {
        if (minCapacity > elementData.length) {
            // Grow array by 50%
            int newCapacity = elementData.length + (elementData.length >> 1);
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
    }
}
```

---

### 4.4 LinkedList

**Definition:**
Doubly linked list implementation of List. Each node has reference to previous and next node.

**Internal Structure:**
- Uses doubly linked list
- Node structure: `Node { E item; Node next; Node prev; }`
- Not thread-safe

**Key Points:**
- **Time Complexity:**
  - get(index): O(n)
  - add(element): O(1)
  - add(index, element): O(n) - need to traverse
  - remove(index): O(n)
  - contains(): O(n)

**Example:**
```java
LinkedList<String> list = new LinkedList<>();
list.add("First");
list.add("Last");
list.addFirst("Before First");  // O(1)
list.addLast("After Last");     // O(1)

System.out.println(list.get(1));  // O(n) - slow!
list.removeFirst();  // O(1)
list.removeLast();   // O(1)
```

**When to Use:**
- Frequent insertions/deletions at beginning/end
- Don't need random access
- Implementing stack/queue

**Internal Working:**
```java
// Simplified LinkedList internals
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
    
    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

---

### 4.5 ArrayList vs LinkedList

| Aspect | ArrayList | LinkedList |
|--------|-----------|------------|
| **Internal Structure** | Dynamic array | Doubly linked list |
| **Random Access** | O(1) - Fast | O(n) - Slow |
| **Insert at End** | O(1) amortized | O(1) |
| **Insert at Beginning** | O(n) | O(1) |
| **Insert at Middle** | O(n) | O(n) |
| **Delete by Index** | O(n) | O(n) |
| **Memory Overhead** | Less (just array) | More (node pointers) |
| **Cache Performance** | Better (contiguous memory) | Worse (scattered) |
| **When to Use** | Random access, mostly appends | Frequent insertions/deletions at ends |

**Decision Guide:**
- Use **ArrayList** when: Random access needed, mostly appending
- Use **LinkedList** when: Frequent insertions/deletions at beginning/end

---

### 4.6 Vector

**Definition:**
Synchronized (thread-safe) version of ArrayList. Legacy class, use ArrayList with Collections.synchronizedList() instead.

**Key Points:**
- Thread-safe (synchronized methods)
- Slower than ArrayList
- Legacy class (Java 1.0)
- Grows by 100% (doubles) when capacity exceeded

**Example:**
```java
Vector<String> vector = new Vector<>();
vector.add("Element");
// All methods are synchronized
```

**When to Use:** Rarely - prefer ArrayList with external synchronization

---

### 4.7 Set Interface

**Definition:**
Collection that doesn't allow duplicate elements. At most one null element.

**Key Characteristics:**
- No duplicates
- No guarantee of order (except LinkedHashSet, TreeSet)
- Mathematical set abstraction

**Example:**
```java
Set<String> set = new HashSet<>();
set.add("Java");
set.add("Python");
set.add("Java");  // Duplicate - ignored

System.out.println(set.size());  // 2
System.out.println(set.contains("Java"));  // true
```

**When to Use:** When you need unique elements, no duplicates

---

### 4.8 HashSet

**Definition:**
Hash table implementation of Set. Uses HashMap internally. No order guarantee.

**Internal Structure:**
- Uses HashMap internally: `HashMap<E, Object> map`
- Stores elements as keys, dummy object as value
- Not thread-safe

**Key Points:**
- **Time Complexity:**
  - add(): O(1) average, O(n) worst
  - remove(): O(1) average, O(n) worst
  - contains(): O(1) average, O(n) worst
- **Load Factor:** 0.75 (default)
- **Initial Capacity:** 16

**Example:**
```java
HashSet<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple");  // Ignored

for (String fruit : set) {
    System.out.println(fruit);  // Order not guaranteed
}
```

**When to Use:**
- Need unique elements
- Don't care about order
- Fast lookups needed

**Internal Working:**
```java
// HashSet uses HashMap internally
public class HashSet<E> {
    private transient HashMap<E,Object> map;
    private static final Object PRESENT = new Object();
    
    public boolean add(E e) {
        return map.put(e, PRESENT) == null;  // Uses HashMap
    }
}
```

---

### 4.9 LinkedHashSet

**Definition:**
HashSet with insertion order maintained. Uses LinkedHashMap internally.

**Key Points:**
- Maintains insertion order
- Uses doubly linked list + hash table
- Slightly slower than HashSet
- Same time complexity as HashSet

**Example:**
```java
LinkedHashSet<String> set = new LinkedHashSet<>();
set.add("First");
set.add("Second");
set.add("Third");

for (String item : set) {
    System.out.println(item);  // Order maintained: First, Second, Third
}
```

**When to Use:** Need unique elements with insertion order

---

### 4.10 TreeSet

**Definition:**
Red-Black tree implementation of Set. Maintains sorted order.

**Internal Structure:**
- Uses TreeMap internally
- Red-Black tree (self-balancing BST)
- Sorted order (natural or Comparator)

**Key Points:**
- **Time Complexity:**
  - add(): O(log n)
  - remove(): O(log n)
  - contains(): O(log n)
- Maintains sorted order
- No null elements allowed

**Example:**
```java
TreeSet<Integer> set = new TreeSet<>();
set.add(5);
set.add(2);
set.add(8);
set.add(1);

for (Integer num : set) {
    System.out.println(num);  // Sorted: 1, 2, 5, 8
}

// Custom ordering
TreeSet<String> customSet = new TreeSet<>((a, b) -> b.compareTo(a));
customSet.add("Zebra");
customSet.add("Apple");
// Sorted descending: Zebra, Apple
```

**When to Use:**
- Need sorted unique elements
- Range queries needed (headSet, tailSet, subSet)

---

### 4.11 Set Comparison

| Aspect | HashSet | LinkedHashSet | TreeSet |
|--------|---------|---------------|---------|
| **Order** | No order | Insertion order | Sorted order |
| **Time Complexity** | O(1) avg | O(1) avg | O(log n) |
| **Null Allowed** | Yes (one) | Yes (one) | No |
| **Thread-Safe** | No | No | No |
| **When to Use** | Fast, no order needed | Fast + insertion order | Sorted needed |

---

### 4.12 Map Interface

**Definition:**
Key-value pairs. Not a Collection, but part of Collections Framework. Each key maps to exactly one value.

**Key Characteristics:**
- Key-value pairs
- No duplicate keys
- One null key allowed (HashMap, LinkedHashMap)
- Multiple null values allowed

**Example:**
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 10);
map.put("Banana", 20);
map.put("Apple", 15);  // Replaces previous value

System.out.println(map.get("Apple"));  // 15
```

**When to Use:** When you need key-value associations

---

### 4.13 HashMap

**Definition:**
Hash table implementation of Map. No order guarantee. Most commonly used Map.

**Internal Structure (Java 8+):**
- **Buckets:** Array of buckets (default: 16)
- **Hash Function:** `hash(key) % bucketCount`
- **Collision Handling:**
  - Java 7: Linked list in each bucket
  - Java 8+: Linked list → Tree (if > 8 elements, treeify)
- **Load Factor:** 0.75 (when 75% full, rehash)
- **Rehashing:** Doubles bucket count, redistributes elements

**Key Points:**
- **Time Complexity:**
  - put(): O(1) average, O(n) worst
  - get(): O(1) average, O(n) worst
  - remove(): O(1) average, O(n) worst
- **Not thread-safe**
- **Allows one null key, multiple null values**

**Example:**
```java
HashMap<String, Integer> map = new HashMap<>();
map.put("One", 1);
map.put("Two", 2);
map.put(null, 0);  // Null key allowed

System.out.println(map.get("One"));  // 1
System.out.println(map.containsKey("Two"));  // true
System.out.println(map.size());  // 3
```

**When to Use:**
- Fast key-value lookups
- No order needed
- Most common use case

**Internal Working (Detailed):**
```java
// Simplified HashMap internals
public class HashMap<K, V> {
    // Bucket array
    Node<K, V>[] table;
    int size;
    float loadFactor = 0.75f;
    int threshold;  // When to rehash
    
    static class Node<K, V> {
        final int hash;
        final K key;
        V value;
        Node<K, V> next;  // For collision (linked list)
    }
    
    public V put(K key, V value) {
        int hash = hash(key);
        int index = hash % table.length;
        
        // Check if key exists
        Node<K, V> node = table[index];
        while (node != null) {
            if (node.key.equals(key)) {
                V oldValue = node.value;
                node.value = value;
                return oldValue;
            }
            node = node.next;
        }
        
        // Add new node
        if (size >= threshold) {
            resize();  // Rehash when load factor exceeded
        }
        // Add to bucket...
    }
    
    private void resize() {
        // Double bucket count
        // Redistribute all elements
    }
}
```

**Collision Handling:**
1. **Hash Collision:** Two keys have same hash code
2. **Java 7:** Store in linked list in same bucket
3. **Java 8+:** If bucket has > 8 elements, convert to balanced tree
4. **Why Tree?** O(log n) vs O(n) for worst case

**Load Factor & Rehashing:**
- **Load Factor 0.75:** When 75% full, rehash
- **Rehashing:** Create new array (2x size), redistribute all elements
- **Why 0.75?** Balance between space and time

---

### 4.14 LinkedHashMap

**Definition:**
HashMap with insertion order or access order maintained. Uses doubly linked list.

**Key Points:**
- Maintains insertion order (default) or access order
- Uses HashMap + doubly linked list
- Slightly slower than HashMap
- Same time complexity

**Example:**
```java
LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
map.put("First", 1);
map.put("Second", 2);
map.put("Third", 3);

for (String key : map.keySet()) {
    System.out.println(key);  // Order: First, Second, Third
}

// Access order mode
LinkedHashMap<String, Integer> accessOrder = new LinkedHashMap<>(16, 0.75f, true);
// true = access order (LRU cache behavior)
```

**When to Use:** Need HashMap with order (insertion or access)

---

### 4.15 TreeMap

**Definition:**
Red-Black tree implementation of Map. Maintains sorted order by keys.

**Key Points:**
- **Time Complexity:** O(log n) for all operations
- Sorted by keys (natural or Comparator)
- No null keys allowed
- Range operations: headMap, tailMap, subMap

**Example:**
```java
TreeMap<String, Integer> map = new TreeMap<>();
map.put("Zebra", 1);
map.put("Apple", 2);
map.put("Banana", 3);

for (String key : map.keySet()) {
    System.out.println(key);  // Sorted: Apple, Banana, Zebra
}

// Range queries
Map<String, Integer> subMap = map.subMap("Apple", "Zebra");
```

**When to Use:** Need sorted key-value pairs, range queries

---

### 4.16 Hashtable

**Definition:**
Legacy synchronized Map. Similar to HashMap but thread-safe and doesn't allow null.

**Key Points:**
- Thread-safe (synchronized methods)
- No null keys or values
- Legacy class (Java 1.0)
- Slower than HashMap

**Example:**
```java
Hashtable<String, Integer> table = new Hashtable<>();
table.put("Key", 1);
// table.put(null, 1);  // ERROR - null not allowed
```

**When to Use:** Rarely - prefer ConcurrentHashMap for thread-safety

---

### 4.17 HashMap vs Hashtable vs LinkedHashMap vs TreeMap

| Aspect | HashMap | Hashtable | LinkedHashMap | TreeMap |
|--------|---------|-----------|---------------|---------|
| **Order** | No order | No order | Insertion/Access order | Sorted order |
| **Null Keys** | One allowed | Not allowed | One allowed | Not allowed |
| **Null Values** | Multiple allowed | Not allowed | Multiple allowed | Allowed |
| **Thread-Safe** | No | Yes | No | No |
| **Time Complexity** | O(1) avg | O(1) avg | O(1) avg | O(log n) |
| **When to Use** | General purpose | Legacy (avoid) | Order needed | Sorted needed |

---

### 4.18 ConcurrentHashMap

**Definition:**
Thread-safe HashMap. Better than Hashtable. Uses segment locking (Java 7) or CAS (Java 8+).

**Key Points:**
- Thread-safe without full synchronization
- Better performance than Hashtable
- Uses fine-grained locking
- Java 8+: Uses CAS (Compare-And-Swap) operations

**Example:**
```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.put("Key", 1);
// Thread-safe operations
```

**When to Use:** Need thread-safe HashMap in multi-threaded environment

---

### 4.19 Queue Interface

**Definition:**
FIFO (First In First Out) collection. Elements added at end, removed from front.

**Key Methods:**
- `offer()`: Add element
- `poll()`: Remove and return head
- `peek()`: Return head without removing

**Example:**
```java
Queue<String> queue = new LinkedList<>();
queue.offer("First");
queue.offer("Second");
queue.offer("Third");

System.out.println(queue.poll());  // "First" - FIFO
System.out.println(queue.poll());  // "Second"
```

**When to Use:** Need FIFO behavior (task scheduling, BFS algorithms)

---

### 4.20 PriorityQueue

**Definition:**
Heap-based priority queue. Elements ordered by natural order or Comparator.

**Key Points:**
- **Time Complexity:**
  - offer(): O(log n)
  - poll(): O(log n)
  - peek(): O(1)
- Not thread-safe
- No null elements

**Example:**
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(5);
pq.offer(2);
pq.offer(8);
pq.offer(1);

System.out.println(pq.poll());  // 1 - smallest first
System.out.println(pq.poll());  // 2

// Max heap (reverse order)
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

**When to Use:** Need priority-based processing (Dijkstra's algorithm, task scheduling)

---

### 4.21 ArrayDeque

**Definition:**
Resizable array implementation of Deque. Can be used as stack or queue.

**Key Points:**
- Faster than Stack and LinkedList
- Can add/remove from both ends
- Not thread-safe
- No null elements

**Example:**
```java
ArrayDeque<String> deque = new ArrayDeque<>();
deque.addFirst("First");
deque.addLast("Last");
deque.addFirst("Before First");

System.out.println(deque.removeFirst());  // "Before First"
System.out.println(deque.removeLast());   // "Last"
```

**When to Use:** Need stack or queue, better performance than Stack/LinkedList

---

### 4.22 Iterator

**Definition:**
Interface to traverse through collections. Provides hasNext(), next(), remove() methods.

**Types:**
1. **Iterator:** Basic iterator
2. **ListIterator:** For Lists, can traverse both directions
3. **Fail-fast:** Throws ConcurrentModificationException if collection modified during iteration

**Example:**
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
list.add("C");

Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String item = it.next();
    System.out.println(item);
    // it.remove();  // Safe removal
}

// ListIterator - bidirectional
ListIterator<String> listIt = list.listIterator();
while (listIt.hasNext()) {
    System.out.println(listIt.next());
}
while (listIt.hasPrevious()) {
    System.out.println(listIt.previous());
}
```

**Fail-Fast Behavior:**
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");

Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String item = it.next();
    list.add("C");  // ConcurrentModificationException!
}
```

**When to Use:** To traverse collections safely

---

### 4.23 Comparable vs Comparator

**Definition:**
Two ways to define ordering for objects.

**Comparable:**
- Interface with `compareTo()` method
- Natural ordering
- Implemented by the class itself
- `Collections.sort(list)` uses Comparable

**Comparator:**
- Separate class/functional interface
- External ordering
- Multiple comparators for same class
- `Collections.sort(list, comparator)` uses Comparator

**Example - Comparable:**
```java
class Student implements Comparable<Student> {
    private String name;
    private int age;
    
    @Override
    public int compareTo(Student other) {
        return this.age - other.age;  // Sort by age
    }
}

List<Student> students = new ArrayList<>();
students.add(new Student("John", 20));
students.add(new Student("Jane", 18));
Collections.sort(students);  // Uses compareTo()
```

**Example - Comparator:**
```java
// Multiple comparators for same class
Comparator<Student> byName = (s1, s2) -> s1.getName().compareTo(s2.getName());
Comparator<Student> byAge = Comparator.comparingInt(Student::getAge);
Comparator<Student> byAgeDesc = (s1, s2) -> s2.getAge() - s1.getAge();

List<Student> students = new ArrayList<>();
Collections.sort(students, byName);   // Sort by name
Collections.sort(students, byAge);    // Sort by age
Collections.sort(students, byAgeDesc); // Sort by age descending
```

**When to Use:**
- **Comparable:** Natural ordering (one way to sort)
- **Comparator:** Multiple orderings, external control

---

### 4.24 Collections Utility Class

**Definition:**
Utility class with static methods for collection operations.

**Key Methods:**
- `sort()`: Sort list
- `reverse()`: Reverse list
- `shuffle()`: Randomize order
- `binarySearch()`: Search in sorted list
- `max()`, `min()`: Find max/min
- `frequency()`: Count occurrences
- `synchronizedCollection()`: Make thread-safe

**Example:**
```java
List<Integer> list = new ArrayList<>();
list.add(5);
list.add(2);
list.add(8);
list.add(1);

Collections.sort(list);  // [1, 2, 5, 8]
Collections.reverse(list);  // [8, 5, 2, 1]
Collections.shuffle(list);  // Random order

int max = Collections.max(list);  // 8
int freq = Collections.frequency(list, 2);  // Count of 2

// Binary search (list must be sorted)
Collections.sort(list);
int index = Collections.binarySearch(list, 5);  // Returns index
```

**When to Use:** Common operations on collections

---

### 4.25 Immutable Collections

**Definition:**
Collections that cannot be modified after creation.

**Ways to Create:**
1. `Collections.unmodifiableList()`: Wrapper (original can change)
2. `List.of()` (Java 9+): Truly immutable
3. `Collections.singletonList()`: Single element immutable list

**Example:**
```java
// Unmodifiable (wrapper)
List<String> mutable = new ArrayList<>();
mutable.add("A");
List<String> unmodifiable = Collections.unmodifiableList(mutable);
// unmodifiable.add("B");  // UnsupportedOperationException
// But mutable.add("B") will reflect in unmodifiable!

// Truly immutable (Java 9+)
List<String> immutable = List.of("A", "B", "C");
// immutable.add("D");  // UnsupportedOperationException
// immutable.set(0, "X");  // UnsupportedOperationException
```

**When to Use:** When you need read-only collections, thread-safety

---

### 4.26 Collections Synchronization

**Definition:**
Making collections thread-safe.

**Methods:**
- `Collections.synchronizedList()`
- `Collections.synchronizedSet()`
- `Collections.synchronizedMap()`

**Example:**
```java
List<String> list = new ArrayList<>();
List<String> syncList = Collections.synchronizedList(list);

// Now thread-safe, but need synchronized blocks for iteration
synchronized(syncList) {
    Iterator<String> it = syncList.iterator();
    while (it.hasNext()) {
        // Safe iteration
    }
}
```

**When to Use:** Need thread-safety (but prefer ConcurrentHashMap for maps)

---

### 4.27 Collections Performance Summary

| Collection | get/contains | add/remove | Index Access | Order |
|------------|---------------|------------|--------------|-------|
| **ArrayList** | O(n) | O(1) end, O(n) middle | O(1) | Insertion |
| **LinkedList** | O(n) | O(1) ends, O(n) middle | O(n) | Insertion |
| **HashSet** | O(1) avg | O(1) avg | N/A | No |
| **LinkedHashSet** | O(1) avg | O(1) avg | N/A | Insertion |
| **TreeSet** | O(log n) | O(log n) | N/A | Sorted |
| **HashMap** | O(1) avg | O(1) avg | N/A | No |
| **LinkedHashMap** | O(1) avg | O(1) avg | N/A | Insertion/Access |
| **TreeMap** | O(log n) | O(log n) | N/A | Sorted |

---

## 📝 5. Detailed Code Examples

### Example 1: Complete Collections Usage

```java
import java.util.*;

public class CollectionsExample {
    public static void main(String[] args) {
        // List - Ordered, allows duplicates
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Apple");  // Duplicate allowed
        System.out.println("List: " + list);  // [Apple, Banana, Apple]
        
        // Set - No duplicates
        Set<String> set = new HashSet<>();
        set.add("Apple");
        set.add("Banana");
        set.add("Apple");  // Ignored
        System.out.println("Set: " + set);  // [Apple, Banana] - order may vary
        
        // Map - Key-value pairs
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 10);
        map.put("Banana", 20);
        map.put("Apple", 15);  // Replaces previous
        System.out.println("Map: " + map);  // {Apple=15, Banana=20}
        
        // Queue - FIFO
        Queue<String> queue = new LinkedList<>();
        queue.offer("First");
        queue.offer("Second");
        System.out.println("Queue poll: " + queue.poll());  // "First"
    }
}
```

---

### Example 2: HashMap Internals Deep Dive

```java
// Understanding HashMap internals
public class HashMapDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> map = new HashMap<>();
        
        // Initial capacity: 16, Load factor: 0.75
        // Threshold: 16 * 0.75 = 12
        // When 12 elements added, rehash happens
        
        for (int i = 0; i < 12; i++) {
            map.put("Key" + i, i);
            // No rehash yet
        }
        
        map.put("Key12", 12);  // Triggers rehash!
        // New capacity: 32, new threshold: 24
        
        // Hash collision example
        // If two keys have same hash % bucketCount
        // They go in same bucket (linked list or tree)
    }
}
```

---

### Example 3: Custom Comparator Examples

```java
class Employee {
    private String name;
    private int age;
    private double salary;
    
    // Constructors, getters...
}

// Multiple comparators
Comparator<Employee> byName = Comparator.comparing(Employee::getName);
Comparator<Employee> byAge = Comparator.comparingInt(Employee::getAge);
Comparator<Employee> bySalary = Comparator.comparingDouble(Employee::getSalary).reversed();

// Chained comparators
Comparator<Employee> byAgeThenSalary = 
    Comparator.comparingInt(Employee::getAge)
              .thenComparingDouble(Employee::getSalary);

List<Employee> employees = new ArrayList<>();
Collections.sort(employees, byAgeThenSalary);
```

---

## 🔄 6. Comparison with Related Concepts

### Collections vs Arrays

| Aspect | Arrays | Collections |
|--------|--------|-------------|
| **Size** | Fixed | Dynamic |
| **Type Safety** | Runtime | Compile-time (generics) |
| **Primitives** | Supported | Wrapper classes only |
| **Performance** | Faster | Slightly slower |
| **Functionality** | Basic | Rich API |

---

## ⚡ 7. Important Points & Best Practices

### Must-Know Points:
1. **Choose right collection:** ArrayList for random access, LinkedList for frequent insertions
2. **HashMap is most common:** Use for key-value lookups
3. **Thread-safety:** Collections are not thread-safe by default
4. **Generics:** Always use generics for type safety
5. **Iterator:** Use for safe traversal, supports remove()

### Best Practices:
- ✅ **Do:** Use generics: `List<String>` not `List`
- ✅ **Do:** Program to interfaces: `List<String> list = new ArrayList<>()`
- ✅ **Do:** Use ArrayList for most List needs
- ✅ **Do:** Use HashMap for most Map needs
- ✅ **Do:** Use Collections utility methods
- ❌ **Don't:** Use Vector or Hashtable (legacy)
- ❌ **Don't:** Modify collection during iteration (use Iterator.remove())
- ❌ **Don't:** Use raw types (without generics)

### Common Mistakes:
1. **Modifying during iteration:** Causes ConcurrentModificationException
2. **Using wrong collection:** ArrayList when LinkedList needed (or vice versa)
3. **Not using generics:** Loses type safety
4. **Assuming thread-safety:** Most collections are not thread-safe
5. **Not handling nulls:** Some collections don't allow null

---

## 🎯 8. Interview Questions & Answers

### Q1: How does HashMap work internally?
**Answer:**
HashMap uses hash table with buckets. When you put a key-value pair:
1. Hash function converts key to hash code
2. Hash code % bucket count = bucket index
3. If collision (same bucket), uses linked list (Java 7) or tree (Java 8+)
4. When load factor (0.75) exceeded, rehashes (doubles buckets)
5. Time complexity: O(1) average, O(n) worst

**Detailed Explanation:** [Use internal working section above]

---

### Q2: Difference between ArrayList and LinkedList?
**Answer:**
**ArrayList:**
- Dynamic array internally
- O(1) random access
- O(n) insert/delete in middle
- Better for random access, mostly appends

**LinkedList:**
- Doubly linked list internally
- O(n) random access
- O(1) insert/delete at ends
- Better for frequent insertions/deletions

**When to use:** ArrayList for most cases, LinkedList for frequent end operations

---

### Q3: What is fail-fast iterator?
**Answer:**
Iterator that throws ConcurrentModificationException if collection is modified during iteration (except through iterator's own remove() method).

**Example:**
```java
List<String> list = new ArrayList<>();
list.add("A");
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    list.add("B");  // ConcurrentModificationException!
    it.next();
}
```

**Solution:** Use iterator's remove() or use CopyOnWriteArrayList

---

### Q4: How to make ArrayList thread-safe?
**Answer:**
1. `Collections.synchronizedList(new ArrayList<>())`
2. `CopyOnWriteArrayList` (for read-heavy scenarios)
3. External synchronization with synchronized blocks

**Best:** Use CopyOnWriteArrayList for read-heavy, synchronizedList for general use

---

## 🔍 9. Edge Cases & Special Scenarios

### Edge Case 1: HashMap with Custom Objects as Keys
**Problem:** Must override equals() and hashCode()
**Solution:**
```java
class Person {
    private String name;
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
    // If hashCode not overridden, HashMap won't work correctly!
}
```

### Edge Case 2: ConcurrentModificationException
**Problem:** Modifying collection during iteration
**Solution:**
```java
// Wrong
for (String item : list) {
    list.remove(item);  // Exception!
}

// Right
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (condition) {
        it.remove();  // Safe
    }
}
```

### Edge Case 3: Null Handling
**Problem:** Some collections don't allow null
**Solution:**
- HashMap: One null key, multiple null values
- Hashtable: No null keys or values
- TreeSet/TreeMap: No null elements
- ArrayDeque: No null elements

---

## 📊 10. Performance & Complexity

### Time Complexity Summary:

**List Operations:**
- ArrayList get(index): O(1)
- ArrayList add(index, element): O(n)
- LinkedList get(index): O(n)
- LinkedList addFirst/Last: O(1)

**Set Operations:**
- HashSet add/contains: O(1) average
- TreeSet add/contains: O(log n)

**Map Operations:**
- HashMap put/get: O(1) average
- TreeMap put/get: O(log n)

### Space Complexity:
- ArrayList: O(n)
- LinkedList: O(n) + pointer overhead
- HashMap: O(n) + bucket array overhead

### Performance Tips:
1. **Pre-size collections:** `new ArrayList<>(100)` if you know size
2. **Use right collection:** Don't use LinkedList for random access
3. **Avoid unnecessary iterations:** Use contains() for Sets
4. **Consider capacity:** For HashMap, set initial capacity if known

---

## 🛠️ 11. Practical Use Cases

### Use Case 1: Word Frequency Counter
```java
Map<String, Integer> frequency = new HashMap<>();
String[] words = {"apple", "banana", "apple", "cherry"};

for (String word : words) {
    frequency.put(word, frequency.getOrDefault(word, 0) + 1);
}
// Result: {apple=2, banana=1, cherry=1}
```

### Use Case 2: Remove Duplicates
```java
List<String> listWithDuplicates = Arrays.asList("A", "B", "A", "C");
Set<String> unique = new LinkedHashSet<>(listWithDuplicates);
List<String> withoutDuplicates = new ArrayList<>(unique);
// Maintains order, removes duplicates
```

### Use Case 3: Grouping by Category
```java
List<Student> students = // ... students list
Map<String, List<Student>> byGrade = new HashMap<>();

for (Student s : students) {
    byGrade.computeIfAbsent(s.getGrade(), k -> new ArrayList<>()).add(s);
}
// Groups students by grade
```

---

## 📚 12. Resources Used

### Articles/Websites:
1. Baeldung - Java Collections (excellent)
2. GeeksforGeeks - Collections Framework
3. Oracle Java Tutorials - Collections
4. JavaTpoint - Collections

### Videos:
1. Java Brains - Collections series
2. Programming with Mosh - Collections

### Books:
1. Effective Java - Collections items
2. Java: The Complete Reference

---

## ✅ 13. Self-Assessment

### Can You:
- [ ] Explain all collection types and when to use each?
- [ ] Explain HashMap internals (buckets, collisions, load factor)?
- [ ] Write code examples for all collection types?
- [ ] Compare ArrayList vs LinkedList vs Vector?
- [ ] Compare HashMap vs Hashtable vs LinkedHashMap vs TreeMap?
- [ ] Explain Comparable vs Comparator?
- [ ] Handle ConcurrentModificationException?
- [ ] Choose right collection for a scenario?
- [ ] Explain time complexity for operations?
- [ ] Answer interview questions confidently?

### Mastery Level: ⭐⭐⭐⭐⭐ (Rate 1-5)

---

## 📝 14. Personal Notes & Insights

### Key Takeaways:
1. HashMap is most important - understand internals deeply
2. Choose collection based on use case, not habit
3. Always use generics for type safety
4. Understand time complexity for performance
5. Thread-safety is not automatic

### Questions to Explore Further:
1. How does TreeMap maintain sorted order internally?
2. What is the difference between fail-fast and fail-safe iterators?
3. How does ConcurrentHashMap achieve thread-safety?

### Connections to Other Topics:
- Related to: Generics, Iterators, Algorithms
- Used with: Streams API, Lambda expressions
- Foundation for: Spring Framework, Hibernate

---

## 🔄 15. Revision Schedule

- **First Review:** [Date]
- **Second Review:** [Date]
- **Third Review:** [Date]

---

**Last Reviewed:** ___________
**Next Review:** ___________

