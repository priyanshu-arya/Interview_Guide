# 📚 Recommended Python Learning & Interview Resources

A curated list of high-quality learning resources, documentation, technical books, interactive prep platforms, and repository guides for mastering Python.

---

## 📌 1. Official Documentation & Key PEP Specifications

- **[Python Official Documentation (docs.python.org)](https://docs.python.org/3/)**:
  - *Why it's worth using*: Authoritative reference for the CPython standard library, language reference, and dynamic data model protocols.
  - *Key Sections to Read*: The Python Language Reference (Data Model section), Standard Library (`asyncio`, `itertools`, `functools`, `collections`, `tracemalloc`).
- **[PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)**:
  - *Why it's worth using*: Standard code formatting guidelines expected in all professional technical interviews.
- **[PEP 484 – Type Hints](https://peps.python.org/pep-0484/)**:
  - *Why it's worth using*: Establishes formal specification for Python's static type annotation system.
- **[PEP 3156 – Asynchronous IO Support (asyncio)](https://peps.python.org/pep-3156/)**:
  - *Why it's worth using*: Architectural blueprint behind Python's event loop and coroutine design.
- **[PEP 557 – Data Classes](https://peps.python.org/pep-0557/)**:
  - *Why it's worth using*: Clean, boilerplate-free data object container patterns.

---

## 📖 2. Recommended Books

1. **"Fluent Python: Clear, Concise, and Effective Programming" by Luciano Ramalho**:
   - *Target Tier*: Mid-Level to Staff+ Engineers.
   - *Why read it*: The undisputed gold standard for understanding Python's data model, dunder protocols, generators, metaclasses, descriptors, and modern concurrency.
2. **"Python Cookbook" by David Beazley & Brian K. Jones**:
   - *Target Tier*: Intermediate to Senior SDE.
   - *Why read it*: Packed with practical code recipes covering data structures, generators, metaprogramming, C-extensions, and network programming.
3. **"Effective Python: 90 Specific Ways to Write Better Python" by Brett Slatkin**:
   - *Target Tier*: SDE-1 to SDE-2.
   - *Why read it*: Concise rule-based advice on writing idiomatic, pythonic, robust, and maintainable production code.
4. **"High Performance Python" by Micha Gorelick & Ian Ozsvald**:
   - *Target Tier*: Senior to Staff Architect / Data Engineers.
   - *Why read it*: Deep dive into memory allocation, CPU vectorization (NumPy), Cython, multi-core multiprocessing, and profiling tools.

---

## 🎥 3. Top YouTube Channels & Video Lectures

- **[ArjanCodes](https://www.youtube.com/@ArjanCodes)**: Software design patterns, clean architecture, refactoring, and type hinting in modern Python.
- **[Raymond Hettinger PyCon Talks](https://www.youtube.com/results?search_query=raymond+hettinger+python)**: Deep insights into Python internals, dictionary mechanics (`Transforming Code into Pythonic Code`), and iterator design by a CPython core developer.
- **[James Powell Metaprogramming Talks](https://www.youtube.com/results?search_query=james+powell+python+expert)**: Masterclass explanations of descriptors, metaclasses, decorators, and internal execution machinery.
- **[mCoding by James Murphy](https://www.youtube.com/@mcoding)**: Advanced technical explanations of GIL internals, C-extensions, type system nuances, and common bug traps.

---

## 🧪 4. Practice Websites & GitHub Repositories

- **[Exercism (Python Track)](https://exercism.org/tracks/python)**: Free, mentor-reviewed coding exercises emphasizing pythonic solutions and clean idioms.
- **[LeetCode (Python 3 Filtered)](https://leetcode.com/)**: Essential for practicing algorithm problems, data structures, and optimizing time/space complexity using Python built-in modules (`heapq`, `collections.deque`, `bisect`).
- **[Real Python (realpython.com)](https://realpython.com/)**: In-depth tutorials explaining both surface syntax and underlying implementation mechanics.
- **[Python Morsels](https://www.pythonmorsels.com/)**: Weekly pythonic coding exercises focused on core language mastery and iterator techniques.

---

## 💡 5. Recommended Study Plan

| Stage | Resource Focus | Weekly Goal |
|-------|----------------|-------------|
| **Week 1** | *Fluent Python* (Ch 1-4) + PEP 8 | Master Data Model, Mutability, Sequences, Dict internals. |
| **Week 2** | *Effective Python* + Exercism | Master Decorators, Generators, Context Managers, Type hints. |
| **Week 3** | Raymond Hettinger Talks + *Fluent Python* (Concurrency) | Master GIL, `threading`, `multiprocessing`, `asyncio` event loops. |
| **Week 4** | *High Performance Python* + `Top_Questions.md` | Master CPython GC, `tracemalloc`, memory layout, descriptors, metaclasses. |
