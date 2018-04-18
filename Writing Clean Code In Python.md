# Writing Clean Code in Python
Inefficient software isn't gross. What's gross is a language that makes programmers do needless work. Wasting programmer time is the true inefficiency, not wasting machine time.
-Paul Graham

---

## What is Clean Code?
Clean code is
1. **Functional** 
    - Completes the task at hand
    - Works in a reasonably "normal" world where "normal" is defined by the use case
2. **Readable**
    - The code is easily understandable by the original developer
    - The code is easily understood by the current and future team
3. **Reusable**
    - Code written in one part of the program can be reused in other parts of the program
    - Work done today will benefit the team in the future


---

## Goals for Today
1. Writing Clean, Pythonic Code
2. Idiomatic Python
3. Structuring Projects

---

## Writing Clean Code 

### Design Rules

##### 1. Follow standard coding conventions
- Especially conventions laid out by the language
- Add on common patterns to standard pattern to a group-specific style guide
- [Google Style Guide](https://google.github.io/styleguide/pyguide.html)
- [PEP 8 - The official Python Style Guide](https://www.python.org/dev/peps/pep-0008/)
- Use Linters
    - `pylint` and `pycodestyle`
    - [Python Linters In Practice](https://jeffknupp.com/blog/2016/12/09/how-python-linters-will-save-your-large-python-project/)

```python
# Nope
f = lambda x: 2*x

# Yup
def double(x): return 2*x
```

##### 2. KISS - Keep It Simple Stupid
- Understanding your code's logic shouldn't be challenging.
- In any language, there are cool features. Don't use them.
- Use Case: [List Comprehensions](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions)

```python
# GOOD!
squares = [x * x for x in range(10)]

# Not so easy to understand...
result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

# Better
result = []
  for x in range(10):
      for y in range(5):
          if x * y > 10:
              result.append((x, y))
```

##### 3. Boy scout rule
- Keep the campground cleaner than you found it.
- Always think about Future You and Future Team
- Make it really easy on your future self to implement new features, reason about the codebase, etc.
- This means writing code that is reusable eliminating waste

---

### Naming Best Practices
1. Choose descriptive and unambiguous names.
    - `variable_name`
    - `function_name`
    - `ClassName`
2. Make meaningful distinction.
3. Use pronounceable names.
4. Use searchable names.
5. Replace magic numbers with named constants.

---

### Writing Functions
1. Optimize for reusability
    - If you think you'll reuse something, make it into a function
1. Small and do one thing.
1. Use descriptive names and arguments
    - We will revisit you in a second
1. Prefer fewer arguments.
1. Have no side effects.
    - We will revisit you in a second


```python
# Not so great
def ps(upper):
    if upper == None:
        upper = float('inf')

    ps = [2]
    curr = ps[-1]
    while curr < upper:
        if any(map(lambda p: curr % p == 0, ps)):
            ps.append(curr)
            yield curr
        curr += 1

# Better
def primes(upper_limit = None) -> Generator[int, None, None]:
    """
    A generator that yields prime numbers starting with 2
    :upper_limit: Optional parameter for a maximum prime number to yield
    :return: Generator[int, None, None]
    """

    if not upper_limit:
        upper_limit = float('inf')

    primes = []
    current = 2
    while current < upper_limit:
        # Lazily evaluates divisibility
        divisible_by_primes = map(lambda prime: current % prime == 0, primes)
        if not any(divisible_by_primes):
            primes.append(current)
            yield current
        current += 1
```

---

### Side Effects Revisited

1. A Side Effect is a change some sort of state
1. For functions, this is a change of state that isn't part of the `return` value
1. Examples of changing state
    - Changing the value of a global variable
    - Changing the data of an input argument (appending to an input list)
    - Writing to stdout (`print`)
1. It becomes easier to reason about code when we write functions that are Side Effect-free

```python
# Side Effect Free!
def sum_of_list1(values: List[int]) -> int:
    total = sum(values)
    return total

# Side Effects Alert!
def sum_of_list2(values: List[int]) -> int:
    total = sum(values)
    values.append(total)    # I'm not Pure!
    return total

# Side Effects Alert!
def sum_of_list3(values: List[int]) -> int:
    total = sum(values)
    print("Sum:", total)
    return total
```

---

### Function Arguments

1. Positional Arguments
    - Required to call a function
2. Keyword Arguments
    - Optional arguments for a function
3. Positional Arguments can be called using keywords
    - This can make your code more readable

```python
# A non-descript function call
twitter_search('@obama', False, 20, True)

# So much easier to understand
twitter_search('@obama', retweets=False, numtweets=20, popular=True)
```

#### A note on keyword arguments: Mutable Default Arguments
1. Keyword arguments are evaluated once. This can lead to some weird effects
2. To get around some weird effects, use a default parameter of `None` rather than a data structure

```python
# Buggy!
def append_to(element, to=[]):
    to.append(element)
    return to

>>> my_list = append_to(12)
[12]

>>> my_other_list = append_to(42)
[12, 42]

def append_to(element, to=None):
    if to is None:
        to = []
    to.append(element)
    return to


# Note: What's wrong about this example in general?
```


---
---

## Idiomatic Python - Writing Python Code

```python
>>> import this
```
```
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Readability counts.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
```

### Iteration Everywhere!

##### 1. Iterating over indices
```python
colors = ['red', 'green', 'blue', 'yellow']

# Bad 
for i in range(len(colors)):
    print i, '--->', colors[i]

# Better
for i, color in enumerate(colors):
    print i, '--->', color
```

##### 2. Iterating over Multiple Collections
```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue', 'yellow']

# Bad
n = min(len(names), len(colors))
for i in range(n):
    print names[i], '--->', colors[i]

# Better
for name, color in zip(names, colors):
    print name, '--->', color
```

##### 3. Counting and Grouping with Dictionaries
```python
# Not so elegant
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

# Simple, basic way to count. A good start for beginners.
d = {}
for color in colors:
    if color not in d:
        d[color] = 0
    d[color] += 1

# {'blue': 1, 'green': 2, 'red': 3}


# Better
d = {}
for color in colors:
    d[color] = d.get(color, 0) + 1

# Best
from collections import defaultdict
d = defaultdict(int)
for color in colors:
d[color] += 1
```
.

```python
names = ['raymond', 'rachel', 'matthew', 'roger',
         'betty', 'melissa', 'judith', 'charlie']

# In this example, we're grouping by name length

# Meh
d = {}
for name in names:
    key = len(name)
    if key not in d:
        d[key] = []
    d[key].append(name)

# {5: ['roger', 'betty'], 6: ['rachel', 'judith'], 7: ['raymond', 'matthew', 'melissa', 'charlie']}

# Better
d = {}
for name in names:
    key = len(name)
    d.setdefault(key, []).append(name)

# Best
d = defaultdict(list)
for name in names:
    key = len(name)
    d[key].append(name)
```

##### 4. On A Deeper Level, Python 3.6 is Iterators
1. Delays computation until you a values is needed
2. `map`, `filter`, and all of `itertools` are powerful for writing clean and efficient code

### Tuple Packing and Unpacking
1. You can pack and unpack tuples with commas
2. We have already been doing that when dealing with `enumerate` and `zip`

```python
# Bad 
def fibonacci(n):
    x = 0
    y = 1
    for i in range(n):
        print x
        t = y
        y = x + y
        x = t

# Better
def fibonacci(n):
    x, y = 0, 1
    for i in range(n):
        print x
        x, y = y, x + y
```

3. Nested unpacking works too and more elegant unpacking
```python
a, (b, c) = 1, (2, 3)

a, *rest = [1, 2, 3]
# a = 1, rest = [2, 3]
a, *middle, c = [1, 2, 3, 4]
# a = 1, middle = [2, 3], c = 4
```

4. Ignoring arguments too
```python
fname, __, lname = ("David", "Welin", "Grossman")
```

---

## Errata

### On Modular Code
1. When writing Python modules, all code should be in a function
1. Use `if __name__ == "__main__"` to allow the script to run as a standalone program too

```python
from that import this
import this_other_thing

def function1(arg1, arg2):
    return arg1 * arg2

# ...

if __name__ == "__main__":
    # Run code when the script is executed as a standalone function
    pass
``

### Typing
1. Python doesn't have declared types. However, you can optionally use types
1. Types can make the code more readable and can catch errors before execution

```python
from typing import List

def greeting(name: str) -> str:
    return 'Hello ' + name

Vector = List[float]

def scale(scalar: float, vector: Vector) -> Vector:
    return [scalar * num for num in vector]

```


---

## The Important Points to Take Away
1. Use linters
    - They will create a syntax and coding standard
    - They will pick up bugs before runtime
1. Optimize for Readability and Reusability
    - Eat your vegetables today so that tomorrow you will be fit and strong. And have a fantastic code base!!
1. Modular code
    - Rewrite everything into functions
    - Break problems down into their fundamental steps and work up from there

---

## Next Time
1. Python Code Base Structure
2. Logging Events in Code
3. Good software practices for working in a team
4. A closer look at some RSMAS code
