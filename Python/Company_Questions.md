# 🏢 Top Company-Inspired Python Interview Questions

A curated collection of Python interview themes, architecture tasks, and coding challenges inspired by recurring hiring patterns at top-tier tech companies.

---

## 🎯 Company Categories & Hiring Focus

| Category | Representative Companies | Technical Focus Areas |
|----------|--------------------------|-----------------------|
| **Big Tech / Cloud Infrastructure** | Google, Microsoft, Meta, Amazon, Apple | CPython internals, memory management, garbage collection, static typing, low-level concurrency, custom libraries. |
| **High-Scale Streaming & Backend** | Netflix, Uber, Spotify, Stripe, Airbnb | `asyncio` event loops, non-blocking I/O, custom context managers, zero-copy buffer views, microservice connection pooling. |
| **AI Infrastructure & Compute Labs** | Nvidia, OpenAI, Databricks, Hugging Face | Memory profiling (`tracemalloc`), zero-copy buffer protocols (`memoryview`), C-Extensions, multiprocessing, PyTorch/NumPy interop. |
| **FinTech & High-Throughput E-Commerce** | Razorpay, Swiggy, Zomato, PayPal, Adobe | Thread safety, atomic operations, cache eviction algorithms (LRU/LFU), fault-tolerant decorator pipelines. |

---

## 📌 Company-Inspired Question Bank

### 1. High-Concurrency Rate Limiter Decorator
- **Commonly Asked By**: Netflix, Uber, Stripe, Razorpay
- **Why This Question Matters**: Tests understanding of closures, state management across requests, threading locks, and decorator wrapper architecture.
- **Problem Scenario**: Design a decorator `@rate_limit(max_calls=10, period=1.0)` that restricts a function call to at most $N$ executions per time window. Excess calls must raise a custom `RateLimitExceeded` exception.

```python
import time
import threading
from functools import wraps

class RateLimitExceeded(Exception):
    pass

def rate_limit(max_calls: int, period: float):
    def decorator(func):
        calls = []
        lock = threading.Lock()

        @wraps(func)
        def wrapper(*args, **kwargs):
            nonlocal calls
            with lock:
                now = time.perf_counter()
                # Remove timestamps older than period
                calls = [t for t in calls if now - t < period]
                if len(calls) >= max_calls:
                    raise RateLimitExceeded(f"Rate limit of {max_calls} calls per {period}s exceeded.")
                calls.append(now)
            return func(*args, **kwargs)
        return wrapper
    return decorator
```

- **Expected Answer & Follow-up Questions**:
  1. *Follow-up*: How does this scale in multi-process worker environments? *(Requires a shared distributed store like Redis using leaky bucket / token bucket Lua scripts).*
  2. *Follow-up*: What is the time complexity of the list filtering step? How would you optimize it? *(Use `collections.deque` with sliding window for $O(1)$ pop operations).*
- **Interviewer Tips**: Focus on thread safety (`threading.Lock`) and correct state retention using `nonlocal`.

---

### 2. Custom Thread-Safe Connection Pool Manager
- **Commonly Asked By**: Amazon, Microsoft, Databricks
- **Why This Question Matters**: Evaluates object resource management, context manager protocols, semaphores, and graceful cleanup.
- **Problem Scenario**: Create a reusable `ConnectionPool` class that limits total open socket connections. Code using `with pool.get_connection() as conn:` must block if no connections are available and return the connection to the pool upon exit.

```python
import queue
import threading
from contextlib import contextmanager

class ConnectionPool:
    def __init__(self, capacity: int):
        self._pool = queue.Queue(maxsize=capacity)
        for i in range(capacity):
            self._pool.put(f"DB_Connection_{i+1}")

    @contextmanager
    def get_connection(self, timeout: float = 5.0):
        try:
            conn = self._pool.get(block=True, timeout=timeout)
        except queue.Empty:
            raise RuntimeError("Connection pool exhausted timeout")
        try:
            yield conn
        finally:
            self._pool.put(conn)
```

- **Expected Answer & Follow-up Questions**:
  1. *Follow-up*: What happens if the connection becomes stale or dropped during use? *(Implement health-check ping before yielding, recreating broken socket connections).*
- **Interviewer Tips**: Use `queue.Queue` because it handles internal thread locking and blocking wait logic natively.

---

### 3. Zero-Copy Large Binary File Stream Aggregator
- **Commonly Asked By**: Google, Nvidia, Meta, Airbnb
- **Why This Question Matters**: Distinguishes candidates who know how to avoid $O(N)$ memory crashes when handling multi-gigabyte files.
- **Problem Scenario**: Read a giant binary stream or log file, extract fixed headers using `struct` and `memoryview`, and aggregate stats without loading the whole file into RAM.

```python
import struct

def parse_binary_records(filepath: str):
    # Header format: 4 bytes unsigned int (ID), 8 bytes double (value)
    record_struct = struct.Struct(">Id") 
    record_size = record_struct.size

    with open(filepath, "rb") as f:
        while chunk := f.read(record_size * 1000): # Read batch of 1000 records
            view = memoryview(chunk)
            for offset in range(0, len(view), record_size):
                record_bytes = view[offset : offset + record_size]
                rec_id, val = record_struct.unpack(record_bytes)
                yield rec_id, val
```

- **Expected Answer & Follow-up Questions**:
  1. *Follow-up*: How does `memoryview` avoid memory copying? *(Refers to underlying buffer pointer directly without creating intermediate bytes objects).*

---

### 4. Custom Async Task Retry Engine with Backoff
- **Commonly Asked By**: Spotify, Uber, Swiggy, Zomato
- **Why This Question Matters**: Tests understanding of `asyncio` coroutines, task scheduling, exception handling, and non-blocking sleep.
- **Problem Scenario**: Write an `async def async_retry(...)` wrapper that retries an async callable with exponential backoff and randomized jitter.

```python
import asyncio
import random
import logging
from typing import Callable, Any

async def async_retry(
    coro_func: Callable[..., Any],
    *args,
    max_retries: int = 3,
    base_delay: float = 1.0,
    **kwargs
) -> Any:
    for attempt in range(1, max_retries + 1):
        try:
            return await coro_func(*args, **kwargs)
        except Exception as exc:
            if attempt == max_retries:
                raise exc
            jitter = random.uniform(0, 0.5)
            delay = (base_delay * (2 ** (attempt - 1))) + jitter
            logging.warning(f"Attempt {attempt} failed ({exc}). Retrying in {delay:.2f}s...")
            await asyncio.sleep(delay) # NON-BLOCKING sleep!
```

- **Expected Answer & Follow-up Questions**:
  1. *Follow-up*: Why must you use `await asyncio.sleep(delay)` instead of `time.sleep(delay)`? *(Because `time.sleep` blocks the entire single-threaded asyncio event loop, freezing all concurrent coroutines!)*.

---

### 5. ORM-Style Field Descriptor with Type & Range Validation
- **Commonly Asked By**: Adobe, Microsoft, Apple, Swiggy
- **Why This Question Matters**: Evaluates metaprogramming mastery, descriptor protocols (`__set_name__`, `__get__`, `__set__`), and clean class architectures.

```python
class BoundedInteger:
    def __init__(self, min_val: int, max_val: int):
        self.min_val = min_val
        self.max_val = max_val

    def __set_name__(self, owner, name):
        self.private_name = f"_{name}"

    def __get__(self, instance, owner):
        if instance is None:
            return self
        return getattr(instance, self.private_name, self.min_val)

    def __set__(self, instance, value):
        if not isinstance(value, int):
            raise TypeError(f"{self.private_name[1:]} must be an integer.")
        if not (self.min_val <= value <= self.max_val):
            raise ValueError(f"Value {value} out of bounds [{self.min_val}, {self.max_val}]")
        setattr(instance, self.private_name, value)

class ProductInventory:
    quantity = BoundedInteger(min_val=0, max_val=10_000)
    price = BoundedInteger(min_val=1, max_val=1_000_000)
```
