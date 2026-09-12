# C++

> **Collection:** `languages/`  
> **Kind:** General-purpose systems programming language  
> **Paradigms:** Multi-paradigm: procedural, object-oriented, generic, functional techniques where useful  
> **Created by:** Bjarne Stroustrup  
> **Origins:** "C with Classes" work began in 1979; the name C++ appeared in the 1980s  
> **First standardized:** 1998  
> **File extensions:** commonly `.cpp`, `.cc`, `.cxx`, with headers such as `.h` / `.hpp`  
> **Museum target:** C++23  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)

C++ is what happens when a language spends decades accumulating ways to tell the computer exactly what you mean, ways to avoid telling the computer exactly what you mean, and ways for the compiler to write a novella explaining that you meant something impossible.

It is a high-performance, statically typed, compiled language with roots in C and an enormous modern feature set.

C++ is used in places where performance, memory layout, latency, native interoperability or direct control over resources matter:

- operating systems and system components;
- game engines;
- browsers;
- databases;
- embedded software;
- high-performance computing;
- desktop applications;
- compilers;
- financial systems;
- graphics;
- infrastructure.

It is also used in places where somebody said:

> We already have four million lines of C++.

which is one of the strongest forces in software engineering.

---

# Current status

As of **12 September 2026**, the most recently published ISO C++ standard is **C++23**, formally published as **ISO/IEC 14882:2024**.

The slightly weird date is administrative: the language revision is called C++23 because the technical work was completed and approved on the C++23 cycle, even though the ISO publication carries a 2024 suffix.

Work on **C++26** is advanced but C++26 is not yet the published standard this exhibit targets.

So the museum code uses ordinary modern C++ that is comfortable in a C++23 compiler without depending on draft C++26 features.

---

# A tiny bit of history

Bjarne Stroustrup began developing what was initially called **C with Classes** at Bell Labs around 1979.

The project aimed to combine C's efficiency and systems-programming capabilities with higher-level abstraction mechanisms inspired partly by languages such as Simula.

The name **C++** arrived in the 1980s.

The `++` is the increment operator inherited from C:

```cpp
x++;
```

So the name is essentially a programmer joke meaning something like:

> C, but incremented.

This is either charming or an early warning.

C++ became standardized in 1998 and has continued evolving through revisions including:

- C++98
- C++03
- C++11
- C++14
- C++17
- C++20
- C++23

C++11 was especially transformative, introducing or standardizing a huge wave of "modern C++" features such as move semantics, lambdas, `auto`, smart pointers, range-based `for`, and a standardized memory model.

Modern C++ is therefore not just "C with classes" anymore.

It is a very large language whose archaeology remains visible in the walls.

---

# First impressions

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name = "Mika";
    int age = 22;

    if (age >= 18) {
        std::cout << name << " passes the age check.\n";
    } else {
        std::cout << "No :(\n";
    }

    return 0;
}
```

Immediately, this is considerably more explicit than Python.

We have:

- header inclusion;
- static types;
- a `main()` entry point;
- braces;
- semicolons;
- names from the standard library's `std` namespace;
- explicit program structure.

C++ gives the programmer enormous control.

The invoice for that control is also enormous.

---

# The factorial problem

Before translating the museum program, C++ forces us to make a decision.

The canonical pseudocode conceptually has integers large enough to represent a person's factorial.

C++'s standard integer types are **fixed-width or implementation-bounded machine integers**.

For example, on typical systems:

```cpp
unsigned long long
```

is 64 bits.

That can represent:

```text
20! = 2432902008176640000
```

but not:

```text
21! = 51090942171709440000
```

So a standard-library-only implementation using ordinary integers would overflow for a 21-year-old.

That is not an obscure corner case.

Our bar would break as soon as somebody legally old enough to drink in the United States walked in.

Excellent specimen design.

---

# Exact implementation with Boost.Multiprecision

To preserve the canonical program's exact factorial behaviour, this exhibit uses:

```cpp
boost::multiprecision::cpp_int
```

from **Boost.Multiprecision**.

Important distinction:

> **Boost is not part of the C++ standard library.**

It is a widely used external C++ library collection.

This is therefore a C++ implementation of the canonical program with a library dependency, chosen deliberately because standard C++ currently has no built-in arbitrary-precision integer type.

That limitation is itself part of the exhibit.

---

# The museum program

```cpp
#include <boost/multiprecision/cpp_int.hpp>

#include <iostream>
#include <string>
#include <vector>

constexpr const char* LANGUAGE = "C++";
constexpr int MAX_PASSENGERS = 100;

struct Person {
    std::string name;
    int age;
};

void greet(const std::string& name) {
    std::cout << "Hi " << name << "!\n";
    std::cout << "I hope you are well, " << name << '\n';
    std::cout << "Nice to meet you, " << name << '\n';
}

std::string age_check(int age) {
    if (age >= 18) {
        return "You may legally drink alcohol here.";
    } else {
        return "No :(";
    }
}

boost::multiprecision::cpp_int factorial(unsigned int num) {
    if (num <= 1) {
        return 1;
    }

    return num * factorial(num - 1);
}

int main() {
    std::vector<Person> people;

    int num_passengers;

    std::cout
        << "How many people are you going into the bar with? "
        << "(maximum " << MAX_PASSENGERS << ") ";

    std::cin >> num_passengers;

    for (int index = 0; index < num_passengers; ++index) {
        std::cout << "What is your name? ";

        std::string name;
        std::cin >> std::ws;
        std::getline(std::cin, name);

        greet(name);

        std::cout << "How old are you (in human years)? ";

        int age;
        std::cin >> age;

        std::cout << age_check(age) << '\n';

        people.push_back(Person{name, age});
    }

    std::cout
        << "Fun fact! For everyone here, "
        << "their ages in factorial would be:\n";

    for (const Person& person : people) {
        std::cout
            << "- " << person.name
            << " is "
            << factorial(static_cast<unsigned int>(person.age))
            << '\n';
    }

    std::cout
        << "Interactive museum entry for "
        << LANGUAGE
        << " completed.\n";

    return 0;
}
```

Assuming Boost is installed, one common compile command with GCC is:

```sh
g++ -std=c++23 -O2 museum.cpp -o museum
```

Then:

```sh
./museum
```

On Windows, depending on toolchain and shell:

```powershell
.\museum.exe
```

Clang can compile the same source with a similar command:

```sh
clang++ -std=c++23 -O2 museum.cpp -o museum
```

MSVC uses different command-line syntax.

C++ is standardized as a language, but compilers and build systems remain gloriously plural.

---

# What would a standard-library-only factorial look like?

If we deliberately accept machine-integer limits, we could write:

```cpp
unsigned long long factorial(unsigned int num) {
    if (num <= 1) {
        return 1;
    }

    return num * factorial(num - 1);
}
```

This version is compact and uses no third-party library.

It also stops being mathematically reliable after `20!` on the common 64-bit `unsigned long long` case.

Unsigned integer overflow in C++ wraps modulo a power of two.

Signed integer overflow is worse: it is **undefined behaviour**.

So the museum uses Boost for the canonical translation rather than turning patrons over 20 into numerical fan fiction.

---

# What changed from the pseudocode?

## Types are explicit and static

The pseudocode says:

```text
DECLARE Age : INTEGER
```

C++ says:

```cpp
int age;
```

and:

```text
DECLARE Name : STRING
```

becomes:

```cpp
std::string name;
```

The compiler knows these types before the program runs.

This means:

```cpp
age = "banana";
```

is rejected at compile time.

Static typing is central to C++, although modern C++ also has type inference:

```cpp
auto age = 18;
```

Here `age` is still statically typed. The compiler simply inferred `int`.

`auto` does **not** turn C++ into a dynamically typed language.

---

# `struct` matches the record nicely

The pseudocode's:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE
```

maps naturally to:

```cpp
struct Person {
    std::string name;
    int age;
};
```

C++ also has `class`.

The practical language-level difference is mostly default access:

- members of a `struct` are `public` by default;
- members of a `class` are `private` by default.

Both can have methods, constructors, inheritance, templates and the rest of C++'s object machinery.

Using `struct` for a simple data record is common style.

---

# The procedure becomes `void`

Cambridge pseudocode has:

```text
PROCEDURE Greet(...)
```

C++ uses a function whose return type is `void`:

```cpp
void greet(const std::string& name) {
    ...
}
```

`void` means the function returns no value.

Functions that do return values declare the return type:

```cpp
std::string age_check(int age)
```

and:

```cpp
boost::multiprecision::cpp_int factorial(unsigned int num)
```

This is a major contrast with Python and JavaScript, where the return type is not part of the runtime function declaration.

---

# What is `const std::string&` doing?

This parameter:

```cpp
const std::string& name
```

contains several C++ ideas in miniature.

`std::string` is the type.

`&` means the parameter is a **reference** to an existing string rather than a new copied string.

`const` means the function promises not to modify that string through this reference.

So:

```cpp
void greet(const std::string& name)
```

roughly communicates:

> Give me access to an existing string without copying it, and I will only read it.

This kind of performance-and-ownership detail is much more visible in C++ than in Python or JavaScript.

---

# `std::vector` replaces the fixed array

The pseudocode uses:

```text
ARRAY[1:MaxPassengers] OF Person
```

Modern C++ commonly uses:

```cpp
std::vector<Person> people;
```

A `std::vector` is a dynamically sized contiguous sequence.

We append with:

```cpp
people.push_back(Person{name, age});
```

The braced expression:

```cpp
Person{name, age}
```

initializes the `Person` structure.

C++ also has built-in arrays and `std::array`, but `std::vector` is a more natural match for "collect an input-dependent number of people".

---

# Range-based `for`

The pseudocode indexes through its array.

Modern C++ can iterate directly:

```cpp
for (const Person& person : people) {
    ...
}
```

Read this approximately as:

> for each `Person` referenced as `person` in `people`

Again we use a `const` reference so each object does not need to be copied and the loop body cannot modify it through that reference.

The older index-based style is also valid:

```cpp
for (std::size_t i = 0; i < people.size(); ++i) {
    std::cout << people[i].name;
}
```

Both forms matter in real C++.

---

# Why does input suddenly contain `std::ws`?

We read the passenger count with:

```cpp
std::cin >> num_passengers;
```

The formatted extraction operator leaves the trailing newline in the input stream.

Then we want a whole line for the person's name:

```cpp
std::getline(std::cin, name);
```

If we call `getline()` immediately, it may consume the leftover newline and return an empty string.

So the exhibit writes:

```cpp
std::cin >> std::ws;
std::getline(std::cin, name);
```

`std::ws` consumes leading whitespace first.

This tiny wrinkle is extremely C++.

We came here to ask somebody's name and accidentally learned about stream state.

---

# Streams and `<<`

Output:

```cpp
std::cout << "Hi " << name << "!\n";
```

uses the stream insertion operator `<<`.

Input:

```cpp
std::cin >> age;
```

uses the extraction operator `>>`.

These operators are overloadable, which means user-defined types can participate in stream syntax too.

For example, a library type can define how it should print when inserted into an output stream.

Operator overloading is one of C++'s major abstraction tools.

Used well, it makes custom types feel natural.

Used badly, `a + b` can summon a database transaction and send an email.

Power demands adult supervision.

---

# Namespaces

Why does C++ keep saying:

```cpp
std::string
std::vector
std::cout
```

?

The standard library puts most of its names in the namespace:

```cpp
std
```

The `::` operator accesses a name inside a namespace or class scope.

You may encounter:

```cpp
using namespace std;
```

which makes standard-library names available without the `std::` prefix.

It is common in tiny examples and competitive programming.

It is often avoided in larger code, especially in headers, because importing an entire namespace can create name collisions and make origins less clear.

The museum keeps the qualification explicit.

---

# Compilation

Python and JavaScript are often experienced through an interpreter or runtime command.

C++ is traditionally **compiled ahead of time**.

A simplified pipeline is:

```text
source code
   ↓
preprocessing
   ↓
compilation
   ↓
object files
   ↓
linking
   ↓
native executable
```

Real toolchains add many delicious layers of complexity.

Header files, libraries, translation units, linker errors, ABI compatibility, build systems and compiler flags are major parts of practical C++ development.

The language is only one floor of the building.

---

# The preprocessor

This:

```cpp
#include <iostream>
```

is handled by the C++ preprocessor.

Historically, `#include` effectively causes header contents to be made available to a translation unit through textual inclusion.

The preprocessor also supports macros:

```cpp
#define SOMETHING 42
```

Modern C++ provides many language features that reduce the need for macro tricks, but the preprocessor remains deeply embedded in the ecosystem.

C++20 also introduced **modules**, intended to provide a more structured alternative for many code-organization use cases.

Adoption is ongoing and toolchain support has historically varied.

C++ has never met a migration it couldn't turn into a multi-year archaeological dig.

---

# Memory and object lifetime

C++ gives programmers much more direct control over object lifetime than garbage-collected languages.

Objects can live in automatic storage:

```cpp
Person person{"Mika", 22};
```

and be destroyed automatically when their scope ends.

Dynamic allocation is possible:

```cpp
new Person{"Mika", 22}
```

but modern C++ strongly encourages **RAII** and ownership types rather than scattering raw `new` and `delete` across a codebase.

For dynamically owned objects, smart pointers include:

```cpp
std::unique_ptr<T>
std::shared_ptr<T>
```

RAII stands for **Resource Acquisition Is Initialization**.

The central idea is that resource lifetime is tied to object lifetime, so destructors release resources automatically.

This works not only for memory, but files, locks, sockets and other resources.

RAII is one of the most important ideas in idiomatic C++.

---

# Pointers and references

C++ has both pointers and references.

A pointer:

```cpp
Person* pointer = &person;
```

stores an address and can be null.

A reference:

```cpp
Person& reference = person;
```

acts as another name for an existing object and is designed around non-null aliasing semantics.

Then C++ adds:

- `const`;
- pointer-to-const;
- const pointer;
- rvalue references;
- smart pointers;
- iterators;
- spans;
- views.

The museum gift shop sells a laminated ownership diagram.

It is A1-sized.

---

# Templates

Templates provide C++'s generic programming system.

For example:

```cpp
template <typename T>
T larger(T a, T b) {
    return a > b ? a : b;
}
```

The compiler can instantiate versions for different suitable types.

Much of the standard library is template-based:

```cpp
std::vector<int>
std::vector<std::string>
std::optional<Person>
```

Templates are extraordinarily powerful.

Template error messages have historically also been extraordinarily powerful, in the sense that a firehose is powerful.

Modern compilers have improved considerably.

---

# Compile-time programming

Modern C++ can perform substantial computation during compilation.

For example:

```cpp
constexpr int square(int x) {
    return x * x;
}

constexpr int value = square(12);
```

`constexpr`, templates, concepts and other facilities make compile-time programming a major part of modern C++.

This can improve performance, validate invariants and generate specialized code.

It can also cause your compiler to spend several meaningful seconds contemplating a type.

---

# Undefined behaviour

One of the most important concepts in C and C++ is **undefined behaviour**.

Some operations are outside what the language standard defines.

A classic example is overflowing a signed integer:

```cpp
int x = INT_MAX;
x = x + 1; // signed overflow: undefined behaviour
```

"Undefined" does not mean:

> the result wraps around.

It means the C++ standard imposes no requirements on what happens.

Compilers can use assumptions about undefined behaviour when optimizing code.

This is powerful for performance and hazardous when code accidentally violates the rules.

C++ therefore rewards programmers who understand not only what their machine appears to do, but what the language actually promises.

---

# Zero-cost abstractions

C++ culture often talks about **zero-cost abstractions**.

The rough ideal is:

> You should not pay at runtime for language abstractions you do not use, and a well-designed abstraction should be capable of compiling down to code comparable with a hand-written lower-level implementation.

Templates, inlining, RAII and compile-time polymorphism all contribute to this philosophy.

"Zero cost" does not mean compilation is free, complexity is free, or your build server has forgiven you.

---

# Multiple programming styles

C++ supports many styles at once.

Procedural:

```cpp
do_something();
```

Object-oriented:

```cpp
person.greet();
```

Generic:

```cpp
std::sort(values.begin(), values.end());
```

Functional-ish:

```cpp
std::ranges::transform(...)
```

Compile-time:

```cpp
constexpr
```

Low-level:

```cpp
std::byte*
```

High-level:

```cpp
std::vector<std::string>
```

This breadth is a major strength.

It is also why two C++ codebases can look like they were written in different languages by civilizations that communicated only through linker errors.

---

# C versus C++

C++ grew from C and maintains substantial compatibility and shared syntax.

But modern C++ is **not simply C with extra features**.

Idiomatic C++ uses abstractions that do not map neatly onto typical C style:

- constructors and destructors;
- RAII;
- templates;
- references;
- exceptions;
- standard containers;
- algorithms;
- lambdas;
- smart pointers;
- ranges;
- concepts.

Code written as "C, but compiled as C++" may be valid, but it is not representative of the language's full design.

C and C++ deserve separate museum exhibits.

Putting them in one cabinet would start an argument audible from the gift shop.

---

# Implementations and compilers

Major C++ compiler families include:

- **GCC / G++**
- **Clang / LLVM**
- **Microsoft Visual C++ (MSVC)**

They target many platforms and generally aim to implement the ISO standard, but support for the newest language and library features arrives at different times.

That means:

> valid C++23

and:

> accepted by the specific compiler/library version installed on this machine

are related but not identical statements.

Compiler conformance tables matter when using fresh language features.

---

# Build systems

A single file can be compiled directly:

```sh
g++ museum.cpp -o museum
```

Real C++ projects often use build systems such as:

- CMake;
- Meson;
- Bazel;
- Ninja as a lower-level build executor;
- Visual Studio project systems;
- many others.

Dependency management has historically been less centralized than ecosystems such as npm or Cargo.

Tools such as vcpkg and Conan are common modern options.

This part of C++ has improved substantially.

It has also generated enough configuration files to alter local weather patterns.

---

# Shitpost cabinet

## C++ declaration archaeology

This is a real kind of C++ type:

```cpp
int (*function_pointer)(double);
```

It means:

> `function_pointer` is a pointer to a function taking a `double` and returning an `int`.

There is a systematic grammar.

There is also a point at which everybody writes:

```cpp
using Callback = int (*)(double);
```

and chooses happiness.

## `std::vector<bool>`

You would reasonably expect:

```cpp
std::vector<bool>
```

to behave exactly like `std::vector<T>` instantiated with `bool`.

History has other plans.

`std::vector<bool>` has a special space-saving representation and proxy-reference behaviour.

It is one of the standard library's most famous "well, technically..." exhibits.

## Template errors

Old C++ compiler diagnostics around templates became legendary for producing screens of text in response to one tiny mistake.

Modern diagnostics are far better.

The memes have tenure.

---

# Learn more

Language Museum is emphatically not a complete C++ course.

Useful resources:

- [Standard C++](https://isocpp.org/)
- [Current ISO C++ status](https://isocpp.org/std/status)
- [cppreference](https://en.cppreference.com/w/)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [LearnCpp](https://www.learncpp.com/)
- [Boost](https://www.boost.org/)
- [Boost.Multiprecision](https://www.boost.org/doc/libs/release/libs/multiprecision/)

Compiler resources:

- [GCC](https://gcc.gnu.org/)
- [Clang](https://clang.llvm.org/)
- [Microsoft C++](https://learn.microsoft.com/cpp/)

For the source program:

- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

The C++ translation exposes several differences immediately:

- static types are part of function and variable declarations;
- records map naturally to `struct`;
- procedures become `void` functions;
- dynamic collections use library types such as `std::vector`;
- references and `const` make copying and mutation policy explicit;
- source is normally compiled to native code;
- resource lifetime is a core language concern;
- standard integer types have finite bounds.

And then factorial detonates the glass case.

Python simply grows its integer.

JavaScript reaches for `BigInt`.

Standard C++ says:

> I can give you several extremely fast integer types. Arbitrary precision is a library problem.

So the exact museum implementation imports Boost.Multiprecision.

That single difference captures a surprising amount of C++ philosophy: the language gives you powerful low-level primitives and abstractions, but it does not insist that every useful facility belong in the core or standard library.

C++ is less interested in hiding the machine than in giving you enough abstraction to negotiate with it.
