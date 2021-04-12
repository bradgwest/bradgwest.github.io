---
title: "Python Lazy Iteration"
---

A few days ago I needed to lazily iterate over lines in a file. I quickly
realized I didn't know what exactly makes iteration lazy in python. An example
of what I was trying to do:

```py
import csv

def process_csv_lazily(path, handle):
    with open(path) as f:
        reader = csv.reader(f)
        for row in reader:
            handle(row)
```

It wasn't properly formulated at the time, but there's a very specific question
I should have asked to determine if `process_csv_lazily` loads rows lazily:

> Does the value returned by `csv.reader` implement the `Iterator` protocol?

Python's glossary clearly defines the
[`iterator`](https://docs.python.org/3/glossary.html#term-iterator) protocol:

> An object representing a stream of data. Repeated calls to the iterator’s
  `__next__()` method (or passing it to the built-in function `next()`) return
  successive items in the stream.

To answer _"is it lazy?"_ we simply need to determine if the
reader object returned by `csv.reader` implements `__next__` and `__iter__`.

It does:

```py
def is_iterator(x):
    return hasattr(x, "__next__") and hasattr(x, "__iter__")

obj = csv.reader(io.StringIO("1,2\n3,4"))
print(is_iterator(obj))
# True
```

There's an easier way to do this though, using the `Iterator` abstract base
class and the `isinstance` builtin.

```py
from collections.abc import Iterator

obj = csv.reader(io.StringIO("1,2\n3,4"))
print(isinstance(x, Iterator))
# True
```

It takes a few method calls to get there (`__instancecheck__` and
`__subclasscheck__` on the `ABCMeta` class, and `__subclasshook__` on the
`Iterator` class), but eventually that call checks whether the object returned
by `csv.reader` has the `__next__` and `__iter__` attributes. The standard lib
code iterates over the Method Resolution Order list[^1], but the logic is the
same as our `hasattr` check above.

[^1]: `hasattr` is "clumsy or subtly wrong" for checking interface
      compatibility. ABCs exist, in part, to circumvent this. See the
      [glossary](https://docs.python.org/3/glossary.html#term-abstract-base-class).


To get a better idea of Python's interfaces, see the `collections.abc`
[module](https://docs.python.org/3/library/collections.abc.html#collections-abstract-base-classes),
which defines ABCs for testing whether a class implements an interface.

`Generator` is an interesting ABC which inherits from `Iterator`.
[generators](https://docs.python.org/3/glossary.html#term-generator) return
[generator iterators](https://docs.python.org/3/glossary.html#term-generator-iterator)
which implement the iterator protocol. Generators are a higher level abstraction
than Iterators, and as such provide a number of additional conveniences. For
example, they don't raise `StopIteration` when they're exhausted, and you can
create them with generator expressions. So, in a lot of cases, if you need an
iterator, create a generator function and call it to return a generator iterator.
