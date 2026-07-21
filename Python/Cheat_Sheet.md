# ⚡ Python Quick Revision Cheat Sheet

A high-density reference sheet designed for 5-minute pre-interview review. Covers core syntax, high-value comparison tables, dunder protocols, concurrency matrices, and common pitfall traps.

---

## 🚀 1. Core Syntax & Dunder Methods Cheat Sheet

| Dunder Method | Trigger Expression | Purpose & Context |
|---------------|-------------------|-------------------|
| `__init__(self, ...)` | `obj = MyClass()` | Instance initializer (returns `None`). |
| `__new__(cls, ...)` | `obj = MyClass()` | Instance creator (allocates memory, returns new instance). Used for Singletons and Immutable subclassing. |
| `__str__(self)` | `str(obj)`, `print(obj)` | User-readable string representation. |
| `__repr__(self)` | `repr(obj)`, REPL output | Unambiguous developer string representation (ideally valid Python code `eval(repr(x)) == x`). |
| `__call__(self, ...)` | `obj(*args)` | Makes instance callable like a function. |
| `__getitem__(self, key)` | `obj[key]` | Indexing/slicing read access. |
| `__setitem__(self, key, val)`| `obj[key] = val` | Indexing write access. |
| `__len__(self)` | `len(obj)` | Collection size (must return non-negative int). |
| `__enter__` / `__exit__` | `with obj as x:` | Context manager protocol. `__exit__` returns `True` to suppress exceptions. |
| `__iter__` / `__next__` | `for x in obj:` | Iterator protocol. `__next__` raises `StopIteration` when exhausted. |
| `__eq__` / `__hash__` | `a == b`, `hash(obj)` | Equality & hashing. If `__eq__` is overridden, `__hash__` defaults to `None` unless defined! |
| `__slots__` | Class attribute | Restricts dynamic attributes, eliminates per-instance `__dict__` memory. |

---

## 📊 2. High-Value Comparison Tables

### Table A: Mutable vs Immutable Data Types
| Characteristic | Mutable (`list`, `dict`, `set`) | Immutable (`int`, `str`, `tuple`, `frozenset`) |
|----------------|---------------------------------|------------------------------------------------|
| **In-Place Modification** | Supported (`a.append(1)`) | Unsupported (`TypeError`) |
| **Hashability (`__hash__`)** | Not Hashable (cannot be dict keys/set elements) | Hashable (if all contained items are hashable) |
| **Memory Allocation** | Dynamic resizing, higher overhead | Fixed allocation, optimized C memory |
| **Pass-by-Assignment** | Mutations reflect in caller scope | Rebinding creates new object reference |

### Table B: Method Types (`instance` vs `@classmethod` vs `@staticmethod`)
| Dimension | Instance Method | `@classmethod` | `@staticmethod` |
|-----------|-----------------|----------------|-----------------|
| **First Parameter** | `self` (Instance) | `cls` (Class) | None |
| **Access Scope** | Can access instance & class state | Can access class state only | Isolated function in class namespace |
| **Primary Use Case** | Standard state manipulation | Factory methods, alternative constructors | Self-contained helper utilities |

### Table C: Concurrency Models Comparison
| Feature | `threading` | `multiprocessing` | `asyncio` |
|---------|-------------|-------------------|-----------|
| **Execution Architecture** | OS Threads in single process | Multiple OS Processes | Single thread cooperative event loop |
| **GIL Bound?** | **Yes** (Only 1 thread runs bytecode) | **No** (Independent GIL per process) | **Yes** (Single thread execution) |
| **Best For** | I/O-bound tasks (network, disk) | CPU-bound tasks (math, ML) | High-concurrency I/O (10k+ web sockets) |
| **Memory Overhead** | Low (shared memory space) | High (IPC serialization & process overhead)| Minimal (coroutines lightweight) |
| **Data Sharing** | Shared memory (requires locks!) | `Queue`, `Pipe`, `Value`, `Manager` | Shared single-thread state |

### Table D: Object Identity (`is`) vs Equality (`==`)
| Operator | Mechanism | Example Case |
|----------|-----------|--------------|
| `==` | Calls `a.__eq__(b)` | `[1, 2] == [1, 2]` $\rightarrow$ `True` |
| `is` | Compares memory address `id(a) == id(b)` | `[1, 2] is [1, 2]` $\rightarrow$ `False` |

---

## 💡 3. Memory Tricks & Quick Interview Notes

1. **Small Integer Caching**: CPython pre-allocates integer objects from `-5` to `256`.
   - `a = 256; b = 256; a is b` $\rightarrow$ `True`.
   - `a = 257; b = 257; a is b` $\rightarrow$ `False` (in script mode, compiler flags may merge constants in same code block, but distinct runtime allocations yield `False`).
2. **String Interning**: Identical string literals matching identifier syntax (`[a-zA-Z0-9_]*`) are automatically interned by CPython. Forced interning via `sys.intern(s)`.
3. **C3 Linearization MRO Rule**: Method Resolution Order in multiple inheritance.
   - Equation: $L(C(B_1 \dots B_N)) = C + \text{merge}(L(B_1), \dots, L(B_N), B_1 \dots B_N)$.
   - View class MRO via `ClassName.__mro__`.
4. **`__slots__` Memory Advantage**: Defining `__slots__ = ('x', 'y')` skips `__dict__` creation, saving ~200 bytes per object instance.

---

## ⚠️ 4. Production Pitfalls & Common Bugs

```python
# PITFALL 1: Late Binding in Closures
# Bug: Functions print [2, 2, 2] because i is bound at call time!
funcs = [lambda: i for i in range(3)]
print([f() for f in funcs]) # Output: [2, 2, 2]

# Fix: Use default argument to bind variable at definition time
funcs = [lambda i=i: i for i in range(3)]
print([f() for f in funcs]) # Output: [0, 1, 2]


# PITFALL 2: Modifying Tuple with Contained Mutable Elements
t = (1, 2, [3, 4])
try:
    t[2] += [5]
except TypeError:
    pass
# Result: Exception is raised BUT list IS modified! t becomes (1, 2, [3, 4, 5])
# Reason: += evaluates in-place extend on list first, then attempts assignment back to tuple index!


# PITFALL 3: Catching Base Exception vs Specific Exceptions
# WRONG: Swallows KeyboardInterrupt, SystemExit, and obscures bugs
try:
    process()
except Exception as e: # Better than bare `except:` but specify concrete types where possible!
    logger.error(e)
```

---

## ⚡ 5. LEGB Scope Resolution Summary

```
+------------------------------------+
| BUILT-IN (len, range, Exception)   |
|  +-------------------------------+ |
|  | GLOBAL (Module-level names)   | |
|  |  +--------------------------+ | |
|  |  | ENCLOSING (nonlocal)     | | |
|  |  |  +---------------------+ | | |
|  |  |  | LOCAL (def / lambda)| | | |
|  |  |  +---------------------+ | | |
|  |  +--------------------------+ | |
|  +-------------------------------+ |
+------------------------------------+
```
*Variables marked `global x` bind to GLOBAL scope. Variables marked `nonlocal x` bind to nearest ENCLOSING scope.*
