# Why I Love Python
A balanced, objective, and unbiased view on why Python (Python 3, that is) is the best programming language. Ever.

## What was Cool When I Was First Learning Python
- I came from writing code in Java and C++ and someone told me I should try out Python. 
- Note that a lot of these things that are available in a lot of modern langauges (like Kotlin, Javascript, etc).
- Coding in Python feels like I'm cheating.
- I haven't yet gone back to Java or C++ since. Here were some of the things I loved.
- If you know how to code and want a place to start, check out the Python Tutorial: https://docs.python.org/3/tutorial/index.html.

### 1. So Simple To Write
- Great levels of abstraction
         - If I want to print something, I don't have to think about input and output
Java:
```java
import static java.lang.System.out;
System.out.println("Hello World")
```

Python:
```python
print("Hello World")
```

### 2. There's a lot that comes right out of the box.
- For example, everything you could imagine you would want to do with a String (`str`), is built in
        - https://docs.python.org/3/library/stdtypes.html#string-methods
        - `isalpha`, `beginswith`, `endswith`, `isupper`, `upper`, `split`
 ```python
 >>> '1,2,3'.split(',')
['1', '2', '3']
```

### 3. `dict`s. are. bae.
- Soooo simple to use. Especially compared to Java
-  Incredibly useful and lightweight
- Super fast and optimized
       - Check out [modern `dict`s with Raymond Hettinger](https://www.youtube.com/watch?v=p33CVV29OG8). Their implementation is pretty interesting

### 4. List Comprehensions and Dictionary Comprehensions are the coolest
```python
integer_magnitudes = dict()
for x in range(10):
    for y in range(10):
        integer_magnitudes[(x,y)] = x**2 + y**2
```
vs. 

```python
integer_magnitudes = {(x,y) : x**2 + y**2 for x in range(3) for y in range(3)}
```

### 5. Keyword Arguments
- I totally forgot that Java has a really messy way to handle this: method overloading
- So intuitive and useful

## What I've Come to Love About Python
As I've delved further into the language, here's what I've come to appreciate.
### 1. Python is Everywhere
- Python is the language of choice in many domains. Therefore, if I want a module/package that does X, there's a module for that
      - [UCLA Rocket Project in Python](https://github.com/UCLA-Rocket-Project/ares-controls)

### 2. Iterators Everywhere
- An iterator is an object that allows you to traverse a data structure
- In Python, any class that implements the `__iter__` function is an iterator
- The cool part: Uncomputed iterators
    - `range(1000000000000000)`
         - In Python 2, this would lead to an `OverflowError`
         - In Python 3, all is right in the universe.
    - `map` creates an iterator
         - `x = map(lambda y: y**2 + 3, range(1000))`
- Learn more [here](https://docs.python.org/3/tutorial/classes.html#iterators)
    - As a bonus, keep on reading into the Generators section.

### 3. The Functional Taste 
- Python is not a functional programming language, however you can write Python in a functional style.
- This often helps with readability

### 4. Fantastic Data Analysis Libraries
- `numpy` makes Python "as fast as" C for any matrix operations
- `pandas` is great for dealing with things that look like spreadsheets
- `matplotlib` is great for graphing
- Python is full of active libraries for whatever you want to work on

## Where I Think Python is Lacking and Can Improve
### 1. No Type System
- It is often hard to read or keep track of untyped code.
      - If you don't know what type a variable is, it is harder to reason about code.
- What does this function do?
```python
(tsi,
temp_cohi,
ts_stdi,
ifg_numi) = ifgram_inversion_patch(ifgram_file,
                     box=box,
                     ref_phase=ref_phase,
                     unwDatasetName=inps.unwDatasetName,
                     weight_func=inps.weightFunc,
                     min_norm_velocity=inps.minNormVelocity,
                     mask_dataset_name=inps.maskDataset,
                     mask_threshold=inps.maskThreshold,
                     min_redundancy=inps.minRedundancy,
                     water_mask_file=inps.waterMaskFile,
                     skip_zero_phase=inps.skip_zero_phase)
```

- Python is working on this. There are now optional Type Annotations you can give to code.
     - PyCharm will even make suggestions if you are trying to pass a `str` to a function that takes `int` as an input.
     - I think this is a good tradeoff: you don't need to annotate types and you can still change variable types on the fly, however you have the option to be clear about a variable's type
- [Type Annotations in Python 3](https://docs.python.org/3/library/typing.html)

Example:
```python
def function_name(parameter: List[int]) -> output_type:
```


### 2. Not Great for Mobile Development

### 3. Most Errors Occur At Runtime
- It's a lot easier to get a program to compile, however you spend a lot more time debugging a running program.
     - The classic static/dynamic programming language tradeoff
     
### 4. Strings Can Be Weird
- Unicode hell, anyone?
     
## Final Thoughts
1. If you're just starting off, learn Python and another language with it.
     - It'll give you a more complete view of what a programming language is and the tradeoffs between languages
     - Coding in Python some
2. Learn some basic functional programming concepts. It'll make you a better programmer
     - E.g. Write code as if variables are immutable. 
     - Avoid side effects in functions
