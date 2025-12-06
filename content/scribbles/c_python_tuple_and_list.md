---
title: Memory management and Internals of List and Tuples in 'C-python'
draft: false
tags: [dev,python]
date: 2025-12-06
---
## Memory concepts:

Everything in python is an object and python is dynamically typed language.

When x = 20. an integer object 20 is created and is referenced to variable x. If x is incremented, the another object is created of integer class and is then referenced to variable x.

For memory optimization, **python allocated an object reference to a new variable if object exists with same value,** say z here if z = 20, just as x, then id of z and x are equal since both refer the same integer object 20

**In python, any reference variable may later be assigned to an object of different class. Hence python is a dynamically typed language.**

### Stack memory and Heap memory

 OS allocates memory for python process, its exact size is depend upon the version, type etc...

 Python has two types of memory allocated to it. It is stack memory and heap memory.

In stack memory, all objects are referenced in stack memory, ie reference variables are located in stack memory, furthermore, methods are invoked in stack memory, method, by the order of their invocation, are stacked up. When a method finishes its execution , after returning values if any, the method's stack frame will be destroyed automatically.

Just as methods, references are also destroyed automatically.

Interpreter keeps track of all the objects and the number of references to those objects in a table. Whenever the no of references of an object becomes zero, we call the object as dead object, and is removes by a process called Garbage collection, it uses a algorithm called 'Reference counting'.

When frequent invoke of reference algorithm will result in performance reduction, but it can keep the memory optimal.

// garbage collector doesn't keep track of weak references

```python
obj_name = weakref.ref(object_name_to_be_ref)
```

### Data structures : List and Tuple

#### List Characteristics

- Mutable // add or remove elements from the list

Here we have different operations like append and pop also insert

```python
eg = [1,2,3,4]
print(eg)
eg.append(5) # appending an element into eg
print(eg)
eg.pop()     # removing the last element
print(eg)
eg.pop(1)    # removing the element at index 1
print(eg)
eg.insert(2,10) # inserting element 10 in 2 nd index
print(eg)
```

- Extendable // can extend an existing list by adding another list to it

```python
eg.extend([5,6,7,8,9,2])
print(eg)
eg2 = ['eleven','twelve','thirteen']
eg.extend(eg2)
print(eg)
```

- Sort-able // the elements inside a list can be sorted in ascending or descending fashion

```python
del eg[16:18]
del eg[22:24]
eg.sort()
print(eg)
```

Memory allocation of list:

mylist = ['a', 'b', 'c, 'd']  
This creates a array of pointers in memory. These pointers will point to the address of elt a, b , c and d of mylist.

Logic used for expansion :

```c
  new_allocated = (size_t)newsize + (newsize >> 3) + (newsize < 9 ? 3 : 6);
```

As the size gets bigger , we have approx of 12 percent increase.

A really optimal way is pre allocated list, python already amortize the cost. 

### inefficient methods :  
pop with no index is fine, since last element is the one that is removed, but any other index , worst case elt at 0th index makes the array of pointers shift itself down to one level, which is not really fine, worst case is O(N) case. 
Similarly , insert() operation is also inefficient

### Operations :

- Append - O(1)
- Extend - O(k)
- Pop last element - O(1)
- Pop anywhere else - O(n) // worst case
- Insert - O(n) // worst case
- Index - O()1
- Slice - O(k)
- in operator - O(n)
- Sort  - O(nlogn)

---

## Tuple characteristics

- Immutable // no insertion and deletion

```python
eg = (1,2,3,4,5)
print(eg)
eg[0] = 7
```

The size is fixed in size, and it also creates an array of pointers. Later no change is possible. 

For reducing Memory fragmentation, python introduced the approach , reusing tuples.

**Memory id of the previously cleared tuple of the same size is used for a new tuple.**

```python
eg = (1,2,3,4)
print(id(eg))
eg = 1 # clearing the tuple in eg
eg2 = (4,7,8,3)
print(id(eg2))
```

since tuples are not re-sizable, they are not over-allocated. A tuple is more efficient if you want to return a collection of values from a function.

The implementation of tuple is over thousand lines of code, whereas for list, it is over three thousand lines of code.

### Operations available :

- Index  - O(1)
- Slice  - O(k)
- in operator - O(n) // worst case, no searching item in tuple
