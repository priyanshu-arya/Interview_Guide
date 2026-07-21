# 🧪 Python Practice Questions & Exercises

A structured collection of hands-on practice problems designed for interview preparation across 8 distinct categories.

---

## 📌 Section 1: Concept Questions

### Problem 1.1: CPython Garbage Collection & Cycle Resolution
- **Difficulty**: Medium
- **Problem Statement**: Explain step-by-step how CPython detects and reclaims a cyclic reference between two custom instances when both variable names are deleted in the main script scope.
- **Hint**: Think about `ob_refcnt` vs generational GC (`gc.collect()`) tri-color marking.
- **Expected Approach**:
  1. Primary step: Reference counting decrements `ob_refcnt` from 2 to 1 for both objects when `del` is executed. Neither reaches 0.
  2. Secondary step: Cyclic GC runs (Gen 0/1/2 threshold reached).
  3. GC copies reference counts of candidate container objects (`gc_refs`).
  4. GC subtracts 1 for each reference originating from container objects in the candidate set.
  5. Objects whose `gc_refs` drop to 0 are marked candidate-unreachable and collected.
- **Common Mistakes**: Saying "CPython automatically deletes cycles instantly when `del` is called". Reference counting cannot resolve cycles instantly; generational GC is required.

---

## 📌 Section 2: Coding Questions

### Problem 2.1: Flatten Nested Iterable Generator
- **Difficulty**: Medium
- **Problem Statement**: Write a recursive generator function `flatten(nested_iterable)` that yields atomic elements from arbitrary nested lists, tuples, or sets, while skipping string characters from being unpacked as iterables.
- **Hint**: Check `isinstance(x, (str, bytes))` to prevent infinite string recursion!
- **Expected Approach**:
  ```python
  def flatten(iterable):
      for item in iterable:
          if isinstance(item, (str, bytes)):
              yield item
          else:
              try:
                  yield from flatten(item)
              except TypeError:
                  yield item
  ```
- **Common Mistakes**: Forgetting that strings are iterables in Python (`iter("abc")` works), leading to `RecursionError`.

---

## 📌 Section 3: Debugging Questions

### Problem 3.1: Silent Memory Leak in Class Caching
- **Difficulty**: Hard
- **Problem Statement**: Identify the memory leak in the following cache implementation and rewrite it to fix the memory growth issue.
  ```python
  class ImageProcessor:
      _cache = {}

      def process(self, image_id: str, data: bytes):
          if image_id not in self._cache:
              self._cache[image_id] = data
          return self._cache[image_id]
  ```
- **Hint**: Strong references inside class-level dictionary `_cache` persist indefinitely across the application lifetime.
- **Expected Approach**: Use `weakref.WeakValueDictionary` or `@functools.lru_cache(maxsize=N)`.
  ```python
  import weakref

  class ImageProcessor:
      def __init__(self):
          self._cache = weakref.WeakValueDictionary()
  ```
- **Common Mistakes**: Clearing `_cache` manually without thread locks or unbounded global memory growth.

---

## 📌 Section 4: Output Prediction Questions

### Problem 4.1: Tuple Mutability Exception Paradox
- **Difficulty**: Medium
- **Problem Statement**: Predict the exact output and explain why it occurs:
  ```python
  t = (1, 2, [3, 4])
  try:
      t[2] += [5, 6]
  except Exception as e:
      print(f"Caught: {type(e).__name__}")

  print(f"Final Tuple: {t}")
  ```
- **Hint**: `+=` on a list invokes `__iadd__` (in-place extension) BEFORE trying to assign back to `t[2]`.
- **Expected Approach**:
  - `t[2] += [5, 6]` first looks up `t[2]` (list object `[3, 4]`) and calls `__iadd__`, extending the list in RAM to `[3, 4, 5, 6]`.
  - Next, it attempts to store the modified reference back into `t[2]`. Since `t` is a tuple, `t.__setitem__` fails and raises `TypeError`.
  - Output:
    `Caught: TypeError`
    `Final Tuple: (1, 2, [3, 4, 5, 6])`
- **Common Mistakes**: Predicting that the list inside the tuple remains unchanged `(1, 2, [3, 4])`.

---

## 📌 Section 5: Scenario-Based Questions

### Problem 5.1: High-Throughput Log Stream Processing
- **Difficulty**: Hard
- **Problem Statement**: You are tasked with designing a log processor that reads a 50GB log file on a machine with 4GB RAM. You must parse lines, count occurrences of `"ERROR"`, and write error logs to an output file.
- **Hint**: Pipeline generator functions!
- **Expected Approach**:
  ```python
  def read_lines(filepath):
      with open(filepath, "r", encoding="utf-8") as f:
          for line in f:
              yield line

  def filter_errors(lines):
      for line in lines:
          if "ERROR" in line:
              yield line

  def process_logs(in_file, out_file):
      lines = read_lines(in_file)
      errors = filter_errors(lines)
      with open(out_file, "w", encoding="utf-8") as out:
          for err in errors:
              out.write(err)
  ```
- **Common Mistakes**: Calling `f.readlines()` (loads entire 50GB into RAM, triggering OS OOM killer).

---

## 📌 Section 6: Tricky Questions

### Problem 6.1: Key Collision with Booleans and Integers
- **Difficulty**: Medium
- **Problem Statement**: What is the length of dictionary `d = {1: "one", True: "true", 1.0: "float_one"}` and why?
- **Hint**: Check equality `1 == True == 1.0` and hash values `hash(1) == hash(True) == hash(1.0)`.
- **Expected Approach**:
  - `True == 1` evaluates to `True`, and `hash(True) == hash(1) == 1`.
  - `1.0 == 1` evaluates to `True`, and `hash(1.0) == 1`.
  - All three keys collide in hash value AND equality! The dictionary overwrites the value at key `1` while keeping the original key object (`1`).
  - Output: `len(d) == 1`, `d` is `{1: "float_one"}`.
- **Common Mistakes**: Thinking `1`, `True`, and `1.0` create 3 distinct keys.

---

## 📌 Section 7: Practical Problems

### Problem 7.1: Thread-Safe In-Memory Metrics Collector
- **Difficulty**: Medium
- **Problem Statement**: Build a thread-safe metrics collector class `Metrics` with methods `increment(key)` and `get_stats()`.
- **Hint**: Use `collections.defaultdict` protected by `threading.Lock`.
- **Expected Approach**:
  ```python
  import threading
  from collections import defaultdict

  class Metrics:
      def __init__(self):
          self._counts = defaultdict(int)
          self._lock = threading.Lock()

      def increment(self, key: str, amount: int = 1) -> None:
          with self._lock:
              self._counts[key] += amount

      def get_stats(self) -> dict:
          with self._lock:
              return dict(self._counts)
  ```

---

## 📌 Section 8: Interview Challenges

### Problem 8.1: Custom `asyncio` Event Loop Coroutine Task Scheduler
- **Difficulty**: Very Hard
- **Problem Statement**: Implement a simplified generator-based cooperative task scheduler that runs multiple coroutines until all finish, supporting non-blocking task yielding.
- **Hint**: Maintain a `collections.deque` of generators, calling `next()` or `send()` in a loop!
- **Expected Approach**:
  ```python
  from collections import deque

  class SimpleScheduler:
      def __init__(self):
          self.task_queue = deque()

      def add_task(self, coro):
          self.task_queue.append(coro)

      def run(self):
          while self.task_queue:
              task = self.task_queue.popleft()
              try:
                  task.send(None)
                  self.task_queue.append(task) # Re-queue for next cycle
              except StopIteration:
                  pass # Task completed
  ```
