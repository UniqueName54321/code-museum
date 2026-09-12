# Python

> **Collection:** `languages/`  
> **Kind:** General-purpose programming language  
> **Paradigms:** Multi-paradigm: procedural, object-oriented, functional-ish when it feels like it  
> **First released:** 1991  
> **Created by:** Guido van Rossum  
> **Canonical implementation:** CPython  
> **File extension:** `.py`  
> **Museum target:** Python 3.14  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)

Python is a general-purpose programming language built around a dangerous proposition:

> What if source code were allowed to look reasonably pleasant?

It is famous for readable syntax, indentation-based blocks, a large standard library, dynamic typing, automatic memory management, and being used for approximately everything from tiny automation scripts to web servers, data science, machine learning, education, scientific computing and miscellaneous scripts named `final_final_really_final.py`.

Python is one of the cleanest first stops after the museum's pseudocode exhibit because the two already resemble each other.

That resemblance is not an accident. Pseudocode often borrows the visual vocabulary of high-level languages, and modern pseudocode frequently ends up looking Python-shaped even when nobody explicitly intended it to.

---

## Current status

As of **12 September 2026**, the latest stable Python 3 release is **Python 3.14.7**.

Python 3.15 is still in release-candidate status at the time this page was written, with the final 3.15.0 release scheduled for October 2026.

This exhibit therefore targets **Python 3.14** rather than documenting an unreleased language version.

Python 2 also existed and was enormously important historically, but it reached end-of-life in 2020. Unless an exhibit is specifically discussing history, "Python" here means **Python 3**.

---

## A tiny bit of history

Guido van Rossum began work on Python at CWI in the Netherlands around the end of the 1980s, and the language's first public release appeared in 1991.

The name came from **Monty Python**, not the snake, although several decades of logos and book covers have ensured that the snake won the branding war anyway.

Python grew into a major scripting and application language during the 1990s and 2000s. Python 2.0 arrived in 2000.

Python 3.0 arrived in 2008 and deliberately broke compatibility with Python 2 in order to clean up parts of the language that had become difficult to fix while preserving old behaviour. The transition took years and became one of the language's most famous historical headaches.

Today Python is developed as an open-source project, with language changes commonly discussed through **PEPs**, or Python Enhancement Proposals.

---

## First impressions

Here is some Python:

```python
name = "Mika"
age = 22

if age >= 18:
    print(f"{name} passes the age check.")
else:
    print("No :(")
```

A few characteristics are immediately visible.

### Blocks are indentation

Python does not write:

```text
IF condition THEN
    ...
ENDIF
```

or:

```c
if (condition) {
    ...
}
```

It writes:

```python
if condition:
    ...
```

The indentation is not decoration. It is syntax.

This:

```python
if age >= 18:
    print("yes")
```

is different from incorrectly indenting the body.

For programmers coming from brace-heavy languages, Python's position is essentially:

> You were indenting your code anyway. I have decided to make that legally binding.

### Variables do not need declarations

You can simply write:

```python
age = 18
name = "Ren"
```

Python is **dynamically typed**. Values have types, and variables are names referring to objects, but you normally do not declare a variable's type before assigning to it.

Python also supports optional type annotations:

```python
age: int = 18
name: str = "Ren"
```

These annotations are useful to humans, editors and static type checkers, but ordinary Python execution does not generally enforce them in the same way that C++ enforces static types.

### Semicolons are almost never needed

A normal Python statement ends at the newline.

```python
print("hello")
print("world")
```

You *can* cram multiple simple statements onto one line with semicolons.

Please do not make the museum call security.

---

# The museum program

Here is the canonical Language Museum specimen translated into ordinary Python 3.

```python
from dataclasses import dataclass

LANGUAGE = "Python"
MAX_PASSENGERS = 100


@dataclass
class Person:
    name: str
    age: int


def greet(name: str) -> None:
    print(f"Hi {name}!")
    print(f"I hope you are well, {name}")
    print(f"Nice to meet you, {name}")


def age_check(age: int) -> str:
    if age >= 18:
        return "You may legally drink alcohol here."
    else:
        return "No :("


def factorial(num: int) -> int:
    if num <= 1:
        return 1

    return num * factorial(num - 1)


people: list[Person] = []

num_passengers = int(
    input(
        f"How many people are you going into the bar with? "
        f"(maximum {MAX_PASSENGERS}) "
    )
)

for _ in range(num_passengers):
    name = input("What is your name? ")

    greet(name)

    age = int(input("How old are you (in human years)? "))

    print(age_check(age))

    people.append(Person(name, age))

print("Fun fact! For everyone here, their ages in factorial would be:")

for person in people:
    print(f"- {person.name} is {factorial(person.age)}")

print(f"Interactive museum entry for {LANGUAGE} completed.")
```

Run it with:

```sh
python museum.py
```

Depending on your operating system and installation, the command may instead be:

```sh
python3 museum.py
```

---

# What changed from the pseudocode?

Quite a lot of the reference program maps almost directly to Python.

## Constants are conventional rather than enforced

The pseudocode declares:

```text
CONSTANT Language = "Pseudocode"
```

Python has no ordinary `const` declaration for module variables.

The convention is to use an uppercase name:

```python
LANGUAGE = "Python"
MAX_PASSENGERS = 100
```

Nothing stops later code from doing:

```python
LANGUAGE = "COBOL wearing a Python hat"
```

The uppercase spelling tells programmers "please treat this as a constant".

Python itself responds with a relaxed shrug.

---

## `Person` becomes a dataclass

The pseudocode has:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE
```

Python has several ways to represent that information.

The exhibit uses a **dataclass**:

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
```

The `@dataclass` decorator tells Python to generate useful boilerplate for a simple data-holding class, including an initializer.

That lets us create a person with:

```python
Person("Mika", 22)
```

We could also have used dictionaries:

```python
{"name": "Mika", "age": 22}
```

or tuples, named tuples, ordinary classes, or several other representations.

The dataclass is a nice conceptual match for the pseudocode record.

---

## Procedures and functions collapse into one construct

Cambridge pseudocode distinguishes:

```text
PROCEDURE Greet(...)
```

from:

```text
FUNCTION Factorial(...) RETURNS INTEGER
```

Python uses `def` for both:

```python
def greet(name: str) -> None:
    ...
```

and:

```python
def factorial(num: int) -> int:
    ...
```

A Python function that reaches the end without an explicit `return` returns the special value `None`.

So `greet()` is still a function in Python terminology even though the pseudocode treats it as a procedure.

---

## Strings use f-strings

The pseudocode can output several pieces:

```text
OUTPUT "Hi ", Name, "!"
```

In Python we use an **f-string**:

```python
print(f"Hi {name}!")
```

An `f` before the opening quote allows expressions inside `{...}`:

```python
score = 42
print(f"The score is {score}.")
```

This is called a formatted string literal.

---

## Input is text until you convert it

Python's `input()` reads a line from the user and returns a string:

```python
name = input("What is your name? ")
```

For an integer:

```python
age = int(input("How old are you? "))
```

the operations happen inside-out:

1. `input(...)` returns text;
2. `int(...)` converts that text to an integer.

If the user enters something that cannot be converted to an integer, Python raises a `ValueError`.

The museum program assumes valid input because the canonical pseudocode does too.

A production program should validate it.

---

## The array became a list

The pseudocode uses a fixed-size array:

```text
DECLARE People : ARRAY[1:MaxPassengers] OF Person
```

Python's natural choice is a **list**:

```python
people: list[Person] = []
```

and new values are appended:

```python
people.append(Person(name, age))
```

Python lists are dynamically sized. We therefore do not reserve 100 slots in advance.

This is an example of the museum preserving the behaviour rather than mechanically copying the reference representation.

---

## `range()` drives the counted loop

The pseudocode says:

```text
FOR Index ← 1 TO NumPassengers
```

Python commonly writes:

```python
for _ in range(num_passengers):
```

`range(num_passengers)` produces the integer sequence:

```text
0, 1, 2, ... num_passengers - 1
```

But this loop never uses the number itself.

The name `_` is a convention meaning approximately:

> Yes, Python, I know you are giving me a value here. I do not care about it.

---

## Iterating over the people is more direct

Instead of keeping an index:

```python
for person in people:
    print(person.name)
```

iterates directly over each `Person` object.

This is very common Python style.

You *could* write an index-based loop, but doing so when the index serves no purpose tends to look like you translated C literally while Python watched from across the room.

---

# The factorial trap

This exhibit is where the museum specimen gives Python an opportunity to show off.

Python's ordinary `int` type supports **arbitrary-precision integers**.

That means:

```python
factorial(18)
factorial(37)
factorial(100)
```

can all produce exact integers, subject to available memory and computation time.

There is no fixed 32-bit or 64-bit overflow point for normal Python integers.

For example:

```python
print(100!)
```

is not legal Python syntax because Python does not use `!` as a postfix factorial operator, but:

```python
import math
print(math.factorial(100))
```

works perfectly well.

Our museum version deliberately implements factorial recursively instead of using `math.factorial()` because recursion is one of the concepts the specimen is designed to expose.

In real code, use the standard library when the standard library already knows what it is doing.

---

# Recursion in Python

The factorial implementation:

```python
def factorial(num: int) -> int:
    if num <= 1:
        return 1

    return num * factorial(num - 1)
```

is almost a direct translation of the pseudocode.

Python supports recursion, but it does **not** optimize tail calls away, and CPython intentionally has a recursion limit to prevent uncontrolled recursion from consuming the C stack.

That does not matter for normal human ages.

If your bar patron is old enough to hit Python's recursion limit, the alcohol is probably the least interesting part of the evening.

---

# Python's object model

In Python, essentially everything you work with is an object.

Integers are objects.

Strings are objects.

Functions are objects.

Classes are objects.

Modules are objects.

You can assign a function to another variable:

```python
say_hello = greet
say_hello("Mika")
```

You can pass functions to other functions.

You can inspect objects dynamically.

This flexibility is one reason Python is comfortable in several programming styles rather than enforcing one grand unified ideology.

---

# Dynamic typing does not mean "no types"

This:

```python
age = 18
```

creates an integer object and binds the name `age` to it.

Later, Python permits:

```python
age = "eighteen"
```

because the *name* is not permanently declared to hold integers.

But values still have concrete types:

```python
type(18)
# <class 'int'>

type("eighteen")
# <class 'str'>
```

And incompatible operations still fail:

```python
"18" + 1
```

raises a `TypeError`.

Dynamic typing means type checks happen primarily at runtime.

It does not mean Python has dissolved into type-flavoured fog.

---

# Type hints

Modern Python lets us write:

```python
def age_check(age: int) -> str:
```

These are **type hints**.

They make intent clearer and can be checked by tools such as Pyright, mypy and IDEs.

But CPython itself will still allow:

```python
age_check("banana")
```

to enter the function.

It will then fail when Python attempts:

```python
"banana" >= 18
```

because those values cannot be ordered that way.

This is a major difference from statically typed languages such as C++.

---

# Lists, tuples, sets and dictionaries

Python ships with several important collection types directly in the language's everyday vocabulary.

```python
names = ["Mika", "Ren"]              # list
point = (10, 20)                     # tuple
unique_names = {"Mika", "Ren"}       # set
ages = {"Mika": 21, "Ren": 23}       # dictionary
```

The museum specimen uses a list because order matters and duplicate names should remain possible.

The original rough draft of the specimen used a dictionary from name to age. That is tempting in Python:

```python
people = {}
people[name] = age
```

but it also means two patrons named Alex would collide.

The record-list representation is semantically cleaner.

---

# Truthiness

Python conditionals do not require literal `True` or `False`.

Many values have a truth value:

```python
if people:
    print("At least one person exists.")
```

An empty list is falsey.

A non-empty list is truthy.

Likewise, `0`, `""`, `None`, empty tuples, empty dictionaries and empty sets are falsey.

This can make Python code concise, although explicit comparisons are sometimes clearer.

---

# Exceptions

Python uses exceptions for errors and unusual conditions:

```python
try:
    age = int(input("Age: "))
except ValueError:
    print("That was not an integer.")
```

Exceptions propagate up the call stack until something handles them.

Python code generally prefers exceptions over C-style numeric error codes for many kinds of failure.

The canonical specimen omits validation so its structure remains comparable across languages.

---

# Modules and packages

Python source files can act as modules.

If `museum.py` contains:

```python
def greet(name):
    ...
```

another file can import it:

```python
import museum

museum.greet("Mika")
```

or:

```python
from museum import greet

greet("Mika")
```

Larger projects organize modules into packages.

The Python ecosystem's primary package index is **PyPI**, and package installation is commonly performed with tools such as `pip`, although modern Python packaging has considerably more moving parts than those four letters suggest.

That story deserves its own exhibit label reading **PLEASE DO NOT FEED THE BUILD BACKENDS**.

---

# Implementations

Python is a language, not one specific executable.

The dominant implementation is **CPython**, written primarily in C.

Other implementations include or have included:

- **PyPy**, with a tracing JIT compiler;
- **MicroPython**, aimed at microcontrollers and constrained systems;
- **Jython**, targeting the Java platform;
- **IronPython**, targeting .NET.

When somebody casually says "Python 3.14", they are very often talking about CPython 3.14, but the distinction between language and implementation matters.

---

# The Global Interpreter Lock

CPython has historically had a **Global Interpreter Lock**, usually shortened to **GIL**, which affects how Python bytecode executes across native threads.

Modern Python is in the middle of a major evolution here: CPython now also supports a **free-threaded build** where the GIL can be disabled.

That does not mean every Python installation has suddenly become GIL-free, and it does not mean concurrency has become simple. It means "Python has a GIL" is no longer a sufficiently precise museum label.

Concurrency in Python can involve threads, processes, `asyncio`, native extensions, subinterpreters and other machinery.

The rabbit hole has several entrances.

---

# Things Python makes easy

Python is especially pleasant for:

- scripting and automation;
- quick prototypes;
- command-line tools;
- web backends;
- scientific computing;
- data analysis;
- machine learning;
- glue code;
- education;
- text processing.

Its enormous third-party ecosystem is as important to its popularity as the core language.

---

# Things Python makes less easy

Python is not automatically the right choice for everything.

Common trade-offs include:

- lower raw execution speed than optimized native languages for many workloads;
- runtime type errors that static checking might have caught earlier;
- packaging and environment management that can become surprisingly complicated;
- CPython implementation details affecting threading and performance;
- difficulty shipping tiny self-contained native binaries compared with some compiled languages.

None of these make Python "bad".

Languages are collections of trade-offs wearing syntax.

---

# Shitpost cabinet

### Significant whitespace

Other languages:

```text
Please use indentation so humans can read this.
```

Python:

```text
Compliance is mandatory.
```

### `import antigravity`

This is valid Python:

```python
import antigravity
```

In CPython it opens the famous XKCD comic about Python.

The standard library contains an official joke import.

The prosecution rests.

### The Zen of Python

Run:

```python
import this
```

and Python prints **The Zen of Python**, a short collection of aphorisms associated with Python's design culture.

The fact that one of the language's easter eggs is philosophy while another launches XKCD is an extremely efficient summary of Python.

---

# Learn more

Language Museum is not a Python course.

Good next stops:

- [Official Python website](https://www.python.org/)
- [Python 3.14 documentation](https://docs.python.org/3.14/)
- [The Python Tutorial](https://docs.python.org/3.14/tutorial/)
- [Python Language Reference](https://docs.python.org/3.14/reference/)
- [Python Standard Library](https://docs.python.org/3.14/library/)
- [Python Enhancement Proposals](https://peps.python.org/)
- [PyPI](https://pypi.org/)

For the source program this exhibit implements:

- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

Python translates the canonical specimen with remarkably little ceremony.

The biggest changes are not algorithmic. They are cultural:

- indentation defines blocks;
- variables need no declarations;
- functions and procedures are the same construct;
- lists grow dynamically;
- type hints are optional;
- integers grow to arbitrary precision;
- iteration tends to work directly with values rather than indexes.

That is why Python is such a useful first real language in the museum.

The pseudocode takes off its name badge, puts on a hoodie, and is suddenly suspiciously close to executable.
