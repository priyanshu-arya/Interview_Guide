# 🏢 Top Company-Inspired Python Interview Questions & Frequently Rejected Patterns

A curated collection of company-specific technical interview questions and the **Top 50 Frequently Rejected Questions** inspired by real-world candidate evaluation patterns at FAANG, Unicorns, and top tech companies.

---

## 📋 Table of Contents

1. [Top 50 Frequently Rejected Questions & Candidate Traps](#top-50-frequently-rejected-questions)
2. [Company-Inspired Question Bank & Architecture Scenarios](#company-inspired-question-bank)

---

## 🚫 Top 50 Frequently Rejected Questions & Candidate Traps

Where candidates fail due to incomplete knowledge, missing edge cases, overgeneralizations, or non-production code.

| # | Question asked by Interviewer | Candidate's Flawed Answer (Rejection Reason) | Proper Production-Grade Answer |
|---|-------------------------------|----------------------------------------------|--------------------------------|
| 1 | "What is the Global Interpreter Lock (GIL)?" | "Python only runs 1 thread at a time, so threading is completely useless." | GIL prevents parallel execution of Python bytecode across multi-cores, BUT CPython releases GIL during OS I/O system calls. Threading is effective for I/O-bound tasks. |
| 2 | "When would you use `__slots__`?" | "Always, because it makes everything faster and better." | `__slots__` eliminates `__dict__` to save memory. BUT it restricts dynamic attribute creation and requires careful inheritance handling (slots needed in parent and child). |
| 3 | "How to deduplicate a list preserving order?" | "Convert to set then back to list (`list(set(l))`)." | Converting to `set` destroys original order. Use `list(dict.fromkeys(l))` or `[x for i, x in enumerate(l) if x not in l[:i]]`. |
| 4 | "Difference between `append()` and `extend()`" | Confuses appending iterable as single nested element vs extending elements. | `lst.append([1,2])` yields `[..., [1,2]]`; `lst.extend([1,2])` yields `[..., 1, 2]`. |
| 5 | "Implement a Singleton in Python" | Uses global variable or misses thread-safety locks. | Implement double-checked locking using `threading.Lock()` inside `__new__`, or use module-level singletons. |
| 6 | "What are decorators?" | "Functions that change other functions" but fails to write `@wraps` or handling arguments. | Higher-order functions wrapping callables. Must use `@functools.wraps` to preserve `__name__` and `__doc__`. |
| 7 | "How to handle large 50GB file processing?" | Reads entire file into RAM via `f.readlines()`. | Use generator streams (`for line in f:` or chunked `f.read(8192)`), consuming $O(1)$ memory. |
| 8 | "What is `asyncio` and how does it work?" | "It runs functions in parallel on multiple CPU cores." | Single-threaded cooperative multitasking via event loop. Does not provide CPU parallelism. |
| 9 | "Write a custom Context Manager" | Forgets `__exit__` parameter signature or doesn't return resource in `__enter__`. | Must define `__enter__(self)` returning resource, and `__exit__(self, exc_type, exc_val, exc_tb)`. |
| 10 | "What is the MRO in multiple inheritance?" | Cannot explain Diamond Problem or C3 Linearization. | C3 Linearization determines deterministic resolution order accessible via `Class.mro()`. |
| 11 | "Difference between `is` and `==`" | Reverses meanings ("`is` checks value, `==` checks reference"). | `==` checks value equality (`__eq__`); `is` checks memory address identity (`id(a) == id(b)`). |
| 12 | "Explain monkey patching" | "Editing source code files on disk at runtime." | Dynamically overriding class/module attributes in memory at runtime (`module.func = mock_func`). |
| 13 | "Why use `if __name__ == '__main__':`?" | "Required for script to execute." | Prevents top-level code from executing automatically when module is imported by another script. |
| 14 | "How does argument passing work?" | "Pass by value" or "Pass by reference". | Python uses **pass-by-assignment**. Rebinding local parameter doesn't affect caller; mutating object in-place does. |
| 15 | "What happens when catching `Exception`?" | Bare `except:` or catching `BaseException`. | Bare `except:` swallows `KeyboardInterrupt` and `SystemExit`. Catch specific exceptions or `Exception`. |
| 16 | "Difference between `__getattr__` and `__getattribute__`" | Thinks both do the same thing. | `__getattribute__` intercepts EVERY access; `__getattr__` is called ONLY when attribute is missing. |
| 17 | "How do default arguments work?" | Uses mutable default `def f(lst=[])`. | Default arguments are evaluated ONCE at function definition time, creating persistent state across calls. |
| 18 | "Why is `lambda` limited?" | "It's just slower." | Syntax restriction: lambdas can only contain a single expression, not multi-line statements. |
| 19 | "What does `yield from` do?" | "Same as `yield` in a loop." | Delegating iterator protocol: transparently channels values, exceptions, and `send()` returns to sub-generators. |
| 20 | "Difference between `threading` and `multiprocessing`" | Confuses memory sharing and GIL impact. | Threads share memory space but bound by GIL; processes have separate RAM and bypass GIL. |
| 21 | "What is string interning?" | Doesn't know why `'a' * 20 is 'a' * 20` is True. | Reusing immutable string allocations for identifier-like string literals. |
| 22 | "How does `dict` maintain insertion order?" | "It doesn't, sets and dicts are unordered." | Since Python 3.7+, dicts use compact array + hash table layout preserving insertion order. |
| 23 | "What is `super()`?" | "Calls parent class method" without understanding MRO dispatch. | Calls next method in the class's C3 MRO chain, enabling cooperative multiple inheritance. |
| 24 | "Difference between `range` and `xrange`" | Mentions `xrange` in Python 3 context. | `xrange` was Python 2; Python 3 `range` is a lazy sequence type producing items on demand. |
| 25 | "How to copy an object?" | `b = a` creates a copy. | Assignment `=` creates a new reference to the SAME object. Use `copy.copy()` or `copy.deepcopy()`. |
| 26 | "What is `__init__`?" | "It creates the object in memory." | `__new__` creates the object; `__init__` initializes attributes on the created instance. |
| 27 | "How to make a variable private?" | "Python has `private` keyword." | Python uses name mangling (`__var`) and convention (`_var`); no hard access control keywords. |
| 28 | "What is a closure?" | Cannot explain non-local scope variable retention. | Function retaining bindings to free variables in its enclosing lexical scope. |
| 29 | "Difference between `map()` and list comprehension" | "List comprehension is always faster." | `map()` returns a lazy iterator; comprehension builds eager list. Speed depends on C-builtins. |
| 30 | "What is `functools.wraps`?" | "Makes decorators run faster." | Preserves `__name__`, `__doc__`, and metadata of wrapped function on decorator wrapper. |
| 31 | "How to profile Python code?" | Using `print(time.time())` everywhere. | Use standard profiling tools: `cProfile`, `line_profiler`, and `tracemalloc`. |
| 32 | "What is `typing.Protocol`?" | "Same as `abc.ABC`." | Structural subtyping (duck typing static check) vs nominal subtyping (`abc.ABC`). |
| 33 | "How does `sys.path` work?" | Confuses OS `PATH` with Python `sys.path`. | `sys.path` is list of directory paths searched by Python for module imports. |
| 34 | "What is `dataclass`?" | "Generates database table schemas." | `@dataclass` generates boilerplate dunder methods (`__init__`, `__repr__`, `__eq__`) for classes. |
| 35 | "Why use `f-strings`?" | "Just syntax sugar for `%` format." | Faster execution (evaluated at runtime as formatted bytecode) and better readability. |
| 36 | "How to handle timezone in datetime?" | Using `datetime.now()` without timezone info. | Naive datetimes cause bugs. Use timezone-aware datetimes (`datetime.now(timezone.utc)`). |
| 37 | "What is `weakref`?" | "Reduces memory size of objects." | Reference to object that doesn't increment `ob_refcnt`, preventing reference cycles. |
| 38 | "What is `pickle` security risk?" | "Pickle files can corrupt." | Arbitrary code execution vulnerability: untrusted unpickling executes payload in `__reduce__`. |
| 39 | "What is `asyncio.TaskGroup`?" | Never heard of Python 3.11 TaskGroup. | Structured concurrency context manager guaranteeing task completion or exception cleanup. |
| 40 | "How to create custom exception?" | Inherits from `BaseException`. | BaseException catches system exit signals. Custom exceptions should inherit from `Exception`. |
| 41 | "Difference between `iter()` and `next()`" | Confuses iterable and iterator protocols. | `iter()` returns iterator object (`__iter__`); `next()` retrieves next item (`__next__`). |
| 42 | "What is `contextvars`?" | Uses `threading.local` in async code. | `contextvars` isolates context state across async coroutines executing on single thread. |
| 43 | "What is `memoryview`?" | "Converts bytes to string." | Exposes C-level buffer protocol for zero-copy memory slicing. |
| 44 | "What does `:=` walrus operator do?" | "Assigns global variables." | Assignment expression: assigns value to variable within an expression context. |
| 45 | "How to force GC run?" | `del obj` triggers full GC immediately. | `del` decrements ref count; `gc.collect()` explicitly triggers generational cyclic collector. |
| 46 | "What is `__hash__` requirement?" | "Any object can be hashed." | Object must be immutable and its hash value must remain constant throughout lifetime. |
| 47 | "Difference between `dict.get(k)` and `dict[k]`" | Confuses exception behavior. | `dict[k]` raises `KeyError` if key missing; `dict.get(k)` returns `None` or default sentinel. |
| 48 | "How to re-raise exception in `except` block?" | `raise e` (losing original traceback stack). | Use bare `raise` to preserve stack trace, or `raise NewExc() from e` for explicit chaining. |
| 49 | "What is `__call__` dunder method?" | "Calls superclass method." | Allows instance of a class to be invoked like a function (`obj()`). |
| 50 | "How to break out of nested loops?" | Using `break` expecting it to break all outer loops. | `break` only breaks innermost loop. Use flag variable, helper function return, or `for/else`. |

---

## 🏢 Company-Inspired Question Bank & Architecture Scenarios

### 1. High-Concurrency Rate Limiter Decorator (Netflix, Uber, Stripe, Razorpay)
- **Problem Scenario**: Design a decorator `@rate_limit(max_calls=10, period=1.0)` restricting function calls to at most $N$ executions per time window.
- **Implementation**:
  ```python
  import time, threading
  from functools import wraps

  class RateLimitExceeded(Exception): pass

  def rate_limit(max_calls: int, period: float):
      def decorator(func):
          calls = []
          lock = threading.Lock()
          @wraps(func)
          def wrapper(*args, **kwargs):
              nonlocal calls
              with lock:
                  now = time.perf_counter()
                  calls = [t for t in calls if now - t < period]
                  if len(calls) >= max_calls:
                      raise RateLimitExceeded(f"Rate limit of {max_calls} calls/{period}s exceeded")
                  calls.append(now)
              return func(*args, **kwargs)
          return wrapper
      return decorator
  ```

### 2. Thread-Safe Connection Pool (Amazon, Microsoft, Databricks)
- **Problem Scenario**: Implement a `ConnectionPool` limiting open sockets using `queue.Queue` and context managers.

### 3. Zero-Copy Binary File Stream Aggregator (Google, Nvidia, Meta)
- **Problem Scenario**: Parse gigabyte binary files using `struct` and `memoryview` without loading file into RAM.

### 4. Async Task Retry Engine with Jitter (Spotify, Swiggy, Zomato)
- **Problem Scenario**: Exponential backoff retry engine for async coroutines using non-blocking `asyncio.sleep`.

### 5. Bounded Integer Field Descriptor (Adobe, Apple, Microsoft)
- **Problem Scenario**: Descriptor validating min/max bounds and type enforcement on class attributes.
