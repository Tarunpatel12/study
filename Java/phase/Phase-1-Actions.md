# Phase 1: Core Java Refresh - Action Plan (Weeks 1-2)

**Duration:** 2 weeks (14 days)  
**Focus:** OOP depth, Collections, Exceptions, Generics, Java 8-17 features, Streams, basic concurrency  
**Start Date:** 2025-11-15  
**Status:** Ready to Start 🚀

---

## 📊 Phase 1 Overview

### Week 1 Focus Areas
- OOP (abstraction, encapsulation, inheritance, polymorphism), SOLID
- Collections: List/Set/Map, **HashMap internals** ⚠️ CRITICAL GAP
- Exceptions: checked vs unchecked, **try-with-resources** ⚠️ GAP
- **Generics: wildcards, PECS, type erasure** ⚠️ CRITICAL GAP

### Week 2 Focus Areas
- Java 8-17 features: Lambda, Streams, Optional, Date/Time API
- Concurrency: Thread, ExecutorService, Future, CompletableFuture
- **JVM basics: memory model, GC overview** ⚠️ CRITICAL GAP

---

## 🎯 Week 1: OOP, Collections, Exceptions, Generics

### Day 1: OOP Fundamentals + SOLID (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** OOP concepts (abstraction, encapsulation, inheritance, polymorphism)
  - Read: https://www.baeldung.com/java-classes-objects
  - Read: https://www.baeldung.com/java-oop-solid-principles
  - **Focus:** Add Java interface examples (your gap)
- [ ] **Create Topic Template:** `learning-templates/topics/OOP.md`
  - Fill all sections: Definition, Example, Features, Why, Differences, Rapid-fire
- [ ] **Create Interview Answer:** `interview-prep/answers/OOP.md`
  - Use interview answer framework
  - Practice saying it out loud 3 times
- [ ] **Mark Progress:** Topic #1 (OOP) → See topic list at end ✅

**Afternoon (60-90 mins):**
- [ ] **Study:** SOLID Principles
  - Read: SOLID principles explanation
  - **Create examples** for each principle in IntelliJ
  - S: Single Responsibility example
  - O: Open/Closed example
  - L: Liskov Substitution example
  - I: Interface Segregation example
  - D: Dependency Inversion example
- [ ] **Practice:** Write 2-3 OOP examples combining concepts
- [ ] **Mark Progress:** Topic #2 (SOLID Principles) → See topic list at end ✅

**Evening (30 mins):**
- [ ] **Review:** 10 flashcards created today
- [ ] **DSA:** 1-2 easy LeetCode problems (Arrays/Hashing)
- [ ] **Update:** Daily log + tracker.csv
- [ ] **Update:** Topic list completion at end of file

**Goal:** Solid OOP understanding with Java-specific examples

---

### Day 2: HashMap Deep Dive ⚠️ CRITICAL GAP (3-4 hours)

**Morning (90-120 mins):**
- [ ] **Study:** HashMap Internals (YOUR BIGGEST GAP)
  - Read: https://www.baeldung.com/java-hashmap
  - Read: HashMap internal structure article
  - **Must understand:**
    - Bucket array structure
    - Hash function and hash code
    - Collision handling (linked list → tree in Java 8+)
    - Load factor (0.75) and rehashing
    - Time complexity: O(1) average, O(n) worst
- [ ] **Visualize:** Draw HashMap bucket structure
- [ ] **Create Topic Template:** `learning-templates/topics/HashMap.md`
  - Deep dive: Internals, collision handling, load factor
  - Include: Diagrams, code examples, complexity analysis

**Afternoon (60-90 mins):**
- [ ] **Practice:** Write HashMap from scratch (simplified version)
  - Implement basic put(), get(), hash() functions
  - Understand collision handling
- [ ] **Compare:** HashMap vs Hashtable vs LinkedHashMap vs TreeMap
  - Create comparison table
  - When to use each
- [ ] **Create Interview Answer:** `interview-prep/answers/HashMap.md`
  - Complete answer using framework
  - Practice saying it 3 times
- [ ] **Mark Progress:** Topic #3 (HashMap Internals) → See topic list at end ✅

**Evening (30 mins):**
- [ ] **Review:** HashMap flashcards
- [ ] **DSA:** 1-2 LeetCode problems (HashMap-based)
  - Two Sum
  - Group Anagrams
- [ ] **Self-Assess:** Can you explain HashMap internals without looking?
- [ ] **Update:** Topic list completion at end of file

**Goal:** Master HashMap internals - can explain bucket structure, collisions, load factor

---

### Day 3: Generics Deep Dive ⚠️ CRITICAL GAP (3-4 hours)

**Morning (90-120 mins):**
- [ ] **Study:** Generics Fundamentals (YOUR CRITICAL GAP)
  - Read: https://www.baeldung.com/java-generics
  - Read: Type erasure explanation
  - **Must understand:**
    - Type erasure concept
    - Generic classes, methods, interfaces
    - Bounded types (`<T extends Number>`)
    - Wildcards: `?`, `? extends T`, `? super T`
    - PECS principle (Producer Extends, Consumer Super)
- [ ] **Create Topic Template:** `learning-templates/topics/Generics.md`
  - Fill all sections thoroughly
  - Include: Type erasure examples, PECS examples

**Afternoon (60-90 mins):**
- [ ] **Practice:** Write generic code examples
  - Generic class: `class Box<T>`
  - Generic method: `public <T> T getValue()`
  - Bounded types: `class NumberBox<T extends Number>`
  - Wildcards: `List<? extends Number>`, `List<? super Integer>`
  - PECS examples: Producer vs Consumer
- [ ] **Code Analysis:** Understand why this fails:
  ```java
  List<? extends Number> numbers = new ArrayList<Integer>();
  numbers.add(10);  // Why compile error?
  ```
- [ ] **Create Interview Answer:** `interview-prep/answers/Generics.md`
- [ ] **Mark Progress:** Topic #4 (Generics & Type Erasure) → See topic list at end ✅

**Evening (30 mins):**
- [ ] **Review:** Generics flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Self-Assess:** Can you explain type erasure and PECS?
- [ ] **Update:** Topic list completion at end of file

**Goal:** Master Generics - understand type erasure, wildcards, PECS

---

### Day 4: Collections Framework Deep Dive (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** Collections Framework Overview
  - List: ArrayList, LinkedList, Vector
  - Set: HashSet, LinkedHashSet, TreeSet
  - Map: HashMap (review), LinkedHashMap, TreeMap, Hashtable
- [ ] **Compare:** All collection types
  - When to use which?
  - Time complexity for operations
  - Thread-safety considerations
- [ ] **Practice:** Comparable vs Comparator
  - Write examples for both
  - When to use which?

**Afternoon (60 mins):**
- [ ] **Study:** Immutability in Collections
  - Unmodifiable collections
  - Immutable collections (Java 9+)
- [ ] **Practice:** Collections operations
  - Sorting, searching, filtering
- [ ] **Create Topic Template:** `learning-templates/topics/Collections.md`

**Evening (30 mins):**
- [ ] **Review:** Collections flashcards
- [ ] **DSA:** 2-3 LeetCode problems (Collections-based)
- [ ] **Update:** Daily log

**Goal:** Understand all collection types and when to use each

---

### Day 5: Exception Handling (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** Exception Handling Deep Dive
  - Checked vs Unchecked exceptions (review)
  - **Try-with-resources** ⚠️ YOUR GAP
  - AutoCloseable interface
  - Custom exceptions
  - Exception best practices
- [ ] **Practice:** Write try-with-resources examples
  ```java
  try (FileReader fr = new FileReader("file.txt")) {
      // automatically closes
  }
  ```
- [ ] **Create Topic Template:** `learning-templates/topics/ExceptionHandling.md`

**Afternoon (60 mins):**
- [ ] **Practice:** Exception scenarios
  - When to use checked vs unchecked
  - Creating custom exceptions
  - Exception chaining
  - Finally block behavior
- [ ] **Code Analysis:** Exception handling patterns
- [ ] **Create Interview Answer:** `interview-prep/answers/ExceptionHandling.md`

**Evening (30 mins):**
- [ ] **Review:** Exception flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Update:** Daily log

**Goal:** Master exception handling including try-with-resources

---

### Day 6: Week 1 Review + Assessment (2-3 hours)

**Morning (90 mins):**
- [ ] **Review:** All Week 1 topics
  - OOP + SOLID
  - HashMap internals
  - Generics + Type Erasure
  - Collections
  - Exception Handling
- [ ] **Practice:** Interview answers
  - Say each answer out loud
  - Time yourself (2-3 mins each)

**Afternoon (60 mins):**
- [ ] **Assessment:** Take topic-specific assessments
  - HashMap assessment
  - Generics assessment
  - OOP assessment
- [ ] **Self-Evaluate:** 
  - Can you explain without looking?
  - Are interview answers smooth?

**Evening (30 mins):**
- [ ] **DSA:** 2-3 LeetCode problems (mixed)
- [ ] **Plan:** Week 2 goals

**Goal:** Solidify Week 1 knowledge, ready for Week 2

---

### Day 7: Week 1 Wrap-up + LRUCache Implementation (2-3 hours)

**Morning (90 mins):**
- [ ] **Practice:** Implement LRUCache from scratch
  - Use HashMap + Doubly Linked List
  - O(1) get() and put()
  - Practice in IntelliJ
- [ ] **Review:** Any weak areas from Week 1
  - Revisit topics you struggled with
  - Update topic templates

**Afternoon (60 mins):**
- [ ] **DSA:** 3-4 LeetCode problems (Week 1 patterns)
  - Arrays/Hashing/Two Pointers
- [ ] **Review:** All flashcards created this week

**Evening (30 mins):**
- [ ] **Update:** All daily logs
- [ ] **Plan:** Week 2 schedule
- [ ] **Celebrate:** Week 1 completion! 🎉

**Goal:** Complete Week 1, ready for Week 2

---

## 🎯 Week 2: Java 8-17, Streams, Concurrency Basics

### Day 8: Java 8 Lambda Expressions + Functional Interfaces (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** Lambda Expressions
  - Syntax: `(parameters) -> expression`
  - Functional interfaces
  - Method references
- [ ] **Practice:** Convert anonymous classes to lambdas
- [ ] **Create Topic Template:** `learning-templates/topics/Lambda.md`

**Afternoon (60 mins):**
- [ ] **Study:** Built-in Functional Interfaces
  - Predicate, Function, Consumer, Supplier
  - When to use each
- [ ] **Practice:** Write lambda examples for each

**Evening (30 mins):**
- [ ] **Review:** Lambda flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Update:** Daily log

**Goal:** Comfortable with lambda expressions and functional interfaces

---

### Day 9: Java 8 Streams API (3-4 hours)

**Morning (90-120 mins):**
- [ ] **Study:** Streams API Deep Dive
  - What are streams?
  - Stream pipeline (source → intermediate → terminal)
  - Intermediate operations: filter, map, sorted, distinct
  - Terminal operations: collect, forEach, reduce, findFirst
  - Parallel streams
- [ ] **Practice:** Convert loops to streams
  - Before/After examples
- [ ] **Create Topic Template:** `learning-templates/topics/Streams.md`

**Afternoon (60-90 mins):**
- [ ] **Practice:** Stream operations
  - Filtering, mapping, collecting
  - Grouping and partitioning
  - FlatMap operations
- [ ] **Code Challenges:** Stream problems
  - Process lists with streams
  - Parallel processing examples

**Evening (30 mins):**
- [ ] **Review:** Streams flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Self-Assess:** Can you convert any loop to stream?

**Goal:** Master Streams API - can replace loops with streams confidently

---

### Day 10: Optional + Date/Time API (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** Optional Class
  - Why Optional? (avoid NullPointerException)
  - Optional.of(), Optional.ofNullable(), Optional.empty()
  - map(), flatMap(), filter(), orElse()
  - Best practices
- [ ] **Practice:** Refactor nullable code to use Optional

**Afternoon (60 mins):**
- [ ] **Study:** Java 8 Date/Time API
  - LocalDate, LocalTime, LocalDateTime
  - ZonedDateTime, Instant
  - Period, Duration
  - Formatting and parsing
- [ ] **Practice:** Date/time operations

**Evening (30 mins):**
- [ ] **Review:** Optional + Date/Time flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Update:** Daily log

**Goal:** Master Optional and Date/Time API

---

### Day 11: Concurrency Basics - Threads (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** Thread Basics
  - Thread vs Runnable
  - Creating threads
  - Thread lifecycle states ⚠️ YOUR GAP
  - Thread methods: start(), run(), sleep(), join()
- [ ] **Practice:** Create threads
  - Extend Thread class
  - Implement Runnable
- [ ] **Create Topic Template:** `learning-templates/topics/Threads.md`

**Afternoon (60 mins):**
- [ ] **Study:** Thread Communication
  - wait(), notify(), notifyAll()
  - Which class do they belong to? (Object class)
- [ ] **Practice:** Producer-Consumer pattern
- [ ] **Create Interview Answer:** `interview-prep/answers/Threads.md`

**Evening (30 mins):**
- [ ] **Review:** Thread flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Update:** Daily log

**Goal:** Understand thread basics and lifecycle

---

### Day 12: Concurrency - ExecutorService + Future (2-3 hours)

**Morning (60-90 mins):**
- [ ] **Study:** ExecutorService
  - Why use ExecutorService? (better than manual thread creation)
  - Executor, ExecutorService, Executors
  - submit() vs execute()
  - shutdown() vs shutdownNow()
- [ ] **Practice:** Use ExecutorService for task execution

**Afternoon (60 mins):**
- [ ] **Study:** Future Interface
  - Getting results from threads
  - Future.get(), isDone(), cancel()
  - Limitations of Future
- [ ] **Practice:** Async task execution with Future

**Evening (30 mins):**
- [ ] **Review:** ExecutorService flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Update:** Daily log

**Goal:** Master ExecutorService and Future

---

### Day 13: Concurrency - CompletableFuture + JVM Memory ⚠️ CRITICAL GAP (3-4 hours)

**Morning (90-120 mins):**
- [ ] **Study:** CompletableFuture
  - Why better than Future?
  - thenApply(), thenAccept(), thenRun()
  - allOf(), anyOf()
  - Error handling
- [ ] **Practice:** Async orchestration with CompletableFuture
  - Chain multiple async operations
- [ ] **Create Topic Template:** `learning-templates/topics/CompletableFuture.md`

**Afternoon (60-90 mins):**
- [ ] **Study:** JVM Memory Model ⚠️ YOUR CRITICAL GAP
  - What is JVM?
  - Heap memory vs Stack memory
  - What goes where?
    - Objects → Heap
    - Local variables → Stack
    - Static variables → Method area (PermGen/Metaspace)
  - PermGen vs Metaspace (Java 8+)
  - GC basics
- [ ] **Visualize:** Draw memory structure
- [ ] **Create Topic Template:** `learning-templates/topics/JVM-Memory.md`

**Evening (30 mins):**
- [ ] **Review:** CompletableFuture + JVM flashcards
- [ ] **DSA:** 1-2 LeetCode problems
- [ ] **Self-Assess:** Can you explain heap vs stack?

**Goal:** Master CompletableFuture and JVM memory model

---

### Day 14: Week 2 Review + Phase 1 Assessment (2-3 hours)

**Morning (90 mins):**
- [ ] **Review:** All Week 2 topics
  - Lambda, Streams, Optional
  - Date/Time API
  - Threads, ExecutorService, CompletableFuture
  - JVM Memory
- [ ] **Practice:** Interview answers
  - Say each answer out loud
  - Time yourself

**Afternoon (60 mins):**
- [ ] **Assessment:** Phase 1 self-assessment
  - Retake baseline mock questions for Phase 1 topics
  - Compare with original scores
  - Note improvements
- [ ] **Weak Areas:** Identify remaining gaps

**Evening (30 mins):**
- [ ] **DSA:** 3-4 LeetCode problems (mixed)
- [ ] **Review:** All flashcards
- [ ] **Plan:** Phase 2 goals

**Goal:** Complete Phase 1, ready for Phase 2

---

## 📊 Phase 1 Progress Tracking

### Week 1 Checklist
- [ ] OOP + SOLID mastered
- [ ] HashMap internals mastered ⚠️ CRITICAL
- [ ] Generics + Type Erasure mastered ⚠️ CRITICAL
- [ ] Collections framework understood
- [ ] Exception handling mastered (including try-with-resources)
- [ ] LRUCache implemented
- [ ] 15-20 LeetCode problems solved

### Week 2 Checklist
- [ ] Lambda expressions mastered
- [ ] Streams API mastered
- [ ] Optional understood
- [ ] Date/Time API understood
- [ ] Threads + lifecycle understood
- [ ] ExecutorService + Future mastered
- [ ] CompletableFuture understood
- [ ] JVM Memory model mastered ⚠️ CRITICAL
- [ ] 12-15 LeetCode problems solved

---

## 🎯 Critical Gaps Priority (Based on Baseline Mock)

### 🔴 HIGH PRIORITY (Must Master)
1. **HashMap Internals** → Day 2
2. **Generics + Type Erasure** → Day 3
3. **JVM Memory Model** → Day 13

### 🟡 MEDIUM PRIORITY (Important)
1. **Try-with-resources** → Day 5
2. **Thread Lifecycle** → Day 11
3. **Design Patterns** → Week 1-2 (as you study)

---

## 📝 Daily Checklist (Every Day)

- [ ] Study topic (60-120 mins)
- [ ] Create/update topic template
- [ ] Create interview answer
- [ ] Practice saying answer out loud
- [ ] Solve 1-3 LeetCode problems
- [ ] Create flashcards (3-5 per topic)
- [ ] Update daily log
- [ ] Update tracker.csv
- [ ] Plan tomorrow's Top 3

---

## 🔗 Key Resources

### Week 1
- Baeldung: Java Collections
- Baeldung: Java Generics
- Baeldung: Exception Handling
- Effective Java (relevant items)

### Week 2
- Baeldung: Java Streams
- Baeldung: Java Concurrency
- Baeldung: CompletableFuture
- Java Concurrency in Practice (selected chapters)

---

## 📈 Success Criteria

**Phase 1 Complete When:**
- ✅ All critical gaps filled (HashMap, Generics, JVM Memory)
- ✅ Can explain any topic without looking
- ✅ Interview answers are smooth (2-3 mins)
- ✅ 25-35 LeetCode problems solved
- ✅ All topic templates created
- ✅ All interview answers created

---

## 🚀 Start Today!

**Today (Day 1):**
1. Open this plan
2. Start Day 1 tasks
3. Focus on OOP + SOLID
4. Use topic template system
5. Create interview answer
6. Practice saying it

**Remember:**
- One topic at a time
- Deep understanding > rushing
- Practice speaking answers
- Track your progress

---

**Status:** Ready to Start 🚀  
**Next:** Begin Day 1 - OOP Fundamentals

---

## 📚 Phase 1 Topic List - Complete Checklist

**Instructions:** 
1. **When you complete a topic:**
   - Mark checkbox ✅ in this list
   - Update status from "Not Started" to "Completed"
   - Add completion date
   - Also mark completion in daily action items above (they reference this list)
2. **When you start a topic:**
   - Update status to "🔄 In Progress"
3. **Track progress:** Update progress summary at the end of this list weekly

This helps track overall Phase 1 progress.

---

## Week 1 Topics

### Core Java Fundamentals

#### 1. OOP (Object-Oriented Programming) ⏳
- [ ] **Status:** Not Started
- **Description:** Understanding OOP pillars - Abstraction, Encapsulation, Inheritance, Polymorphism. Java interfaces, abstract classes, and real-world examples.
- **Day:** Day 1
- **Critical Gap:** Needs Java-specific examples (interfaces)
- **Completed Date:** ___________

#### 2. SOLID Principles ⏳
- [ ] **Status:** Not Started
- **Description:** Five design principles: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion. Writing examples for each.
- **Day:** Day 1
- **Priority:** High
- **Completed Date:** ___________

#### 3. HashMap Internals 🔴 CRITICAL GAP ⏳
- [ ] **Status:** Not Started
- **Description:** Deep dive into HashMap structure - bucket array, hash function, collision handling (linked list → tree in Java 8+), load factor (0.75), rehashing, time complexity O(1) average.
- **Day:** Day 2
- **Priority:** CRITICAL - Your biggest gap (3/10)
- **Completed Date:** ___________

#### 4. Generics & Type Erasure 🔴 CRITICAL GAP ⏳
- [ ] **Status:** Not Started
- **Description:** Generic classes, methods, interfaces. Type erasure concept, bounded types, wildcards (? extends, ? super), PECS principle (Producer Extends, Consumer Super).
- **Day:** Day 3
- **Priority:** CRITICAL - Complete gap (0/10)
- **Completed Date:** ___________

#### 5. Collections Framework ⏳
- [ ] **Status:** Not Started
- **Description:** Complete Collections overview - List (ArrayList, LinkedList), Set (HashSet, LinkedHashSet, TreeSet), Map (HashMap, LinkedHashMap, TreeMap, Hashtable). When to use each, time complexity, thread-safety.
- **Day:** Day 4
- **Priority:** High
- **Completed Date:** ___________

#### 6. Comparable vs Comparator ⏳
- [ ] **Status:** Not Started
- **Description:** Two ways to define ordering - Comparable (natural ordering) vs Comparator (external ordering). When to use each, examples.
- **Day:** Day 4
- **Priority:** Medium
- **Completed Date:** ___________

#### 7. Exception Handling ⏳
- [ ] **Status:** Not Started
- **Description:** Checked vs unchecked exceptions, exception hierarchy, best practices, custom exceptions, exception chaining.
- **Day:** Day 5
- **Priority:** High
- **Completed Date:** ___________

#### 8. Try-with-Resources 🟡 GAP ⏳
- [ ] **Status:** Not Started
- **Description:** Automatic resource management using try-with-resources, AutoCloseable interface, preventing resource leaks. File I/O examples.
- **Day:** Day 5
- **Priority:** Medium - Your gap (4/10)
- **Completed Date:** ___________

#### 9. LRUCache Implementation ⏳
- [ ] **Status:** Not Started
- **Description:** Implementing LRU Cache from scratch using HashMap + Doubly Linked List. O(1) get() and put() operations.
- **Day:** Day 7
- **Priority:** High - Interview favorite
- **Completed Date:** ___________

---

## Week 2 Topics

### Java 8+ Features

#### 10. Lambda Expressions ⏳
- [ ] **Status:** Not Started
- **Description:** Lambda syntax `(params) -> expression`, converting anonymous classes to lambdas, method references, variable capture.
- **Day:** Day 8
- **Priority:** High
- **Completed Date:** ___________

#### 11. Functional Interfaces ⏳
- [ ] **Status:** Not Started
- **Description:** Built-in functional interfaces - Predicate, Function, Consumer, Supplier, BiFunction. When to use each, examples.
- **Day:** Day 8
- **Priority:** High
- **Completed Date:** ___________

#### 12. Streams API ⏳
- [ ] **Status:** Not Started
- **Description:** Stream pipeline (source → intermediate → terminal), filter/map/sorted operations, collect/forEach/reduce, parallel streams, converting loops to streams.
- **Day:** Day 9
- **Priority:** High - Modern Java essential
- **Completed Date:** ___________

#### 13. Optional Class ⏳
- [ ] **Status:** Not Started
- **Description:** Avoiding NullPointerException with Optional, Optional.of/ofNullable/empty, map/flatMap/filter/orElse operations, best practices.
- **Day:** Day 10
- **Priority:** Medium
- **Completed Date:** ___________

#### 14. Date/Time API (Java 8+) ⏳
- [ ] **Status:** Not Started
- **Description:** New Date/Time API - LocalDate, LocalTime, LocalDateTime, ZonedDateTime, Instant, Period, Duration. Formatting and parsing.
- **Day:** Day 10
- **Priority:** Medium
- **Completed Date:** ___________

### Concurrency

#### 15. Thread Basics 🟡 GAP ⏳
- [ ] **Status:** Not Started
- **Description:** Thread vs Runnable, creating threads, thread lifecycle states (NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED), thread methods.
- **Day:** Day 11
- **Priority:** High - Your gap (4/10)
- **Completed Date:** ___________

#### 16. Thread Communication ⏳
- [ ] **Status:** Not Started
- **Description:** wait(), notify(), notifyAll() methods, which class they belong to (Object), producer-consumer pattern.
- **Day:** Day 11
- **Priority:** Medium
- **Completed Date:** ___________

#### 17. ExecutorService ⏳
- [ ] **Status:** Not Started
- **Description:** Thread pool management, why use ExecutorService over manual threads, Executor/ExecutorService/Executors, submit() vs execute(), shutdown() methods.
- **Day:** Day 12
- **Priority:** High
- **Completed Date:** ___________

#### 18. Future Interface ⏳
- [ ] **Status:** Not Started
- **Description:** Getting results from async operations, Future.get() blocking, isDone(), cancel(), limitations of Future.
- **Day:** Day 12
- **Priority:** Medium
- **Completed Date:** ___________

#### 19. CompletableFuture ⏳
- [ ] **Status:** Not Started
- **Description:** Better alternative to Future, non-blocking operations, thenApply/thenAccept/thenRun, allOf/anyOf, error handling, async orchestration.
- **Day:** Day 13
- **Priority:** High - Modern async Java
- **Completed Date:** ___________

#### 20. JVM Memory Model 🔴 CRITICAL GAP ⏳
- [ ] **Status:** Not Started
- **Description:** What is JVM, Heap vs Stack memory, what goes where (objects→heap, locals→stack, static→method area), PermGen vs Metaspace (Java 8+), GC basics.
- **Day:** Day 13
- **Priority:** CRITICAL - Your gap (2/10)
- **Completed Date:** ___________

---

## 📊 Phase 1 Progress Summary

### Week 1 Progress: 0/9 topics completed (0%)
- [ ] OOP
- [ ] SOLID Principles
- [ ] HashMap Internals 🔴
- [ ] Generics & Type Erasure 🔴
- [ ] Collections Framework
- [ ] Comparable vs Comparator
- [ ] Exception Handling
- [ ] Try-with-Resources 🟡
- [ ] LRUCache Implementation

### Week 2 Progress: 0/11 topics completed (0%)
- [ ] Lambda Expressions
- [ ] Functional Interfaces
- [ ] Streams API
- [ ] Optional Class
- [ ] Date/Time API
- [ ] Thread Basics 🟡
- [ ] Thread Communication
- [ ] ExecutorService
- [ ] Future Interface
- [ ] CompletableFuture
- [ ] JVM Memory Model 🔴

### Overall Phase 1 Progress: 0/20 topics completed (0%)

**Legend:**
- 🔴 = Critical Gap (Must Master)
- 🟡 = Medium Gap (Important)
- ⏳ = Not Started
- ✅ = Completed
- 🔄 = In Progress

---

## 🎯 Critical Topics Priority Order

Based on your baseline assessment, focus on these first:

1. **HashMap Internals** (Day 2) 🔴 - Score: 3/10
2. **Generics & Type Erasure** (Day 3) 🔴 - Score: 0/10
3. **JVM Memory Model** (Day 13) 🔴 - Score: 2/10
4. **Try-with-Resources** (Day 5) 🟡 - Score: 4/10
5. **Thread Lifecycle** (Day 11) 🟡 - Score: 4/10

---

## 📝 How to Use This Checklist

1. **When you start a topic:** Mark it as 🔄 In Progress
2. **When you complete a topic:**
   - Mark checkbox ✅
   - Update status to "Completed"
   - Add completion date
   - Mark it in daily action items
3. **Track progress:** Update progress summary weekly
4. **Review gaps:** Focus on 🔴 critical gaps first

---

**Last Updated:** 2025-11-15  
**Next Topic to Start:** OOP Fundamentals (Day 1)
