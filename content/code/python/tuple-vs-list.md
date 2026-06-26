---
title: Memory management and Internals of List and Tuples in CPython
draft: false
tags: [python, reference]
date: 2025-12-06
---

How CPython stores Python objects in memory, and what that means for choosing between lists and tuples in your code.

## Memory concepts

Everything in Python is an object, and Python is a dynamically typed language. When you write `x = 20`:

1. An integer object `20` is created.
2. The variable `x` is bound to that object.

If `x` is later incremented, a new integer object is created and `x` is rebound to it. The original object is left alone.

**CPython reuses objects when their value already exists.** If you assign `z = 20` after `x = 20`, both `x` and `z` reference the same integer object — `id(x) == id(z)`.

>[!info] Why this matters for dynamic typing
>
> A reference variable can be reassigned to an object of any class at any time. That's why Python is dynamically typed — the variable carries no type info, only a pointer to whatever object it currently points at.

### Stack and heap

The OS allocates memory for the Python process (exact size depends on the version and build). Inside that, CPython uses two regions:

- **Stack** — references live here. When a method is invoked, a frame is pushed onto the stack; when it returns, the frame is popped. References are also destroyed automatically when their owning frame goes away.
- **Heap** — actual objects live here.

CPython keeps a table that tracks every object and the number of references to it. When an object's reference count hits zero, it's a "dead object" and is freed by the **garbage collector**, which uses a **reference counting** algorithm.

>[!note] Performance trade-off
>
> Constant reference-count updates keep memory tight but introduce a small per-assignment cost. For most workloads this is fine; in tight inner loops it can matter.

The garbage collector does **not** track weak references — for those, use the `weakref` module:

```python
import weakref
obj_name = weakref.ref(object_name_to_be_ref)
```

## Lists

A list is a mutable, dynamically-resizable array of pointers to Python objects.

### Mutability

```python
eg = [1, 2, 3, 4]
print(eg)

eg.append(5)              # add to the end
print(eg)

eg.pop()                  # remove the last element
print(eg)

eg.pop(1)                 # remove the element at index 1
print(eg)

eg.insert(2, 10)          # insert 10 at index 2
print(eg)
```

### Extendable

```python
eg.extend([5, 6, 7, 8, 9, 2])
print(eg)

eg2 = ['eleven', 'twelve', 'thirteen']
eg.extend(eg2)
print(eg)
```

### Sortable

```python
del eg[16:18]
del eg[22:24]
eg.sort()
print(eg)
```

### Memory layout

For a list like `mylist = ['a', 'b', 'c', 'd']`, CPython allocates an **array of pointers** in memory. Each pointer references the actual `str` object for `'a'`, `'b'`, `'c'`, `'d'` — the strings themselves are separate heap objects.

When the list grows beyond its current allocation, CPython uses this over-allocation rule (from CPython's C source):

```c
new_allocated = (size_t)newsize + (newsize >> 3) + (newsize < 9 ? 3 : 6);
```

That's roughly a **12% growth factor** as size increases. CPython also amortises the cost — the over-allocation means appends usually don't trigger reallocation.

### Costly operations

- `pop()` with no argument — **O(1)**, removes the last element.
- `pop(i)` for any other index — **O(N)** worst case, because every element after `i` has to shift down.
- `insert(i, x)` — **O(N)** worst case, same reason.

If you'll be doing a lot of front-of-list insertions or removals, use `collections.deque` instead.

### Complexity

| Operation | Cost |
|-----------|------|
| Append | O(1) |
| Extend by `k` items | O(k) |
| Pop last element | O(1) |
| Pop arbitrary index | O(N) |
| Insert | O(N) |
| Index (`lst[i]`) | O(1) |
| Slice | O(k) |
| `in` operator | O(N) |
| Sort | O(N log N) |

## Tuples

A tuple is an **immutable, fixed-size** array of pointers. Once created, it cannot grow, shrink, or change elements.

### Immutability

```python
eg = (1, 2, 3, 4, 5)
print(eg)

eg[0] = 7   # TypeError: 'tuple' object does not support item assignment
```

### Memory reuse

Because tuples can't be resized, CPython doesn't over-allocate them. To reduce memory fragmentation, CPython **reuses the memory of a previously-cleared tuple of the same size** for a new tuple:

```python
eg = (1, 2, 3, 4)
print(id(eg))   # first allocation

eg = 1          # original tuple's refcount drops to 0; its memory is freed

eg2 = (4, 7, 8, 3)
print(id(eg2))  # may reuse the same memory as the freed tuple
```

Because tuples are not resized, they're more efficient than lists when you need to return a collection of values from a function — the function can return a tuple by reference, with no allocation overhead per element.

The CPython tuple implementation is also simpler: about **1,000 lines of C** versus the list implementation's **3,000+ lines**.

### Complexity

| Operation | Cost |
|-----------|------|
| Index | O(1) |
| Slice | O(k) |
| `in` operator | O(N) |

## List vs Tuple — at a glance

| Property | List | Tuple |
|----------|------|-------|
| Mutability | Mutable | Immutable |
| Resizable | Yes | No, fixed at creation |
| Over-allocated | Yes (~12% growth) | No |
| Memory reuse | No | Yes (same-size tuples share freed memory) |
| Best for | Collections that grow/shrink | Fixed collections, function returns, dict keys |
| CPython source size | ~3,000 lines C | ~1,000 lines C |

Reach for a list when the collection will change. Reach for a tuple when the collection is fixed and you want the size, allocation, and immutability guarantees it gives you.