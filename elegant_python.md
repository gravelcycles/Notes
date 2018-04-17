# Writing Clean Code in Python
Inefficient software isn't gross. What's gross is a language that makes programmers do needless work. Wasting programmer time is the true inefficiency, not wasting machine time.
- Paul Graham

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
1. Coding best practices for writing clean code in any language
2. Writing Pythonic Code
3. Structuring Projects
4. Python Tooling for writing clean code

---

## Writing Clean Code (Language Agnostic)

### Design Rules

##### 1. Follow standard coding conventions
- Especially conventions laid out by the language
- Add on common patterns to standard pattern to a group-specific style guide
- [Google Style Guide](https://google.github.io/styleguide/pyguide.html)
- [PEP 8 - The official Python Style Guid](https://www.python.org/dev/peps/pep-0008/)

##### 2. KISS - Keep It Simple Stupid
- Understanding your code's logic shouldn't be challenging.
- In any language, there are cool features. Don't use them.

##### 3. Boy scout rule
- Keep the campground cleaner than you found it.
- Always think about Future You and Future Team
- Make it really easy on your future self to implement new features, reason about the codebase, etc.


## TODO
1. Side effects
2. Keep Configurable Data at High Levels
2. 

zip and enumerate, defaultdict, tuple packing and unpacking, decorators, list comprehensions

`import this`

Follow standard conventions.
Keep it simple stupid. Simpler is always better. Reduce complexity as much as possible.
Boy scout rule. Leave the campground cleaner than you found it.
Always find root cause. Always look for the root cause of a problem.


## Idiomatic Code
1. Modular Code
2. Reuse Code
3. Functions, functions, functions!
    - If you think you'll reuse something, make it into a function
    - If one function is getting to large, make it into a function
4. 

## Idiomatic Python
1. Variable names should be descriptive
    - `variable_name`
    - `function_name`
    - `ClassName`
2. Function Arguments
    - Positional arguments 
    - Keyword arguments
3. The layout of a Pythonic module
    - Comments on module
    - import statements
    - functions
    - `if __name__ == "__main__"`
    - No global variables if possible
4. Packing and Unpacking: Tuples in Python
5. Iterators Galore!
6. Check if you're following standards by using linters
