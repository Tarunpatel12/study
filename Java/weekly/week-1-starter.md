# Week 1: OOP, Collections, Exception Handling, Generics

## Today's Study Plan (Deep Work 1)

### Core Topics (Pick ONE per day)

#### Day 1: OOP + SOLID
**Resources:**
- https://www.baeldung.com/java-classes-objects
- https://www.baeldung.com/java-oop-solid-principles

**Key Points:**
1. Abstraction vs Encapsulation
   - Abstraction: hiding implementation details (interface/abstract class)
   - Encapsulation: bundling data + methods (private fields, public methods)

2. Inheritance vs Polymorphism
   - Inheritance: reuse via extends
   - Polymorphism: same interface, different behaviors (overriding/overloading)

3. SOLID Principles (Memorize!)
   - S: Single Responsibility
   - O: Open/Closed
   - L: Liskov Substitution
   - I: Interface Segregation
   - D: Dependency Inversion

**Flashcards to Create (3):**
1. "What's the difference between abstraction and encapsulation?"
2. "Explain the SOLID principles with examples"
3. "When would you use an interface vs abstract class?"

---

#### Day 2: Collections Deep Dive
**Resources:**
- https://www.baeldung.com/java-collections
- https://www.baeldung.com/java-hashmap

**Key Points:**
1. List vs Set vs Map
   - List: ordered, allows duplicates (ArrayList, LinkedList)
   - Set: unique elements (HashSet, LinkedHashSet, TreeSet)
   - Map: key-value pairs (HashMap, LinkedHashMap, TreeMap)

2. HashMap Internals (IMPORTANT!)
   - Bucket array + linked lists/balanced trees (Java 8+)
   - Load factor 0.75, rehashing, equals() + hashCode()
   - Time complexity: O(1) average, O(n) worst

3. Comparable vs Comparator
   - Comparable: natural ordering (implements Comparable<T>)
   - Comparator: external ordering (create Comparator<T>)

**Flashcards to Create (3):**
1. "How does HashMap work internally?"
2. "When to use Comparable vs Comparator?"
3. "Explain ArrayList vs LinkedList trade-offs"

---

#### Day 3: Exception Handling
**Resources:**
- https://www.baeldung.com/java-exceptions
- https://www.baeldung.com/java-checked-unchecked-exceptions

**Key Points:**
1. Checked vs Unchecked
   - Checked: must handle (IOException, SQLException)
   - Unchecked: optional (NullPointerException, IllegalArgumentException)

2. Best Practices
   - Don't swallow exceptions
   - Use specific exceptions
   - Create custom exceptions when needed
   - Log before re-throwing

3. Try-with-resources (Java 7+)
   - Auto-closes resources
   - Must implement AutoCloseable

**Flashcards to Create (3):**
1. "Difference between checked and unchecked exceptions?"
2. "When should you create a custom exception?"
3. "What happens if both try and finally throw exceptions?"

---

#### Day 4: Generics
**Resources:**
- https://www.baeldung.com/java-generics
- https://www.baeldung.com/java-generics-variance

**Key Points:**
1. Wildcards (?)
   - `<?>`: unbounded wildcard
   - `<? extends T>`: upper bounded (PECS: Producer)
   - `<? super T>`: lower bounded (PECS: Consumer)

2. Type Erasure
   - Generics removed at compile time
   - Runtime doesn't know generic types

3. PECS Principle
   - Producer Extends, Consumer Super

**Flashcards to Create (3):**
1. "Explain PECS principle with example"
2. "What is type erasure in Java?"
3. "When would you use `? extends T` vs `? super T`?"

---

## Practice Problems (Deep Work 2 - DSA)

### Week 1 LeetCode Problems (15-20 total)

**Day 1-2: Arrays & Hashing**
1. Two Sum (Easy) - https://leetcode.com/problems/two-sum/
2. Contains Duplicate (Easy) - https://leetcode.com/problems/contains-duplicate/
3. Valid Anagram (Easy) - https://leetcode.com/problems/valid-anagram/
4. Group Anagrams (Medium) - https://leetcode.com/problems/group-anagrams/
5. Top K Frequent Elements (Medium) - https://leetcode.com/problems/top-k-frequent-elements/

**Day 3-4: Two Pointers**
6. Valid Palindrome (Easy) - https://leetcode.com/problems/valid-palindrome/
7. Two Sum II (Medium) - https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/
8. Container With Most Water (Medium) - https://leetcode.com/problems/container-with-most-water/
9. 3Sum (Medium) - https://leetcode.com/problems/3sum/

**Day 5: Practice Custom Implementations**
10. Implement LRUCache (Medium) - https://leetcode.com/problems/lru-cache/
11. Rate Limiter (Design) - Use token bucket algorithm

---

## Implementation Practice

### Day 5: Implement LRUCache
```java
public class LRUCache {
    // Task: Implement a cache with Least Recently Used eviction policy
    // Requirements:
    // 1. put(key, value) - O(1) time
    // 2. get(key) - O(1) time
    // 3. Capacity limit
    
    // Hint: Use HashMap + Doubly Linked List
    
    // Try yourself first! Then we'll review.
}
```

### Expected Complexity
- Time: O(1) for both operations
- Space: O(capacity)

---

## Week 1 Goals Checklist

By end of Week 1, you should be able to:
- [ ] Explain OOP concepts confidently
- [ ] Memorize and apply SOLID principles
- [ ] Understand HashMap internals thoroughly
- [ ] Choose between Comparable/Comparator
- [ ] Handle exceptions properly
- [ ] Understand generics and wildcards
- [ ] Solve 15-20 LeetCode problems
- [ ] Implement LRUCache from scratch

---

## Daily Practice Routine

**Morning (Deep Work 1):**
1. Read one topic (20 min)
2. Create 3 flashcards (10 min)
3. Write short code examples (30 min)

**Afternoon (Deep Work 2 - DSA):**
1. Solve 2-3 LeetCode problems (60 min)
2. Review 1 past mistake (15 min)
3. Update patterns notebook (15 min)

**Evening:**
1. Review 10 flashcards
2. Update tracker.csv
3. Plan tomorrow's Top 3

---

## Questions to Reflect On

1. Can you explain HashMap collision resolution without looking?
2. What's the difference between `ArrayList<Object>` and `ArrayList<?>`?
3. When would you catch vs propagate an exception?
4. How does polymorphism work at runtime?

---

Next: Week 2 will cover Java 8+ features (Streams, Lambda, Optional) and Concurrency basics.

