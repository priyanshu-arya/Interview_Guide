# ❓ Top Python Interview Questions & Detailed Answers

This document contains an exhaustive collection of the **Top 25 Most Repeated Questions**, **Top 50 Most Difficult Questions**, **Top 50 Must-Know Questions**, and **Top 50 Questions That Differentiate Top Candidates** across FAANG, Unicorns, FinTech, and AI infrastructure companies.

---

## 📋 Table of Contents

1. [Top 25 Most Repeated Questions](#top-25-most-repeated-questions)
2. [Top 50 Most Difficult Questions](#top-50-most-difficult-questions)
3. [Top 50 Must-Know Questions](#top-50-must-know-questions)
4. [Top 50 Questions That Differentiate Top Candidates](#top-50-differentiating-questions)

---

## 🚀 1. Top 25 Most Repeated Questions

These questions appear across **Big Tech, Unicorn, FinTech, AI, and Startup** interview loops year after year.

---

### Q1: Mutable vs Immutable Types & Memory Implications
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Tests fundamental mental model of Python memory, side effects, and hash table stability.
- **Detailed Answer**: Immutable types (`int`, `float`, `str`, `tuple`, `frozenset`, `bytes`) cannot be modified after creation. Mutable types (`list`, `dict`, `set`, `bytearray`) can be updated in-place. Immutability guarantees hash stability (`__hash__`), which is required for dictionary keys and set elements.
- **Practical Example**:
  ```python
  # Immutable string reassignment rebinds reference
  s = "hello"
  s_ref = s
  s += " world" # Creates new object! s_ref remains "hello"

  # Mutable list modification in-place
  lst = [1, 2]
  lst_ref = lst
  lst.append(3) # Modifies underlying object! lst_ref is now [1, 2, 3]
  ```
- **Common Mistakes**: Claiming strings are mutable because `s = s + 'x'` works. Variable binding changes, but the original string object never changes.
- **Follow-up Questions**: Why can a tuple containing a list NOT be hashed or used as a dict key? *(Raises `TypeError: unhashable type: 'list'` because mutating the inner list alters the tuple's composite state).*

---

### Q2: CPython Garbage Collection Mechanics
- **Difficulty**: Medium | **Level**: SDE1/2 | **Company Categories**: Big Tech, Unicorns
- **Why Interviewers Ask It**: Tests understanding of object lifecycle, memory leaks, and GC tuning in long-running services.
- **Detailed Answer**: CPython uses **reference counting** as its primary collector. Every object header contains `ob_refcnt`. When `ob_refcnt == 0`, memory is freed immediately. To handle reference cycles (e.g. A references B and B references A), CPython includes a secondary **generational garbage collector** (`gc` module) with 3 generations (Gen 0, Gen 1, Gen 2) using a tri-color marking algorithm.
- **Practical Example**:
  ```python
  import gc, sys

  a = []
  b = []
  a.append(b)
  b.append(a) # Cyclic reference created

  del a; del b # Ref counts don't reach 0
  gc.collect() # Generational GC frees unreachable cycle
  ```
- **Common Mistakes**: Saying CPython uses pure mark-and-sweep without reference counting.
- **Follow-up Questions**: How do you detect memory leaks in production? (`tracemalloc`, `objgraph`, `weakref`).

---

### Q3: Global Interpreter Lock (GIL) Mechanics & Workarounds
- **Difficulty**: Hard | **Level**: SDE2/Senior | **Company Categories**: Big Tech, Unicorns, FinTech
- **Why Interviewers Ask It**: Central to evaluating backend concurrency decisions under high throughput.
- **Detailed Answer**: The GIL is a mutual exclusion lock in CPython that protects access to Python objects, preventing multiple native threads from executing Python bytecode simultaneously. It simplifies CPython memory management. Threading benefits I/O-bound tasks (network, disk) because CPython releases the GIL during OS system calls. CPU-bound tasks receive no parallel speedup.
- **Workarounds**: Use `multiprocessing` (spawns processes with independent GILs), write C-extensions releasing GIL (`Py_BEGIN_ALLOW_THREADS`), or use Python 3.13+ free-threaded builds (`--disable-gil`).
- **Follow-up Questions**: How does `asyncio` relate to GIL? *(Cooperative single-threaded multitasking; it avoids GIL contention by running on 1 thread).*

---

### Q4: `@staticmethod` vs `@classmethod` vs Instance Methods
- **Difficulty**: Medium | **Level**: SDE1 | **Company Categories**: All
- **Why Interviewers Ask It**: Fundamental OOP and Pythonic class design.
- **Detailed Answer**:
  - *Instance Method*: Receives `self` (instance). Reads/modifies instance and class state.
  - *Class Method (`@classmethod`)*: Receives `cls` (class). Used for factory methods / alternative constructors.
  - *Static Method (`@staticmethod`)*: Receives neither `self` nor `cls`. Plain utility function in class namespace.
- **Practical Example**:
  ```python
  class Pizza:
      def __init__(self, ingredients):
          self.ingredients = ingredients
      @classmethod
      def margherita(cls):
          return cls(['mozzarella', 'tomatoes'])
      @staticmethod
      def is_valid_ingredient(ingredient):
          return ingredient in ['cheese', 'tomatoes', 'mozzarella']
  ```

---

### Q5: Execution Time Measurement Decorator
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: Big Tech, FinTech
- **Why Interviewers Ask It**: Tests closure mastery, higher-order functions, and `@functools.wraps`.
- **Detailed Answer**:
  ```python
  import time
  from functools import wraps

  def timer(func):
      @wraps(func)
      def wrapper(*args, **kwargs):
          start = time.perf_counter()
          result = func(*args, **kwargs)
          elapsed = time.perf_counter() - start
          print(f"{func.__name__} executed in {elapsed:.6f}s")
          return result
      return wrapper
  ```
- **Interview Tip**: Always use `@wraps(func)` to preserve function name, docstring, and type annotations.

---

### Q6: `*args` and `**kwargs` Unpacking Protocol
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Universal argument forwarding signature pattern.
- **Detailed Answer**: `*args` captures extra positional arguments into a `tuple`. `**kwargs` captures extra keyword arguments into a `dict`. Unpacking operator `*` unpacks iterables; `**` unpacks mappings.

---

### Q7: Generators and `yield`
- **Difficulty**: Medium | **Level**: SDE1/2 | **Company Categories**: All
- **Why Interviewers Ask It**: Memory efficiency and lazy stream evaluation.
- **Detailed Answer**: Generators use `yield` to return values lazily, suspending execution state (variables and instruction pointer). They implement the iterator protocol and raise `StopIteration` when exhausted.
- **Fibonacci Generator**:
  ```python
  def fib():
      a, b = 0, 1
      while True:
          yield a
          a, b = b, a + b
  ```

---

### Q8: List Comprehension vs Generator Expression
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Memory footprint and performance trade-offs.
- **Detailed Answer**: List comprehension `[x*2 for x in range(10^6)]` creates the entire list in memory ($O(N)$ space). Generator expression `(x*2 for x in range(10^6))` yields items one by one ($O(1)$ space).

---

### Q9: Context Manager Protocol
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: All
- **Why Interviewers Ask It**: Deterministic resource management (files, locks, DB handles).
- **Detailed Answer**: Implemented via class (`__enter__` returns resource, `__exit__` handles cleanup and optional exception suppression) or `@contextlib.contextmanager` generator.

---

### Q10: Shallow Copy vs Deep Copy
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Mutability pitfalls in nested data structures.
- **Detailed Answer**: Shallow copy (`copy.copy()`) creates a new outer container but references the same inner objects. Deep copy (`copy.deepcopy()`) recursively copies all outer and inner objects.

---

### Q11: Pass-by-Assignment Parameter Passing
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Accurate parameter mutation mental model.
- **Detailed Answer**: Parameters are assigned references to caller objects. Modifying a mutable object in-place (`lst.append(x)`) updates caller state. Reassigning parameter reference (`lst = [1, 2]`) rebinds local parameter only.

---

### Q12: Monkey Patching
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: Unicorns, Startups
- **Why Interviewers Ask It**: Dynamic object modification at runtime, unit test mocking.
- **Detailed Answer**: Dynamically overriding attributes/methods at runtime (`module.func = new_func`). Useful for mocking in tests (`unittest.mock.patch`), risky in production code.

---

### Q13: `is` vs `==`
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Identity vs equality distinction.
- **Detailed Answer**: `==` checks value equality (`__eq__`). `is` checks reference identity (`id(a) == id(b)`).

---

### Q14: Exception Handling & Custom Exceptions
- **Difficulty**: Easy | **Level**: Fresher | **Company Categories**: All
- **Why Interviewers Ask It**: Robust application design.
- **Detailed Answer**: Custom exceptions inherit from `Exception`. `try/except/else/finally` structure ensures clean control flow and cleanup.

---

### Q15: Memoization Decorator (LRU Cache)
- **Difficulty**: Hard | **Level**: SDE2/Senior | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Closures, stateful decorators, caching strategies.
- **Detailed Answer**: Wrap target function with a dictionary cache mapping `args` to results, or use `@functools.lru_cache`.

---

### Q16: `asyncio.gather()` vs `asyncio.wait()`
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Big Tech, Unicorns
- **Why Interviewers Ask It**: Concurrency control in async workflows.
- **Detailed Answer**: `gather()` takes awaitables and returns ordered results. `wait()` takes tasks/futures and returns two sets `(done, pending)` with fine-grained triggers (`FIRST_COMPLETED`, `ALL_COMPLETED`).

---

### Q17: Metaclasses
- **Difficulty**: Very Hard | **Level**: Senior/Staff | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Deep language internals and framework mechanics (Django ORM, Pydantic).
- **Detailed Answer**: Metaclasses are classes of classes (derived from `type`). They customize class creation (`__new__`, `__init__`, `__call__`).

---

### Q18: `__slots__` Memory Optimization
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Memory optimization for high-instance classes.
- **Detailed Answer**: Defining `__slots__ = ('x', 'y')` skips per-instance `__dict__` creation, storing attributes in a fixed C array and saving ~200 bytes per object.

---

### Q19: Import System & `sys.path`
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: All
- **Why Interviewers Ask It**: Resolving module lookup issues and package structure.
- **Detailed Answer**: Python searches `sys.modules` cache, then standard library and directories in `sys.path` (`PYTHONPATH`, site-packages).

---

### Q20: Thread-Safe Singleton
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: FinTech, Unicorns
- **Why Interviewers Ask It**: Thread safety and design patterns.
- **Detailed Answer**: Implement double-checked locking using `threading.Lock()` inside `__new__`.

---

### Q21: Threading vs Multiprocessing
- **Difficulty**: Medium | **Level**: SDE2 | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Selecting concurrency strategy under the GIL.
- **Detailed Answer**: `threading` shares memory space (I/O bound). `multiprocessing` spawns separate processes with isolated memory (CPU bound).

---

### Q22: Type Hints & Static Type Checking
- **Difficulty**: Easy | **Level**: SDE1 | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Code readability and static analysis with `mypy`.
- **Detailed Answer**: Annotating parameter and return types (`def f(x: int) -> str:`). Analyzed statically, ignored at runtime.

---

### Q23: Profiling and Optimization
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Big Tech
- **Why Interviewers Ask It**: Diagnosing bottlenecks in production.
- **Detailed Answer**: Profile with `cProfile`, `line_profiler`, and `tracemalloc`. Optimize using C-builtins, vectorization, or Cython.

---

### Q24: `with` Statement Protocol
- **Difficulty**: Medium | **Level**: SDE1 | **Company Categories**: All
- **Why Interviewers Ask It**: Understanding resource management semantics.
- **Detailed Answer**: `__enter__` acquires resource; `__exit__` releases resource even if exceptions occur.

---

### Q25: Custom LRU Cache Implementation
- **Difficulty**: Hard | **Level**: Senior | **Company Categories**: Big Tech, FinTech
- **Why Interviewers Ask It**: Data structures (Doubly Linked List + Hash Map / `OrderedDict`).
- **Detailed Answer**: Use `collections.OrderedDict` or custom Doubly Linked List + Dict for $O(1)$ get/put operations.

---

## ⚡ 2. Top 50 Most Difficult Questions

Where experienced candidates stumble. Combine multiple advanced concepts.

| # | Question | Key Challenge & Core Technical Focus |
|---|----------|--------------------------------------|
| 1 | Implement a simplified `asyncio` event loop from scratch. | Generator scheduling, non-blocking I/O polling with `selectors`. |
| 2 | CPython bytecode interpreter loop & GIL release mechanics. | `ceval.c` internals, bytecode evaluation, periodic GIL release. |
| 3 | Write a type-validating Descriptor using `__set_name__`. | Descriptor protocol (`__get__`, `__set__`), class attribute binding. |
| 4 | Implement `await` manually using generators & `yield from`. | PEP 380 coroutine delegation, iterator sending mechanics. |
| 5 | Token bucket rate limiter decorator with thread safety. | Stateful closure, `threading.Lock`, sliding window calculation. |
| 6 | Cooperative Multiple Inheritance MRO (C3 Linearization). | Method Resolution Order equation, `super()` dispatch chain. |
| 7 | Thread-safe environment variable override context manager. | `contextvars`, `os.environ` thread synchronization. |
| 8 | Dynamically create a C-Extension module for CPython. | C-API (`PyObject*`, `PyModule_Create`), reference counting macros. |
| 9 | Implement a `WeakValueDictionary` with GC callbacks. | `weakref.ref` callbacks, automatic key deletion on finalization. |
| 10 | Pickle deserialization vulnerability exploit and prevention. | `__reduce__` exploit vectors, Restricted Unpickler, `json` alternative. |
| 11 | Metaclass for automatic plugin registration. | `Meta.__new__` class creation interception, global registry dictionary. |
| 12 | `asyncio.ensure_future()` vs `create_task()` vs `loop.create_task()`. | Task wrapping, loop binding, task scheduling differences. |
| 13 | Process pool worker queue using `multiprocessing`. | IPC queues, process signal handling, worker lifecycle. |
| 14 | Zero-copy binary parser using `memoryview` & `struct`. | C-buffer protocol, memory slicing without byte copying. |
| 15 | `__slots__` inheritance memory optimization hierarchy. | Slots inheritance rules, combining slots with parent `__dict__`. |
| 16 | Generator pipeline for streaming gigabyte CSV aggregates. | Lazy generator chaining, $O(1)$ RAM footprint. |
| 17 | `async for` and `async with` internals. | `__aiter__`, `__anext__`, `__aenter__`, `__aexit__` coroutines. |
| 18 | Non-blocking socket server using `selectors`. | Epoll/Kqueue multiplexing, non-blocking socket flags. |
| 19 | Custom sequence class with negative slicing views. | `__getitem__` slice object parsing (`slice.start, stop, step`). |
| 20 | Debugging CPython segmentation faults with `gdb` & `faulthandler`. | Signal handling, inspecting C-stack traces, `py-bt`. |
| 21 | Thread-safe LRU cache with TTL expiration. | `OrderedDict`, `threading.Lock`, timestamp validation on access. |
| 22 | Descriptor vs `property` mechanics under the hood. | `property` class implementation of Descriptor protocol. |
| 23 | `__build_class__` internal hook. | Class creation bytecode execution, metaclass selection order. |
| 24 | Dict subclass logging all reads/writes via dunders. | Overriding `__getitem__`, `__setitem__`, `__delitem__`, `get`. |
| 25 | Binary protocol parsing with `struct` alignment/endianness. | Struct format strings (`>I`, `<d`), padding bytes handling. |
| 26 | Forking child process with pipes & signal handlers in `os`. | `os.fork()`, `os.pipe()`, handling `SIGCHLD` to avoid zombie processes. |
| 27 | `Future` and `Task` lifecycle state machine in asyncio. | State transitions (PENDING, CANCELLED, FINISHED), callback registration. |
| 28 | Retrying decorator with exponential backoff & randomized jitter. | Floating math jitter calculation, exception handling, decorators. |
| 29 | Monkey patching a module-level import cleanly in tests. | `unittest.mock.patch`, target lookup paths, teardown restoration. |
| 30 | `functools.singledispatch` for class methods. | Dispatching on first positional argument, method descriptor wrapper. |
| 31 | `__init__` vs `__new__` for immutable object creation. | Subclassing immutable types (`int`, `str`, `tuple`) inside `__new__`. |
| 32 | Hierarchical nested execution timer context manager. | Stack of timers, context manager enter/exit tree building. |
| 33 | Custom import hook using `importlib.abc`. | `MetaPathFinder`, `ModuleLoader`, bytecode decryption/compilation. |
| 34 | `type.__subclasses__()` and `object.__subclasshook__`. | Abstract Base Class structural subtyping hook mechanics. |
| 35 | Building `namedtuple` using `type()` and descriptors. | Dynamic class generation using `type(name, bases, dict)`. |
| 36 | `copyreg.pickle` vs `__reduce__` for pickle custom serialization. | Custom pickling state restoration tuple return signatures. |
| 37 | Parallelizing CPU tasks with `concurrent.futures` & shared state. | `ProcessPoolExecutor`, `multiprocessing.Manager` shared memory. |
| 38 | Inter-process shared memory using `mmap`. | Memory mapped file handles, zero-copy process communication. |
| 39 | Debugging with `sys.settrace()` and `breakpoint()`. | Frame inspection (`frame.f_locals`), trace function events (`call`, `line`, `return`). |
| 40 | Building a micro ORM with metaclasses and descriptors. | Mapping class attributes to SQL column schema descriptors. |
| 41 | How `dataclasses.field(default_factory=...)` prevents mutable bugs. | Field metadata, evaluating default factory at instance creation. |
| 42 | `__init_subclass__` hook vs Metaclasses. | Subclass customization without metaclass inheritance conflict. |
| 43 | RegEx tokenizer handling Unicode identifiers. | `re` module, `\w` flags, Unicode category character classes. |
| 44 | Memory leak profiling using `tracemalloc` snapshots. | `snapshot.compare_to()`, tracking memory line allocations. |
| 45 | Async `subprocess.Popen` wrapper with timeout enforcement. | `asyncio.create_subprocess_exec`, `asyncio.wait_for` timeout. |
| 46 | `compile()` vs `exec()` vs `eval()`. | Compiling string code into code objects (`AST` $\rightarrow$ bytecode). |
| 47 | Detecting circular object references with `gc.get_referents()`. | Traversing container object reference graphs recursively. |
| 48 | Flag Enums with `enum.IntFlag` and bitwise operations. | Bitwise AND/OR/XOR mask operations on enum flags. |
| 49 | Implementing custom `functools.partial` preserving `__qualname__`. | Parameter binding, descriptor wrapping, metadata propagation. |
| 50 | AST refactoring from `typing` to Python 3.12 `TypeAlias`. | `ast` module parsing, transformer nodes, code regeneration. |

---

## 📚 3. Top 50 Must-Know Questions

Core concepts required to pass any Python technical interview.

1. **Mutable vs Immutable**: Definitions, built-in types, hashability implications.
2. **List vs Tuple vs Set vs Dict**: Time/Space complexities, internal mechanics.
3. **Comprehensions**: List, set, dict comprehensions, readability vs performance.
4. **Functional Tools**: `lambda`, `map()`, `filter()`, `functools.reduce()`.
5. **Arguments**: `*args`, `**kwargs`, positional-only `/`, keyword-only `*`.
6. **Decorators**: Function wrappers, `@functools.wraps`, stateful decorators.
7. **Generators**: `yield`, `yield from`, lazy evaluation, iterator protocol.
8. **Context Managers**: `with` statement, `__enter__`, `__exit__`, `@contextlib.contextmanager`.
9. **OOP Core**: Classes, instances, inheritance, `super()`, MRO.
10. **Method Types**: `@staticmethod`, `@classmethod`, instance methods.
11. **Dunder Methods**: `__str__`, `__repr__`, `__eq__`, `__hash__`, `__len__`, `__getitem__`.
12. **Exception Handling**: `try/except/else/finally`, custom exceptions, exception chaining.
13. **File I/O**: `open()`, modes (`r`, `w`, `a`, `b`), buffered reading, context managers.
14. **Modules & Packages**: `__init__.py`, `sys.path`, relative vs absolute imports.
15. **Virtual Environments**: `venv`, `pip`, `pyproject.toml`, dependency lock files.
16. **Threading vs Multiprocessing**: GIL limitations, I/O bound vs CPU bound concurrency.
17. **Asyncio Basics**: `async`/`await`, event loop, coroutines, `create_task()`.
18. **Type Annotations**: Syntax, `mypy`, `typing.Optional`, `Union`, `Callable`.
19. **Dataclasses**: `@dataclass`, `field(default_factory=...)`, immutability with `frozen=True`.
20. **`functools` Utilities**: `@lru_cache`, `partial`, `@cached_property`.
21. **`collections` Module**: `defaultdict`, `Counter`, `deque`, `OrderedDict`.
22. **`itertools` Module**: `chain`, `islice`, `groupby`, `product`, `permutations`.
23. **`pathlib` Module**: OOP file path handling (`Path.exists()`, `Path.read_text()`).
24. **JSON & Serialization**: `json.dumps()`, `json.loads()`, handling custom objects via encoders.
25. **Regular Expressions**: `re.search()`, `re.match()`, `re.findall()`, capture groups.
26. **Debugging Tools**: `pdb`, `breakpoint()`, `logging` levels and handlers.
27. **Testing Frameworks**: `pytest`, fixtures, `@pytest.mark.parametrize`, mocking.
28. **Memory Management**: Reference counting, generational `gc`, `weakref`.
29. **`__slots__`**: Eliminating instance `__dict__` memory overhead.
30. **Copying Objects**: Shallow copy (`copy.copy()`) vs deep copy (`copy.deepcopy()`).
31. **Identity vs Equality**: `is` vs `==`, integer caching (-5 to 256).
32. **Mutable Default Arguments**: Trap (`def f(lst=[])`), solution with `None` sentinel.
33. **Closure Scope**: Late binding trap in closures, default argument fix (`i=i`).
34. **Scope Identifiers**: `global` vs `nonlocal` keywords, LEGB rule.
35. **Iteration Helpers**: `enumerate()`, `zip(strict=True)`.
36. **Sorting**: `sorted()` vs `list.sort()`, `key` functions, `operator.itemgetter()`.
37. **Boolean Evaluation**: `any()`, `all()`, truthiness of empty collections.
38. **Dynamic Code Execution**: `eval()`, `exec()`, security vulnerabilities.
39. **Properties**: `@property`, getter, setter, deleter methods.
40. **Class Variables vs Instance Variables**: Attribute lookup resolution order.
41. **Main Execution Guard**: `if __name__ == "__main__":` import isolation.
42. **Generics**: `typing.List[T]`, `TypeVar`, structural subtyping with `Protocol`.
43. **Pattern Matching**: Python 3.10+ `match`/`case` structural pattern matching.
44. **Walrus Operator**: Python 3.8+ assignment expressions `:=`.
45. **Profiling**: `cProfile`, `timeit` micro-benchmarks.
46. **Logging**: Hierarchy, formatters, stream/file handlers, exception logging.
47. **Command Line Parsing**: `argparse` module, arguments, flags, subcommands.
48. **OS & System Interfaces**: `os.environ`, `sys.argv`, `subprocess.run()`.
49. **Dates & Times**: `datetime`, `timezone`, UTC handling, `zoneinfo`.
50. **Docstrings**: PEP 257 docstring conventions, `help()` inspection.

---

## 🎯 4. Top 50 Questions That Differentiate Top Candidates

Advanced questions used by Staff+ interviewers to evaluate deep expertise.

1. **Python Data Model & Attribute Resolution**: Contrast `__getattr__`, `__getattribute__`, and Data Descriptors.
2. **Zero-Copy Memory Protocol**: Explain `memoryview` C-buffer sharing and slicing without RAM copies.
3. **Coroutine Lifecycle Mechanics**: Detail coroutine suspension via `yield`, state machine transitions, `send()`, `throw()`, and `close()`.
4. **Thread-Safe & Process-Safe Shared Counters**: Compare `threading.Lock`, `multiprocessing.Value`, and `Atomic` operations.
5. **Plugin Architecture via Packaging Entry Points**: Design a plugin system using `importlib.metadata.entry_points()`.
6. **Invalidatable Lazy Property**: Write a custom `@lazy_property` decorator supporting explicit cache invalidation.
7. **Extending `singledispatch` to Methods**: Overcome `functools.singledispatch` limitations when applied to class methods.
8. **Static AST Linter Construction**: Build a custom code linter parsing Python abstract syntax trees via `ast.NodeVisitor`.
9. **Resource Cleanup with `weakref.finalize`**: Implement safe resource destruction without `__del__` resurrection risks.
10. **Generator Backpressure Pipeline**: Build a streaming generator pipeline with explicit queue backpressure control.
11. **CPython PyObject Memory Layout**: Explain header fields (`ob_refcnt`, `ob_type`) and memory alignment.
12. **PyMalloc Small Object Allocator**: Describe arenas, pools, and blocks for objects $\le 512$ bytes.
13. **Tri-Color Marking in Generational GC**: How CPython isolates unreachable cyclic references in container objects.
14. **Custom Async Event Loop Multiplexing**: Integrating `selectors` module with an async task queue.
15. **Structured Concurrency in Python 3.11**: Compare `asyncio.TaskGroup` error propagation with `asyncio.gather()`.
16. **Context Var Isolation in Async Coroutines**: How `contextvars.ContextVar` propagates state across async tasks without thread-locals.
17. **C3 Linearization Algorithm**: Compute MRO for complex multiple inheritance topologies manually.
18. **Metaclass `__call__` Interception**: Overriding `Meta.__call__` to control instance allocation and initialization.
19. **`__init_subclass__` vs Metaclass**: When to prefer `PEP 487` subclass hooks over metaclass inheritance.
20. **Buffer Protocol Slicing**: How `memoryview` handles contiguous vs non-contiguous C array buffers.
21. **C-Extensions GIL Release**: How `Py_BEGIN_ALLOW_THREADS` enables true C multi-threading.
22. **Subprocess Pipe Deadlock Diagnosis**: Diagnosing OS pipe buffer saturation in `subprocess.Popen`.
23. **Custom Import Finders & Loaders**: Implementing `importlib.abc.MetaPathFinder` to dynamically load remote code.
24. **Pickle Vulnerability Exploitation**: Crafting a malicious `__reduce__` payload executing arbitrary shell code.
25. **`sys.settrace` Debugger Construction**: Building a step-debugger recording line execution and variable states.
26. **Global Interpreter Lock in Python 3.13+**: Details of free-threaded CPython (`--disable-gil`) build changes.
27. **AST Code Transformation**: Parsing, mutating AST nodes, and unparsing code back to Python source (`ast.unparse`).
28. **Thread-Safe LRU Cache with Lock Striping**: Designing low-contention concurrent caches.
29. **Custom Container Slice View Handlers**: Supporting step, negative bounds, and extended slicing in custom classes.
30. **Weak Value Dictionary Cleanup Mechanics**: How weak reference callbacks delete dictionary keys upon object garbage collection.
31. **Type System Structural Subtyping**: Contrast `typing.Protocol` with nominal subtyping (`abc.ABC`).
32. **Exception Group Handling (`except*`)**: Handling multiple concurrent exceptions raised within `TaskGroup`.
33. **Optimizing Python Memory with Mmap**: Processing gigabyte binary files using `mmap.mmap()` memory maps.
34. **C-API Reference Count Management**: Avoiding memory leaks and dangling pointers when calling `Py_INCREF` / `Py_DECREF`.
35. **Bytecode Peephole Optimization**: How CPython optimizes constant folding and load operations at compile time.
36. **Python Stack Frames & Frame Objects**: Inspecting `sys._getframe()`, local environments, and call stacks.
37. **Thread State and Re-entrant Locks**: Contrast `threading.Lock` with `threading.RLock` under recursive calls.
38. **Atomic File Writes in Production**: Guaranteeing crash-safe file writes using temporary files and atomic `os.replace()`.
39. **High-Performance Serialization**: Comparing `pickle`, `msgpack`, `protobuf`, and zero-copy `FlatBuffers`.
40. **Garbage Collection Disabling Strategies**: Why high-throughput web servers (Instagram) disable GC during worker runtime.
41. **Custom Dataclass Code Generation**: How `@dataclass` dynamically generates dunder methods via string evaluation.
42. **Dynamic Class Creation with `type()`**: Building classes at runtime dynamically passing namespace dictionaries.
43. **Signal Handling in Multi-Threaded Applications**: How Python routes OS signals (`SIGINT`, `SIGTERM`) to main thread only.
44. **Context Manager Exception Suppression Mechanics**: Evaluating `__exit__` return values and exception propagation.
45. **Function Signature Inspection (`inspect` Module)**: Binding parameters dynamically using `inspect.signature()`.
46. **Coroutines as Finite State Machines**: Designing stateful event processors using `yield` and `send()`.
47. **Thread Pool Executor Cancellation Limits**: Why running tasks in `ThreadPoolExecutor` cannot be cancelled mid-execution.
48. **Overcoming Python Memory Fragmentation**: Strategies for freeing memory back to OS (`malloc_trim`).
49. **Type Checking Generics (`TypeVar`, `Generic`)**: Implementing generic data structures with static type enforcement.
50. **Migrating Codebases to Modern Type Syntax**: Systematic AST-based refactoring from `typing` module to Python 3.12+ syntax.
