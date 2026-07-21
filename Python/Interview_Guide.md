# 📘 Python Interview Guide (Beginner to Staff+)

This comprehensive guide breaks down Python interview expectations into three distinct career tiers: **Beginner (Junior/Fresher)**, **Intermediate (Mid-Level SDE-1/SDE-2)**, and **Advanced (Senior/Staff Engineeer)**.

---

## 🟢 1. Beginner Section (Junior / Fresher Tier: 0–2 YOE)

### Core Topics & Concepts
- **Built-in Data Types & Mutability**:
  - *Immutable*: `int`, `float`, `str`, `tuple`, `frozenset`, `bytes`.
  - *Mutable*: `list`, `dict`, `set`, `bytearray`.
  - *Identity vs Equality*: `is` checks reference equality (`id(a) == id(b)`), `==` checks value equality (`a.__eq__(b)`). Small integer caching (-5 to 256) and string interning can cause identity matching for distinct allocations.
- **Control Flow & Functions**:
  - Conditionals, `for`/`while` loops with `else` block (executes if loop finishes without `break`).
  - Function definitions, default parameters, variable arguments (`*args` tuple, `**kwargs` dict).
  - Scope resolution via **LEGB Rule**: **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in.
- **Basic Data Structures & Operations**:
  - Lists (dynamic arrays): $O(1)$ append, $O(n)$ insertion/deletion.
  - Dictionaries (hash tables): $O(1)$ average lookup/insert, $O(n)$ worst-case rehash.
  - Sets (hash sets): Unordered, hash-based unique elements.
  - Tuples: Fixed-size immutable sequences.
- **Basic OOP**:
  - Classes, instances, `self` parameter, standard attributes, basic inheritance.

### Common Beginner Mistakes & Traps
1. **Mutable Default Arguments**:
   ```python
   # WRONG: Cart list persists across function invocations!
   def add_item(item, cart=[]):
       cart.append(item)
       return cart

   # CORRECT: Use None as default sentinel
   def add_item(item, cart=None):
       if cart is None:
           cart = []
       cart.append(item)
       return cart
   ```
2. **Confusing Assignment with Copying**:
   ```python
   a = [1, 2, 3]
   b = a        # Modifying b modifies a!
   c = a.copy() # Shallow copy creates independent outer container
   ```
3. **Modifying a Collection While Iterating**:
   ```python
   # WRONG: Skips elements due to index shifting
   nums = [1, 2, 3, 4]
   for n in nums:
       if n % 2 == 0:
           nums.remove(n)

   # CORRECT: Iterate over copy or use comprehension
   nums = [n for n in nums if n % 2 != 0]
   ```
4. **Single Element Tuple Syntax**: `x = (1)` is an integer. `x = (1,)` is a 1-tuple.

### Expected Beginner Interview Questions
- *Question*: What is the difference between `list.sort()` and `sorted(list)`?
  - *Answer*: `list.sort()` sorts the list in-place and returns `None`. `sorted(list)` creates a new sorted list and leaves the original untouched.
- *Question*: How does argument passing work in Python?
  - *Answer*: Python uses **pass-by-assignment** (or pass-by-object-reference). Function parameters receive references to the caller's objects. Mutating a mutable object in-place updates the original; reassigning the parameter variable inside the function rebinds the local variable only.
- *Question*: What happens when you do `a = [1, 2]; b = a; b += [3]` vs `b = b + [3]`?
  - *Answer*: `b += [3]` invokes `__iadd__` on lists, which mutates `b` in-place, so `a` becomes `[1, 2, 3]`. `b = b + [3]` evaluates `b + [3]` into a new list object and rebinds `b`, leaving `a` as `[1, 2]`.

---

## 🟡 2. Intermediate Section (Mid-Level Tier: 2–5 YOE)

### Frequently Tested Concepts
- **Pythonic Idioms & Functional Patterns**:
  - List, Dict, and Set Comprehensions (avoiding deeply nested loops).
  - Iterators (`__iter__`, `__next__`) and Generators (`yield`, `yield from`). Generators yield values lazily, holding frame state in memory with minimal footprint $O(1)$ space.
  - Functional helpers: `map()`, `filter()`, `zip()`, `enumerate()`, `functools.reduce()`.
- **Advanced OOP & Dunder Methods**:
  - Methods: Instance (`self`), `@classmethod` (`cls` - factory methods), `@staticmethod` (isolated logic).
  - Special Methods: `__str__` (user display) vs `__repr__` (unambiguous developer representation), `__eq__` and `__hash__` (must be implemented together for custom dict keys).
  - Multiple Inheritance & MRO: Python resolves attribute lookup using **C3 Linearization** algorithm accessible via `Class.mro()` or `Class.__mro__`.
  - `super()`: Cooperative multiple inheritance dispatch.
- **Decorators & Closures**:
  - Closures: Outer function returning inner function retaining non-local scope variables.
  - Decorators: Higher-order functions wrapping target callables. Preserving metadata using `@functools.wraps`.
  - Parameterized decorators (requiring 3 nested function levels).
- **Resource Management & Context Managers**:
  - The `with` statement protocol: `__enter__()` returns target resource, `__exit__(exc_type, exc_val, exc_tb)` handles cleanup and optional exception suppression (by returning `True`).
  - Using `@contextlib.contextmanager` generator pattern.
- **Concurrency Essentials**:
  - CPython GIL (Global Interpreter Lock): Mutex preventing multiple native threads from executing Python bytecode simultaneously.
  - `threading` module: Ideal for I/O-bound tasks (file, network, DB calls) where GIL is released during OS calls.
  - `multiprocessing` module: Spawns independent OS processes to bypass GIL for CPU-bound computation.

### Coding Expectations
- Candidates are expected to write production-ready, clean, PEP-8 compliant code.
- Proper use of type annotations (`typing.List`, `Dict`, `Optional`, `Callable`, `Union`).
- Efficient algorithm design avoiding unnecessary intermediate memory allocations.
- Custom Exception handling hierarchies extending base `Exception`.

### Real Interview Expectations & Scenario Walkthroughs
- *Scenario*: Implement a decorator `@retry(max_retries=3, delay=1.0)` that retries a failing function on specified exceptions.
  - *Evaluation*: Interviewers check if you handle `@wraps`, use parameterized decorator structure, sleep between retries, and re-raise the final exception if retries are exhausted.

### Intermediate Follow-up Questions & Deep Dives
- *Follow-up*: "Why must a dictionary key be hashable? Can a tuple containing a list be used as a dict key?"
  - *Answer*: Dict keys use hash values for $O(1)$ bucket indexing. An object is hashable if its hash value never changes during its lifecycle (`__hash__`) and can be compared for equality (`__eq__`). A tuple containing a mutable list raises `TypeError: unhashable type: 'list'` because mutating the list alters the composite state.

---

## 🔴 3. Advanced Section (Senior / Staff Tier: 5+ YOE)

### Production Concepts & Internals
- **CPython Object Lifecycle & Memory Architecture**:
  - `PyObject` structure: Every Python object header contains `ob_refcnt` (reference count) and `ob_type` (pointer to type object).
  - **Memory Management**: Small object allocator (PyMalloc) manages blocks, pools, and arenas for objects $\le 512$ bytes to prevent fragmentation.
  - **Garbage Collection**:
    1. *Reference Counting*: Primary collector. Deallocates objects immediately when `ob_refcnt == 0`.
    2. *Generational Cyclic GC*: Secondary collector for cyclic references (Generation 0, 1, 2). Uses tri-color marking algorithm to detect unreachable reference cycles.
  - `weakref` module: Weak references allow accessing target objects without incrementing `ob_refcnt`, preventing reference cycles in caches or graphs.
- **Asyncio Internals & Custom Event Loops**:
  - Event Loop mechanics: Task queue, I/O multiplexing (`select`/`epoll`/`kqueue`).
  - Coroutine state machine: `async def` functions compile to code objects with `CO_COROUTINE` flag. Suspended via `yield` / `await` expressions yielding control back to loop.
  - `asyncio.gather()` (concurrency with single failure behavior control) vs `asyncio.wait()` (fine-grained control: `FIRST_COMPLETED`, `ALL_COMPLETED`).
  - Task cancellation mechanics, `asyncio.TaskGroup` (Python 3.11+ structured concurrency).
- **Metaprogramming & Metaclasses**:
  - Metaclasses are classes of classes (derived from `type`).
  - Class instantiation pipeline: `type.__call__` triggers `Meta.__new__` (allocates class object) $\rightarrow$ `Meta.__init__` (initializes class object) $\rightarrow$ Returns class.
  - Modern alternatives: `__init_subclass__` hook (PEP 487) simplifies class creation hooks without full metaclass complexity.
- **Descriptors & Attribute Resolution**:
  - Descriptor Protocol: `__get__(self, instance, owner)`, `__set__(self, instance, value)`, `__delete__(self, instance)`.
  - Data descriptors (define `__set__` or `__delete__`) override instance dictionary lookups. Non-data descriptors (define only `__get__`, e.g., methods) are overridden by instance dictionary entries.
  - Attribute lookup resolution order:
    1. Data descriptor on class & base classes.
    2. Instance `__dict__`.
    3. Non-data descriptor on class & base classes.
    4. Class `__dict__`.
    5. `__getattr__()` if defined.
- **Optimization & Low-Level Interfaces**:
  - `__slots__`: Eliminates dynamic `__dict__` per instance, storing attributes in a fixed C array. Reduces per-instance memory overhead by up to 60-70%.
  - Zero-copy buffer protocol: `memoryview` enables reading/slicing raw binary array memory without copying bytes across C/Python boundaries.
  - C-Extensions & `ctypes`/`cffi`: Releasing GIL explicitly in C extensions (`Py_BEGIN_ALLOW_THREADS` ... `Py_END_ALLOW_THREADS`) for CPU parallelism.

### Architecture Discussions & Senior Expectations
- **Senior Role Focus**: In senior interviews, candidate responses must balance theoretical correctness with production trade-offs.
  - *Example*: Choosing between `multiprocessing` processes vs `Celery` workers vs `asyncio` task queues for a high-throughput backend service.
  - *Analysis*: Discussing IPC serialization costs (pickle overhead in multiprocessing), memory footprint, connection pooling, graceful signal handling (`SIGTERM`), and fault isolation.

### Edge Cases & Critical Nuances
- **`__del__` Resurrection and Finalizers**: `__del__` is called when `ob_refcnt == 0`. If `__del__` assigns `self` to a global variable, the object is resurrected!
- **Exception Chaining**: `raise NewException() from original_exception` sets `__cause__` explicitly. `raise NewException()` inside `except` block automatically sets `__context__`.
- **Contextvars in Async Applications**: Thread-local storage (`threading.local`) fails in async coroutines running on a single thread. Senior engineers use `contextvars.ContextVar` to isolate request context across async execution boundaries.
