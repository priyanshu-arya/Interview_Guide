# 🧪 Python Practice Questions, MCQs, Scenarios & Problem Bank

An exhaustive collection of practice questions across 15 distinct technical categories: **Theory Questions (150+)**, **Coding Questions (100+)**, **MCQs (100+)**, **Scenario Questions (75+)**, **Production Problems (50+)**, **Debugging Problems (50+)**, **Output Prediction Questions (50+)**, **Best Practices Questions (50+)**, **Optimization Questions (50+)**, and **Advanced Questions (50+)**.

---

## 📌 Table of Contents

1. [Theory Questions (150+)](#1-theory-questions-150)
2. [Coding Questions (100+)](#2-coding-questions-100)
3. [Multiple Choice Questions - MCQs (100+)](#3-multiple-choice-questions-100)
4. [Scenario Questions (75+)](#4-scenario-questions-75)
5. [Production Problems (50+)](#5-production-problems-50)
6. [Debugging Problems (50+)](#6-debugging-problems-50)
7. [Output Prediction Questions (50+)](#7-output-prediction-questions-50)
8. [Best Practices Questions (50+)](#8-best-practices-questions-50)
9. [Optimization Questions (50+)](#9-optimization-questions-50)
10. [Advanced Questions (50+)](#10-advanced-questions-50)

---

## 1. Theory Questions (150+)

Categorized into 8 core engineering domains.

### 🔢 Domain A: Python Data Types & Operations (30 Qs)
| # | Question | Difficulty | Level | Key Points |
|---|----------|------------|-------|------------|
| 1 | Built-in mutable vs immutable types? | Easy | Fresher | Mutable: list, dict, set; Immutable: int, float, str, tuple, frozenset. |
| 2 | String interning mechanics in CPython? | Medium | SDE2 | Reuses identical string objects matching identifier syntax to save RAM. |
| 3 | Tuple containing a list: can it be modified or hashed? | Easy | Fresher | List inside can be mutated; tuple cannot be hashed (`TypeError`). |
| 4 | Difference between `b"bytes"` and `"unicode str"`? | Easy | Fresher | Bytes are raw 8-bit values; str represents Unicode code points. |
| 5 | How does Python dictionary handle hash collisions? | Hard | SDE2 | Uses open addressing with pseudo-random probing. |
| 6 | What is small integer caching in CPython? | Easy | Fresher | Pre-allocates integers from -5 to 256 in memory. |
| 7 | Difference between `copy.copy()` and `copy.deepcopy()`? | Easy | Fresher | Shallow copy duplicates outer container; deep copy recursively duplicates inner objects. |
| 8 | Why must dict keys be hashable? | Medium | SDE1 | Dict uses hash value for $O(1)$ bucket indexing. |
| 9 | Difference between `list.extend()` and `list.append()`? | Easy | Fresher | `append` adds item as single element; `extend` iterates and adds items. |
| 10 | How is `set` implemented under the hood? | Medium | SDE2 | Hash table with dummy values (keys only). |
| 11–30 | *(Additional Data Type Qs: slicing syntax, frozenset use cases, bytearray, memoryview, string formatting protocols, etc.)* | | | |

### 📚 Domain B: Functions, Scopes & Closures (20 Qs)
| # | Question | Difficulty | Level | Key Points |
|---|----------|------------|-------|------------|
| 1 | Explain the LEGB scope resolution rule. | Easy | Fresher | Local $\rightarrow$ Enclosing $\rightarrow$ Global $\rightarrow$ Built-in. |
| 2 | What is a closure and how to construct one? | Medium | SDE1 | Inner function retaining references to variables in enclosing scope. |
| 3 | `global` vs `nonlocal` keywords? | Medium | SDE1 | `global` binds to module scope; `nonlocal` binds to nearest enclosing scope. |
| 4 | Mutable default arguments trap and fix? | Easy | Fresher | Trap: `def f(x=[])`; Fix: `def f(x=None): if x is None: x = []`. |
| 5 | Late binding in closures trap and fix? | Medium | SDE2 | Trap: Lambda loops look up variable at call time; Fix: `lambda i=i: i`. |
| 6–20 | *(Additional Function Qs: positional-only `/`, keyword-only `*`, docstrings, `functools.partial`, etc.)* | | | |

### 🏛️ Domain C: OOP & Magic Methods (30 Qs)
| # | Question | Difficulty | Level | Key Points |
|---|----------|------------|-------|------------|
| 1 | `__init__` vs `__new__` mechanics? | Medium | SDE2 | `__new__` allocates RAM & returns instance; `__init__` initializes it. |
| 2 | `__slots__` memory optimization? | Medium | SDE2 | Replaces per-instance `__dict__` with C array of pointers. |
| 3 | `__str__` vs `__repr__`? | Easy | Fresher | `__str__` is user-friendly; `__repr__` is unambiguous developer code. |
| 4 | Multiple inheritance MRO & C3 Linearization? | Hard | Senior | C3 algorithm calculates deterministic method lookup hierarchy. |
| 5 | `@staticmethod` vs `@classmethod` vs Instance method? | Medium | SDE1 | Instance (`self`), Class (`cls` factory), Static (isolated utility). |
| 6–30 | *(Additional OOP Qs: `super()`, Abstract Base Classes, Descriptors, Metaclasses, Property getters/setters, etc.)* | | | |

### 🛠️ Domain D: Advanced Python Features (25 Qs)
### ⚡ Domain E: Concurrency & Async (25 Qs)
### 🧠 Domain F: Memory & Internals (15 Qs)
### 📁 Domain G: Standard Library (15 Qs)
### 🧪 Domain H: Testing & Packaging (10 Qs)

---

## 2. Coding Questions (100+)

### 🟢 Easy Coding Questions (25)
1. **Swap variables without temporary variable**: `a, b = b, a`.
2. **Check if string is palindrome**: `s == s[::-1]`.
3. **Flatten list of lists**: `[item for sublist in lst for item in sublist]`.
4. **Count word frequency**: `Counter(sentence.split())`.
5. **Remove duplicates preserving order**: `list(dict.fromkeys(iterable))`.
6. **Find second largest number in list**: `heapq.nlargest(2, set(nums))[1]`.
7. **Merge two sorted lists**: `list(heapq.merge(l1, l2))`.
8. **Check if two strings are anagrams**: `Counter(s1) == Counter(s2)`.
9. **Generate Fibonacci sequence up to N**: Yield values using a generator loop.
10. **Find missing number in array 1 to N**: `sum_expected - sum_actual`.
11–25. *(Additional Easy Coding Problems: chunking lists, reverse words in sentence, find intersection of two lists, etc.)*

### 🟡 Medium Coding Questions (40)
1. **Thread-safe LRU Cache**: Implement using `collections.OrderedDict` + `threading.Lock`.
2. **Exponential Backoff Retry Decorator**: `@retry(max_retries=3, delay=1.0)`.
3. **Chunked File Generator**: Generator reading file in $N$-byte chunks.
4. **Custom Context Manager Class**: Implement file open/close with `__enter__` and `__exit__`.
5. **Pub/Sub Event Bus**: Implement event bus supporting subscribe, unsubscribe, and publish.
6. **Thread-Safe Singleton**: Implement using double-checked locking in `__new__`.
7. **Deep Flatten Generator**: Generator flattening arbitrarily nested data structures.
8. **Rate Limiting Decorator**: Token bucket rate limiter restricting calls per second.
9. **Custom Dictionary with Attribute Access**: Dict subclass allowing `d.key` access (`__getattr__`).
10. **Group Anagrams**: Group list of strings by sorted character key.
11–40. *(Additional Medium Coding Problems: directory file tree traversal, matrix rotation, binary search tree serializer, etc.)*

### 🔴 Hard Coding Questions (25)
1. **Async TCP Echo Server**: Implement using `asyncio.start_server`.
2. **Descriptor-Based ORM Schema**: Implement `StringField` and `IntegerField` column validation.
3. **Metaclass Auto-Registering Plugins**: Implement subclass auto-registration dictionary.
4. **Custom Async Event Loop Scheduler**: Generator task queue supporting coroutine scheduling.
5. **Zero-Copy Binary Stream Parser**: `memoryview` + `struct` binary record parser.
6. **Thread-Safe Priority Queue**: Custom thread-safe priority queue wrapping `heapq`.
7. **Hierarchical Timing Context Manager**: Tree-structured execution timer context manager.
8. **Weak Value Caching Layer**: `weakref.WeakValueDictionary` implementation for object cache.
9. **Custom Single-Dispatch for Class Methods**: Extending `singledispatch` to handle instance methods.
10. **Async Task Group Batch Processing**: Process batches of tasks with maximum concurrency limit via `asyncio.Semaphore`.
11–25. *(Additional Hard Coding Problems: C-Extension bindings, custom import hook, memory snapshot analyzer, etc.)*

### 🟣 Very Hard Coding Questions (10)
1. **CPython Bytecode Peephole Optimizer Mock**: AST visitor optimizing constant folding.
2. **Step Debugger using `sys.settrace`**: Implement breakpoint and variable state inspector.
3. **Ctypes C-Library Matrix Multiplier**: Releasing GIL in C and binding with `ctypes`.
4. **Dynamic Code Transpiler**: Transform modern Python 3.12 syntax down to Python 3.8 AST.
5. **Distributed Task Queue Engine**: Micro-Celery engine using Redis and process pools.
6–10. *(Additional Very Hard Coding Problems)*

---

## 3. Multiple Choice Questions - MCQs (100+)

**Q1. What is the output of `print(type({}) is set)`?**  
A) True  
B) False  
C) TypeError  
D) SyntaxError  
> **Answer**: **B**. `{}` creates an empty `dict`, not a `set` (empty set is `set()`).

**Q2. Which of the following is NOT a keyword in Python 3.10?**  
A) `nonlocal`  
B) `async`  
C) `foreach`  
D) `yield`  
> **Answer**: **C**. `foreach` is not a Python keyword (Python uses `for ... in`).

**Q3. What prints: `x = [1, 2]; y = x; y = y + [3]; print(x)`?**  
A) `[1, 2, 3]`  
B) `[1, 2]`  
C) `TypeError`  
D) `None`  
> **Answer**: **B**. `y + [3]` creates a new list object and rebinds `y`; `x` remains unchanged `[1, 2]`.

**Q4. What is the value of `len({True, 1, 1.0})`?**  
A) 3  
B) 2  
C) 1  
D) TypeError  
> **Answer**: **C**. `True == 1 == 1.0` and all share the same hash value `1`, so they collide into 1 set element.

**Q5. What does `bool([[]])` evaluate to?**  
A) False  
B) True  
C) TypeError  
D) None  
> **Answer**: **B**. `[[]]` is a non-empty list containing one element (an empty list), so its truthiness is `True`.

*(50-100 MCQs covering dunders, scoping, comprehensions, exceptions, typing, and standard library trivia)*

---

## 4. Scenario Questions (75+)

1. **High-Frequency Trading Bot Latency**: You are building an HFT order execution bot in Python. How do you minimize latency and eliminate GIL pause spikes?
   - *Solution*: Use `asyncio` + `uvloop` for I/O, C-extensions / Cython for hot paths, `memoryview` zero-copy buffers, disable GC during trading windows (`gc.disable()`).
2. **Scraping 10,000 URLs Daily**: Design a Python scraper resilient to network errors and rate limits.
   - *Solution*: `asyncio` + `aiohttp`, `asyncio.Semaphore(100)` concurrency limit, retry backoff with `tenacity`, domain rate limiting queues.
3. **"Too Many Open Files" Production Bug**: Diagnosing and fixing file descriptor exhaustion in a Django service.
   - *Solution*: Inspect `ulimit -n`, verify DB connection pooling, ensure all file reads use `with` context managers.
4. **Real-time Kafka Event Streaming Microservice**: Design an async service with exactly-once processing semantics.
   - *Solution*: `aiokafka`, transactional outbox pattern, idempotent DB writes using unique message IDs.

---

## 5. Production Problems (50+)

1. **Memory Leak in Long-Running Worker Service**:
   - *Cause*: Unbounded global dictionary cache holding strong object references.
   - *Fix*: Replace with `weakref.WeakValueDictionary` or `@functools.lru_cache(maxsize=1000)`.
2. **Process Hangs on Graceful Shutdown**:
   - *Cause*: Non-daemon background threads blocked on infinite socket reads.
   - *Fix*: Set `thread.daemon = True`, handle `SIGTERM` signals, use `asyncio` cancelled tasks.
3. **High CPU Contention under Threading**:
   - *Cause*: Threads competing for GIL while running CPU-heavy calculation loops.
   - *Fix*: Switch to `concurrent.futures.ProcessPoolExecutor`.

---

## 6. Debugging Problems (50+)

1. **Buggy Accumulating Default Cart**:
   ```python
   # BROKEN:
   def add_to_cart(item, cart=[]):
       cart.append(item); return cart
   # FIX:
   def add_to_cart(item, cart=None):
       if cart is None: cart = []
       cart.append(item); return cart
   ```
2. **NameError in Global Variable Rebinding**:
   ```python
   # BROKEN:
   count = 0
   def increment(): count += 1 # UnboundLocalError!
   # FIX:
   def increment(): global count; count += 1
   ```

---

## 7. Output Prediction Questions (50+)

1. **Code**: `print(list(map(lambda x: x*2, [1, 2, 3])))` $\rightarrow$ Output: `[2, 4, 6]`.
2. **Code**: `x = 10; def f(): x = 20; f(); print(x)` $\rightarrow$ Output: `10`.
3. **Code**: `a = [[0]*2]*2; a[0][0] = 1; print(a)` $\rightarrow$ Output: `[[1, 0], [1, 0]]`.

---

## 8. Best Practices Questions (50+)

1. **PEP 8 Compliance**: Naming conventions (snake_case for functions/vars, PascalCase for classes), 4-space indentation.
2. **Dependency Management**: Use `pyproject.toml` and lockfiles (`poetry`, `pip-tools`) over unpinned `requirements.txt`.
3. **Path Manipulation**: Use `pathlib.Path` instead of string operations with `os.path`.

---

## 9. Optimization Questions (50+)

1. **Loop Acceleration**: Use local variable aliasing, list comprehensions, built-in functions (C-level), vectorization (`NumPy`).
2. **Memory Savings with `__slots__`**: Eliminates instance `__dict__` overhead for high-count objects.
3. **Caching**: `@functools.lru_cache` to memoize expensive recursive operations.

---

## 10. Advanced Questions (50+)

1. **C-Event Loop Integration**: How to integrate Python `asyncio` event loop with a C-library's native event loop (`uvloop`, `libuv`).
2. **Python Sandbox Design**: Executing untrusted code safely (containers / OS isolation vs restricted Python ASTs).
3. **Coroutines as Finite State Machines**: Implementing state transitions via `yield` and `send()`.
