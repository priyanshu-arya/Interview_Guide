# ❓ Top 45 Python Interview Questions & Detailed Answers

This document contains 45 of the most frequently asked, high-yield Python interview questions across FAANG, Unicorns, FinTech, and AI infrastructure companies. Each entry includes difficulty rating, interviewer motivation, deep technical breakdown, executable code examples, follow-up questions, and interview tips.

---

## 📋 Table of Contents & Quick Index

1. [Mutable vs Immutable Types & Memory Hashability](#q1-mutable-vs-immutable-types)
2. [CPython Garbage Collection & Reference Counting](#q2-garbage-collection)
3. [Global Interpreter Lock (GIL) Mechanics & Workarounds](#q3-global-interpreter-lock-gil)
4. [`@staticmethod` vs `@classmethod` vs Instance Methods](#q4-method-types)
5. [Execution Time Measurement Decorator](#q5-execution-timer-decorator)
6. [`*args` and `**kwargs` Unpacking Protocol](#q6-args-and-kwargs)
7. [Generators, `yield`, and Lazy Evaluation](#q7-generators-and-yield)
8. [List Comprehension vs Generator Expression](#q8-list-comprehension-vs-generator-expression)
9. [Context Managers & Custom Protocol Implementation](#q9-context-managers)
10. [Shallow Copy vs Deep Copy Mechanics](#q10-shallow-vs-deep-copy)
11. [Pass-by-Assignment Parameter Passing](#q11-pass-by-assignment)
12. [Monkey Patching: Usage, Risks, and Mocking](#q12-monkey-patching)
13. [Object Identity (`is`) vs Equality (`==`)](#q13-is-vs-equals)
14. [Exception Handling Hierarchy & Custom Exceptions](#q14-exception-handling)
15. [Thread-Safe LRU Cache & Caching Decorators](#q15-lru-cache-decorator)
16. [`asyncio.gather()` vs `asyncio.wait()`](#q16-asyncio-gather-vs-wait)
17. [Metaclasses & Class Creation Protocol](#q17-metaclasses)
18. [`__slots__` Memory Optimization](#q18-slots-optimization)
19. [Python Import System & `sys.path`](#q19-import-system)
20. [Thread-Safe Singleton Pattern](#q20-thread-safe-singleton)
21. [`threading.Thread` vs `multiprocessing.Process`](#q21-threading-vs-multiprocessing)
22. [Type Hinting & Static Type Checking (`mypy`)](#q22-type-hinting)
23. [Profiling and Performance Optimization](#q23-profiling-and-optimization)
24. [`with` Statement (`__enter__`/`__exit__`) Protocol](#q24-with-statement-protocol)
25. [Descriptor Protocol (`__get__`, `__set__`)](#q25-descriptors)
26. [Multiple Inheritance, Diamond Problem & MRO](#q26-mro-and-diamond-problem)
27. [Weak References (`weakref`) & Cache Leaks](#q27-weak-references)
28. [Late Binding in Closures & Lambdas](#q28-closure-late-binding)
29. [Decorators with Arguments (3-Level Nesting)](#q29-parameterized-decorators)
30. [Coroutines: `send()`, `throw()`, `close()`](#q30-coroutine-methods)
31. [`__getattr__` vs `__getattribute__`](#q31-getattr-vs-getattribute)
32. [`__init__` vs `__new__`](#q32-init-vs-new)
33. [`functools.wraps` Metadata Preservation](#q33-functools-wraps)
34. [Zero-Copy Operations with `memoryview`](#q34-zero-copy-memoryview)
35. [`contextvars` in Async Execution](#q35-contextvars-in-async)
36. [Structural Pattern Matching (`match`/`case`)](#q36-pattern-matching)
37. [`__init_subclass__` Class Creation Hook](#q37-init-subclass-hook)
38. [Abstract Base Classes (`abc.ABC`, `@abstractmethod`)](#q38-abstract-base-classes)
39. [Async Context Managers (`__aenter__`/`__aexit__`)](#q39-async-context-managers)
40. [`functools.singledispatch` Generic Functions](#q40-singledispatch)
41. [Memory Leak Detection with `tracemalloc`](#q41-tracemalloc)
42. [`subprocess.Popen` Process Streaming & Deadlocks](#q42-subprocess-popen)
43. [Structural Subtyping via `typing.Protocol`](#q43-typing-protocol)
44. [`dataclass` Fields & `default_factory`](#q44-dataclass-default-factory)
45. [Global vs Nonlocal Scope Mutability](#q45-global-vs-nonlocal)

---

### <a id="q1-mutable-vs-immutable-types"></a>1. Mutable vs Immutable Types & Memory Hashability
**Difficulty**: Easy  | **Level**: Fresher  
**Why asked**: Tests fundamental mental model of Python memory, side effects, and hash table stability.

**Detailed Answer**:
In Python, every variable holds a reference to an object in memory. Objects are classified as **mutable** (state can change after instantiation) or **immutable** (state cannot be modified).
- **Immutable Types**: `int`, `float`, `str`, `tuple`, `frozenset`, `bytes`, `bool`.
- **Mutable Types**: `list`, `dict`, `set`, `bytearray`.

Immutability directly determines **hashability**. An object is hashable (`__hash__`) if its hash value never changes during its lifetime and it can be compared for equality (`__eq__`). Only hashable objects can serve as dictionary keys or set elements.

```python
# Modifying mutable object modifies all referencing variables
a = [1, 2, 3]
b = a
b.append(4)
print(a) # Output: [1, 2, 3, 4]

# Attempting to mutate immutable string creates new allocation
s1 = "hello"
s2 = s1
s1 += " world" # Rebinds s1 to new string object
print(s2) # Output: "hello"
```

**Follow-up Questions**:
1. Can a tuple containing a list be hashed or used as a dict key? *(No, `TypeError: unhashable type: 'list'`)*.
2. How does string interning optimize memory for immutable strings?

**Interview Tips & Pitfalls**:
Avoid saying "strings are mutable because `s = s + 'a'` changes `s`". Clarify that variable reassignment rebinds the pointer; the underlying string object was never modified.

---

### <a id="q2-garbage-collection"></a>2. CPython Garbage Collection & Reference Counting
**Difficulty**: Medium | **Level**: SDE-1/2  
**Why asked**: Evaluates understanding of object lifecycle, memory leaks, and GC tuning in long-running services.

**Detailed Answer**:
CPython employs a two-tier memory management system:
1. **Reference Counting (Primary Collector)**: Every object header contains `ob_refcnt`. When an object reference is created, `ob_refcnt` increments; when a reference goes out of scope or is deleted (`del`), it decrements. The moment `ob_refcnt == 0`, memory is deallocated immediately.
2. **Generational Cyclic Garbage Collector (Secondary Collector)**: Reference counting fails for **reference cycles** (e.g., Object A references B, and B references A). The `gc` module divides objects into three generations (Gen 0, Gen 1, Gen 2). Gen 0 runs frequently; objects surviving collection are promoted to older generations. GC uses a tri-color marking algorithm on container objects to isolate unreachable cycles.

```python
import sys
import gc

a = []
b = []
a.append(b)
b.append(a) # Cyclic reference created

del a
del b # Reference counts do not reach 0 due to cycle!

gc.collect() # Triggers generational GC to collect cyclic unreachable objects
```

**Follow-up Questions**:
- What tools would you use to diagnose a memory leak in production? (`tracemalloc`, `objgraph`).
- How does `weakref` prevent reference cycles?

---

### <a id="q3-global-interpreter-lock-gil"></a>3. Global Interpreter Lock (GIL) Mechanics & Workarounds
**Difficulty**: Hard | **Level**: SDE-2/Senior  
**Why asked**: Central to evaluating backend architecture decisions under high throughput concurrency.

**Detailed Answer**:
The **GIL** is a mutual exclusion lock in CPython that prevents multiple native OS threads from executing Python bytecode simultaneously. It simplifies CPython's memory management because C-API data structures and reference counting operations do not need fine-grained locking.

**Impact**:
- **I/O-Bound Tasks** (HTTP calls, DB queries, Disk I/O): Threading works well because CPython releases the GIL during OS system calls.
- **CPU-Bound Tasks** (Image processing, heavy calculations): Threading provides zero speedup (and may run slower due to lock contention context switches).

**Workarounds**:
1. Use `multiprocessing` (spawns separate OS processes with independent GILs).
2. Write/Use C Extensions or Cython that release the GIL explicitly via `Py_BEGIN_ALLOW_THREADS`.
3. Use alternative implementations (PyPy STM, or Python 3.13+ free-threaded build `--disable-gil`).

**Follow-up Questions**:
- Does `asyncio` bypass the GIL? *(No, `asyncio` is single-threaded cooperative multitasking; it avoids GIL lock contention by running on 1 thread).*

---

### <a id="q4-method-types"></a>4. `@staticmethod` vs `@classmethod` vs Instance Methods
**Difficulty**: Medium | **Level**: SDE-1  
**Why asked**: Fundamental OOP design patterns and Pythonic class mechanics.

**Detailed Answer**:
- **Instance Method**: Implicitly receives `self` (the instance). Can read/modify instance and class attributes.
- **Class Method (`@classmethod`)**: Implicitly receives `cls` (the class object). Used primarily for **factory methods** (alternative constructors) and class-level state mutation. Inheritance aware.
- **Static Method (`@staticmethod`)**: Receives neither `self` nor `cls`. Behaves like a plain function utility housed within the class namespace for logical grouping.

```python
class Date:
    def __init__(self, year: int, month: int, day: int):
        self.year, self.month, self.day = year, month, day

    @classmethod
    def from_string(cls, date_str: str) -> "Date":
        year, month, day = map(int, date_str.split("-"))
        return cls(year, month, day) # Factory constructor

    @staticmethod
    def is_valid_year(year: int) -> bool:
        return 1900 <= year <= 2100
```

---

### <a id="q5-execution-timer-decorator"></a>5. Execution Time Measurement Decorator
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Tests closure mastery, higher-order functions, and preservation of callable metadata (`@wraps`).

**Detailed Answer**:
```python
import time
from functools import wraps
from typing import Callable, Any

def timer(func: Callable) -> Callable:
    @wraps(func) # Preserves __name__, __doc__, and type hints of func
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        start_time = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start_time
        print(f"[TIMER] {func.__name__} executed in {elapsed:.6f} seconds")
        return result
    return wrapper

@timer
def compute_sum(n: int) -> int:
    return sum(range(n))
```

**Interview Tips**: Always include `@wraps(func)`. Forgetting `@wraps` wipes out `func.__name__` and `func.__doc__`, replacing them with `'wrapper'`, which breaks documentation tools and introspection.

---

### <a id="q6-args-and-kwargs"></a>6. `*args` and `**kwargs` Unpacking Protocol
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Basic argument passing and parameter forwarding mechanics.

**Detailed Answer**:
- `*args`: Collects positional arguments into a `tuple`.
- `**kwargs`: Collects keyword arguments into a `dict`.
- Can also be used in function calls to unpack iterables (`*iterable`) and mappings (`**mapping`).

```python
def log_and_forward(func, *args, **kwargs):
    print(f"Calling {func.__name__} with positional={args}, keyword={kwargs}")
    return func(*args, **kwargs)
```

---

### <a id="q7-generators-and-yield"></a>7. Generators, `yield`, and Lazy Evaluation
**Difficulty**: Medium | **Level**: SDE-1/2  
**Why asked**: Memory efficiency and lazy stream processing.

**Detailed Answer**:
A generator function contains the `yield` statement. When called, it does not execute immediately; it returns a generator iterator object. Calling `next()` executes code until it hits `yield`, yields the value, and suspends execution state (local variables and instruction pointer preserved).

```python
def fibonacci_generator():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci_generator()
print([next(fib) for _ in range(5)]) # Output: [0, 1, 1, 2, 3]
```

**Follow-up**: How do `send()`, `throw()`, and `close()` allow bidirectional coroutine communication?

---

### <a id="q8-list-comprehension-vs-generator-expression"></a>8. List Comprehension vs Generator Expression
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Memory footprint comparison and iteration choices.

**Detailed Answer**:
- **List Comprehension (`[...]`)**: Eagerly constructs the entire list in memory. Offers indexing and multiple iterations, but requires $O(n)$ space.
- **Generator Expression (`(...)`)**: Lazily produces values one by one on demand. $O(1)$ memory consumption. Single pass only.

```python
import sys

list_comp = [x * 2 for x in range(1_000_000)]
gen_exp = (x * 2 for x in range(1_000_000))

print(sys.getsizeof(list_comp)) # ~8,448,728 bytes
print(sys.getsizeof(gen_exp))   # ~208 bytes
```

---

### <a id="q9-context-managers"></a>9. Context Managers & Custom Protocol Implementation
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Resource safety (DB connections, files, locks) and deterministic cleanup.

**Detailed Answer**:
Implemented either via class with `__enter__`/`__exit__` or via `@contextlib.contextmanager`.

```python
# Method A: Class Protocol
class ManagedFile:
    def __init__(self, filename: str, mode: str):
        self.filename = filename
        self.mode = mode

    def __enter__():
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()
        return False # Returning True suppresses exceptions
```

---

### <a id="q10-shallow-vs-deep-copy"></a>10. Shallow Copy vs Deep Copy Mechanics
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Nested object mutation traps.

**Detailed Answer**:
- **Shallow Copy (`copy.copy()`)**: Creates a new outer container, but populates it with references to the nested objects from the original.
- **Deep Copy (`copy.deepcopy()`)**: Recursively copies all outer and inner nested objects.

```python
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0][0] = 99
print(shallow[0][0]) # 99 (Refers to same inner list)
print(deep[0][0])    # 1  (Fully isolated object hierarchy)
```

---

### <a id="q11-pass-by-assignment"></a>11. Pass-by-Assignment Parameter Passing
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Mental model for parameter scope mutation.

**Detailed Answer**:
Python uses **pass-by-assignment**. Function arguments are assigned to local parameter names as references.
- If parameter references a **mutable** object (e.g. list), mutating it in-place alters caller state (`lst.append(x)`).
- Reassigning parameter reference (`lst = [1, 2]`) rebinds local parameter variable without affecting caller reference.

---

### <a id="q12-monkey-patching"></a>12. Monkey Patching: Usage, Risks, and Mocking
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Dynamic object modification at runtime, unit testing with mocks.

**Detailed Answer**:
Monkey patching is the dynamic modification of a class or module attribute at runtime.
```python
import module_a

def patched_func():
    return "Patched Response"

# Runtime replacement
module_a.real_func = patched_func
```
**Risks**: Violates predictability, creates hard-to-debug side-effects across test suites. Safe usage: Unit test mocking via `unittest.mock.patch`.

---

### <a id="q13-is-vs-equals"></a>13. Object Identity (`is`) vs Equality (`==`)
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Avoiding subtle bug traps in logic conditions.

**Detailed Answer**:
- `==`: Evaluates value equality by invoking `a.__eq__(b)`.
- `is`: Evaluates identity equality (`id(a) == id(b)`), checking if both variables point to identical RAM memory locations.

---

### <a id="q14-exception-handling"></a>14. Exception Handling Hierarchy & Custom Exceptions
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Production error handling and domain exception design.

**Detailed Answer**:
```python
class DatabaseConnectionError(Exception):
    """Base exception for DB connectivity failures."""
    pass

try:
    connect_db()
except DatabaseConnectionError as err:
    logger.error(f"Failed connection: {err}")
else:
    logger.info("Connection established successfully")
finally:
    cleanup_sockets()
```

---

### <a id="q15-lru-cache-decorator"></a>15. Thread-Safe LRU Cache & Caching Decorators
**Difficulty**: Hard | **Level**: SDE-2/Senior  
**Why asked**: Algorithmic data structure design (Doubly Linked List + Hash Map) and caching.

**Detailed Answer**:
Can use standard library `@functools.lru_cache(maxsize=128)` or build custom LRU using `collections.OrderedDict`.

```python
from collections import OrderedDict
import threading

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity
        self.lock = threading.Lock()

    def get(self, key: int) -> int:
        with self.lock:
            if key not in self.cache:
                return -1
            self.cache.move_to_end(key)
            return self.cache[key]

    def put(self, key: int, value: int) -> None:
        with self.lock:
            if key in self.cache:
                self.cache.move_to_end(key)
            self.cache[key] = value
            if len(self.cache) > self.capacity:
                self.cache.popitem(last=False) # Removes LRU item (first item)
```

---

### <a id="q16-asyncio-gather-vs-wait"></a>16. `asyncio.gather()` vs `asyncio.wait()`
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Asynchronous concurrency control in production APIs.

**Detailed Answer**:
- `asyncio.gather(*aws, return_exceptions=False)`:
  - Higher-level wrapper taking awaitables.
  - Returns ordered list of results corresponding to input sequence.
  - If `return_exceptions=True`, exceptions are returned as list elements rather than immediately raised.
- `asyncio.wait(fs, return_when=ALL_COMPLETED)`:
  - Lower-level function taking a set of `Task` objects.
  - Returns tuple of two sets: `(done_tasks, pending_tasks)`.
  - Supports `return_when` options: `FIRST_COMPLETED`, `FIRST_EXCEPTION`, `ALL_COMPLETED`.

---

### <a id="q17-metaclasses"></a>17. Metaclasses & Class Creation Protocol
**Difficulty**: Very Hard | **Level**: Senior/Staff  
**Why asked**: Advanced metaprogramming, ORM architecture, framework design.

**Detailed Answer**:
A metaclass is a class whose instances are classes. `type` is the default built-in metaclass.

```python
class PluginRegistryMeta(type):
    registry = {}
    def __new__(cls, name, bases, attrs):
        new_class = super().__new__(cls, name, bases, attrs)
        if name != "BasePlugin":
            cls.registry[name] = new_class
        return new_class

class BasePlugin(metaclass=PluginRegistryMeta):
    pass

class AudioPlugin(BasePlugin):
    pass

print(PluginRegistryMeta.registry) # Output: {'AudioPlugin': <class '__main__.AudioPlugin'>}
```

---

### <a id="q18-slots-optimization"></a>18. `__slots__` Memory Optimization
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Profiling & memory optimization for high-count objects.

**Detailed Answer**:
By default, Python instances use a dynamic dictionary `__dict__` to store attributes, adding ~150-200 bytes overhead per instance. Defining `__slots__ = ('x', 'y')` replaces `__dict__` with a fixed-size C array of pointers, optimizing memory and accelerating attribute lookup speeds.

---

### <a id="q19-import-system"></a>19. Python Import System & `sys.path`
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Resolving module import issues, package architecture.

**Detailed Answer**:
1. Check `sys.modules` cache (returns existing module if already loaded).
2. Search standard library and directories listed in `sys.path` (Current Dir $\rightarrow$ `PYTHONPATH` $\rightarrow$ Site-Packages).
3. Compile `.py` source to bytecode `.pyc` in `__pycache__` and insert into `sys.modules`.

---

### <a id="q20-thread-safe-singleton"></a>20. Thread-Safe Singleton Pattern
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Concurrency safety in shared configuration objects.

**Detailed Answer**:
```python
import threading

class Singleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock: # Double-checked locking pattern
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
        return cls._instance
```

---

### <a id="q21-threading-vs-multiprocessing"></a>21. `threading.Thread` vs `multiprocessing.Process`
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Selecting concurrency architecture for backend tasks.

**Detailed Answer**:
- `threading.Thread`: Shared memory, low overhead, restricted by GIL (only I/O bound gains).
- `multiprocessing.Process`: Isolated memory, high process spawn overhead, bypasses GIL (true CPU parallelism across cores).

---

### <a id="q22-type-hinting"></a>22. Type Hinting & Static Type Checking (`mypy`)
**Difficulty**: Easy | **Level**: SDE-1  
**Why asked**: Modern Python code quality and maintainability standards.

**Detailed Answer**:
```python
from typing import List, Dict, Optional

def calculate_metrics(user_ids: List[int]) -> Dict[str, float]:
    return {"mean": sum(user_ids) / len(user_ids)}
```
Type hints are ignored at runtime by Python interpreter but validated statically by tools like `mypy` or `pyright`.

---

### <a id="q23-profiling-and-optimization"></a>23. Profiling and Performance Optimization
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Diagnosing performance bottlenecks in production services.

**Detailed Answer**:
- Profiling tools: `cProfile` (CPU execution call count & time), `line_profiler` (line-by-line inspection), `tracemalloc` (RAM allocation snapshots).
- Optimization strategies: Use C-builtins, NumPy vectorization, `__slots__`, PyPy, or Cython.

---

### <a id="q24-with-statement-protocol"></a>24. `with` Statement (`__enter__`/`__exit__`) Protocol
**Difficulty**: Medium | **Level**: SDE-1  
**Why asked**: Resource management protocol guarantees.

**Detailed Answer**:
`__enter__()` executes before entering the code block, returning the managed resource. `__exit__(exc_type, exc_val, exc_tb)` executes upon exiting the block, even if an unhandled exception occurs.

---

### <a id="q25-descriptors"></a>25. Descriptor Protocol (`__get__`, `__set__`)
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Deep language mechanics powering `@property`, `classmethod`, ORM fields.

**Detailed Answer**:
```python
class TypedString:
    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, instance, owner):
        if instance is None:
            return self
        return instance.__dict__.get(self.name, "")

    def __set__(self, instance, value):
        if not isinstance(value, str):
            raise TypeError(f"{self.name} must be a string")
        instance.__dict__[self.name] = value
```

---

### <a id="q26-mro-and-diamond-problem"></a>26. Multiple Inheritance, Diamond Problem & MRO
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Understanding OOP method resolution under complex inheritance graph topologies.

**Detailed Answer**:
Python resolves attribute lookups in multiple inheritance using the **C3 Linearization** algorithm. Prevents duplicate base class evaluation in diamond graphs (`D(B, C)` inheriting from `A`). View order with `D.__mro__`.

---

### <a id="q27-weak-references"></a>27. Weak References (`weakref`) & Cache Leaks
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Memory management without preventing garbage collection.

**Detailed Answer**:
`weakref.ref(obj)` creates a reference that does not increment `ob_refcnt`. If all strong references to `obj` are deleted, GC reclaims `obj` even if weak references exist. Ideal for caches and graphs.

---

### <a id="q28-closure-late-binding"></a>28. Late Binding in Closures & Lambdas
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Common functional bug trap.

**Detailed Answer**:
Variables in inner closure scopes are looked up at **call time**, not definition time.
- *Fix*: Bind default parameter value at definition: `lambda i=i: i`.

---

### <a id="q29-parameterized-decorators"></a>29. Decorators with Arguments (3-Level Nesting)
**Difficulty**: Hard | **Level**: SDE-2  
**Why asked**: Advanced closure and decorator construction.

**Detailed Answer**:
```python
from functools import wraps

def repeat(num_times: int):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator
```

---

### <a id="q30-coroutine-methods"></a>30. Coroutines: `send()`, `throw()`, `close()`
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Bidirectional generator state machine understanding.

**Detailed Answer**:
- `gen.send(val)`: Resumes generator, passing `val` as return value of current `yield` expression.
- `gen.throw(Exc)`: Raises exception inside generator at suspended `yield`.
- `gen.close()`: Raises `GeneratorExit` inside generator to trigger cleanup.

---

### <a id="q31-getattr-vs-getattribute"></a>31. `__getattr__` vs `__getattribute__`
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Attribute access interception and proxy patterns.

**Detailed Answer**:
- `__getattribute__(self, name)`: Intercepts **EVERY** attribute access unconditionally. Danger: Must call `super().__getattribute__(name)` to avoid infinite recursion!
- `__getattr__(self, name)`: Invoked **ONLY** if attribute is NOT found in instance `__dict__` or class hierarchy.

---

### <a id="q32-init-vs-new"></a>32. `__init__` vs `__new__`
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Object allocation lifecycle.

**Detailed Answer**:
- `__new__(cls)`: Allocates RAM for the new object and returns instance. First parameter is class `cls`.
- `__init__(self)`: Initializes the created instance. First parameter is instance `self`.

---

### <a id="q33-functools-wraps"></a>33. `functools.wraps` Metadata Preservation
**Difficulty**: Easy | **Level**: SDE-1  
**Why asked**: Clean decorator design best practices.

**Detailed Answer**:
Copies `__name__`, `__doc__`, `__annotations__`, and sets `__wrapped__` from target function onto decorator wrapper function.

---

### <a id="q34-zero-copy-memoryview"></a>34. Zero-Copy Operations with `memoryview`
**Difficulty**: Hard | **Level**: Staff  
**Why asked**: High-performance binary stream processing (networking, media).

**Detailed Answer**:
`memoryview` exposes C-level buffer protocol. Slicing a `memoryview` creates a slice view pointer without allocating new byte copies in memory.

---

### <a id="q35-contextvars-in-async"></a>35. `contextvars` in Async Execution
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Async context isolation (request tracing, tenant IDs) across coroutine execution contexts.

**Detailed Answer**:
`threading.local` fails in `asyncio` because all coroutines execute on a single OS thread. `contextvars.ContextVar` maintains context propagation across coroutine boundaries safely.

---

### <a id="q36-pattern-matching"></a>36. Structural Pattern Matching (`match`/`case`)
**Difficulty**: Medium | **Level**: SDE-1  
**Why asked**: Modern Python 3.10+ syntax features.

**Detailed Answer**:
```python
match command.split():
    case ["quit"]:
        exit()
    case ["move", ("left" | "right") as direction]:
        move(direction)
    case _:
        print("Unknown command")
```

---

### <a id="q37-init-subclass-hook"></a>37. `__init_subclass__` Class Creation Hook
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Lightweight subclass hook alternatives to metaclasses.

**Detailed Answer**:
```python
class PluginBase:
    subclasses = []
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        cls.subclasses.append(cls)
```

---

### <a id="q38-abstract-base-classes"></a>38. Abstract Base Classes (`abc.ABC`, `@abstractmethod`)
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Interface definition & contract enforcement.

**Detailed Answer**:
Classes inheriting `abc.ABC` containing `@abstractmethod` decorators cannot be instantiated unless all abstract methods are overridden in concrete child subclasses.

---

### <a id="q39-async-context-managers"></a>39. Async Context Managers (`__aenter__`/`__aexit__`)
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Async resource lifecycle management (HTTP pools, DB connections).

**Detailed Answer**:
Uses `async with` syntax. Must implement coroutine methods `async def __aenter__(self)` and `async def __aexit__(self, exc_type, exc_val, exc_tb)`.

---

### <a id="q40-singledispatch"></a>40. `functools.singledispatch` Generic Functions
**Difficulty**: Medium | **Level**: SDE-2  
**Why asked**: Function overloading based on first argument type.

**Detailed Answer**:
```python
from functools import singledispatch

@singledispatch
def process(data):
    raise NotImplementedError

@process.register(int)
def _(data: int):
    return data * 2

@process.register(str)
def _(data: str):
    return data.upper()
```

---

### <a id="q41-tracemalloc"></a>41. Memory Leak Detection with `tracemalloc`
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Debugging RAM growth in production services.

**Detailed Answer**:
Standard library tool tracking memory allocations made by Python. Takes snapshots (`snapshot1`, `snapshot2`) and computes `snapshot2.compare_to(snapshot1, 'lineno')`.

---

### <a id="q42-subprocess-popen"></a>42. `subprocess.Popen` Process Streaming & Deadlocks
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: OS process execution safety.

**Detailed Answer**:
Directly reading `stdout` without reading `stderr` concurrently can cause OS pipe buffers to fill up, deadlocking both parent and child processes. Use `process.communicate()` or `asyncio.create_subprocess_exec()`.

---

### <a id="q43-typing-protocol"></a>43. Structural Subtyping via `typing.Protocol`
**Difficulty**: Hard | **Level**: Senior  
**Why asked**: Static duck typing in large Python codebases.

**Detailed Answer**:
Classes don't need explicit inheritance. Any class defining methods matching a `Protocol` interface passes static type validation (`mypy`).

---

### <a id="q44-dataclass-default-factory"></a>44. `dataclass` Fields & `default_factory`
**Difficulty**: Medium | **Level**: SDE-1  
**Why asked**: Safe defaults in dataclasses without mutable traps.

**Detailed Answer**:
```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class User:
    name: str
    tags: List[str] = field(default_factory=list) # Safe mutable default
```

---

### <a id="q45-global-vs-nonlocal"></a>45. Global vs Nonlocal Scope Mutability
**Difficulty**: Easy | **Level**: Fresher  
**Why asked**: Scope mutation mechanics.

**Detailed Answer**:
- `global var`: Binds local scope name to module-level global variable.
- `nonlocal var`: Binds local scope name to nearest enclosing function scope (excluding module global).
