# C

> **Collection:** `languages/`  
> **Kind:** General-purpose systems programming language  
> **Paradigms:** Primarily imperative / procedural  
> **Created by:** Dennis Ritchie at Bell Labs  
> **Origins:** Early 1970s  
> **First standardized:** 1989  
> **Current standard:** C23, published as ISO/IEC 9899:2024  
> **File extensions:** commonly `.c` with headers such as `.h`  
> **Museum target:** C23, while keeping the specimen broadly portable to older modern C compilers  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)  
> **Related exhibit:** [`cpp.md`](cpp.md)

C is one of the load-bearing languages of modern computing.

Operating systems, compilers, language runtimes, databases, embedded firmware, networking stacks, libraries and approximately half the infrastructure beneath higher-level languages either are written in C, have substantial C components, expose C interfaces, or have spent their lives negotiating with C's calling conventions.

It is small by the standards of modern general-purpose languages.

It is also extraordinarily powerful.

C gives you:

- functions;
- structs;
- arrays;
- pointers;
- manual dynamic memory;
- arithmetic close to machine representations;
- a modest standard library;
- and enough rope to build an operating system, a database, or an extremely educational memory corruption bug.

C++ grew out of C, but **C is not merely "C++ without classes"**.

Modern C and modern C++ are separate languages with separate standards, separate idioms, diverging feature sets, and enough compatibility to ensure people will continue arguing about exactly how related they are until the heat death of the universe.

---

# Current status

The current published C standard is **C23**.

Its formal ISO publication is:

**ISO/IEC 9899:2024**

The name `C23` refers to the revision cycle even though the final ISO publication appeared in 2024.

C23 adds and standardizes features including things such as:

- `_BitInt` bit-precise integer types;
- binary integer literals;
- digit separators;
- attributes;
- `nullptr`;
- `typeof` / `typeof_unqual`;
- empty initializers;
- further Unicode-related changes;
- several formerly library-macro concepts becoming language keywords;
- and removal of some very old pre-standard-style constructs.

This exhibit avoids depending heavily on brand-new C23 features because practical compiler support arrives gradually.

The canonical program should remain understandable and compilable on a wide range of current C toolchains.

---

# A tiny bit of history

C emerged at **Bell Labs** in the early 1970s.

Dennis Ritchie developed it from earlier languages including **B**, which itself descended from **BCPL**.

C became deeply connected with the development of **Unix**.

That combination mattered enormously.

Unix demonstrated that a substantial operating system could be written largely in a high-level language rather than almost entirely in assembly, while C remained close enough to machine operations to make systems programming practical.

C then spread with Unix, escaped Unix, entered almost every computing niche imaginable, and became one of the most influential programming languages ever created.

Languages influenced directly or indirectly by C include:

- C++;
- Objective-C;
- Java;
- C#;
- JavaScript's surface syntax;
- Go;
- Rust;
- countless scripting and systems languages.

Even languages that reject C's memory model frequently still inherit pieces of its punctuation.

Curly braces have excellent international relations.

---

# K&R C

Before there was an ISO C standard, one of the most important descriptions of the language was:

**The C Programming Language**

by Brian Kernighan and Dennis Ritchie.

The first edition appeared in 1978.

The dialect described by that book is commonly called **K&R C**.

Later standardization formalized and changed parts of the language.

This matters when reading ancient C source because code from 1981 may be perfectly authentic C while looking illegal or deeply suspicious to a modern compiler.

C has history in production.

---

# Standardization

C's major standardized generations include:

- **C89 / ANSI C**
- **C90 / ISO C**
- **C95**
- **C99**
- **C11**
- **C17**
- **C23**

The C89 and C90 standards are essentially the first ANSI/ISO generation with small publication differences.

C99 was particularly influential, bringing features such as:

- `//` comments;
- `inline`;
- variable declarations in more places;
- designated initializers;
- variable-length arrays;
- `long long`;
- standard integer types through `<stdint.h>`;
- several library additions.

C11 added, among other things:

- atomics;
- threads in the standard library;
- `_Generic`;
- `_Static_assert`;
- alignment support.

Not every compiler implements every optional or recent feature equally.

"Standard C" and "what this embedded compiler from 2009 accepts" can be dramatically different museum exhibits.

---

# First impressions

```c
#include <stdio.h>

int main(void) {
    const char *name = "Mika";
    int age = 22;

    if (age >= 18) {
        printf("%s passes the age check.\n", name);
    } else {
        printf("No :(\n");
    }

    return 0;
}
```

Compared with Python, C immediately exposes more machinery.

There is:

- a preprocessor include;
- explicit static types;
- a `main` entry point;
- braces;
- semicolons;
- a formatted-output function;
- a pointer hiding inside `const char *`.

C has a way of turning "hello" into a small guided tour of the machine.

---

# The canonical Language Museum program

The museum's factorial specimen exposes an important C limitation:

> **Standard C has no built-in arbitrary-precision integer type.**

We could therefore do what the C++ exhibit does and use an external multiprecision library.

But for C, it is more educational to build the tiny amount of arbitrary-precision arithmetic we need ourselves.

The program below represents a non-negative integer as a dynamically allocated array of **decimal digits**, stored least-significant digit first.

For example:

```text
1234
```

is internally stored as:

```text
[4, 3, 2, 1]
```

That is not the fastest possible big-integer representation.

It is intentionally simple enough to understand in a museum exhibit.

The recursive `factorial()` therefore retains the pseudocode's behaviour without pretending that a normal C integer can hold `50!`.

```c
#include <ctype.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define LANGUAGE "C"
#define MAX_PASSENGERS 100
#define MAX_NAME_LENGTH 100

typedef struct {
    char name[MAX_NAME_LENGTH + 1];
    unsigned int age;
} Person;

typedef struct {
    unsigned char *digits;
    size_t length;
    size_t capacity;
} BigUInt;


static void greet(const char *name) {
    printf("Hi %s!\n", name);
    printf("I hope you are well, %s\n", name);
    printf("Nice to meet you, %s\n", name);
}


static const char *age_check(unsigned int age) {
    if (age >= 18) {
        return "You may legally drink alcohol here.";
    } else {
        return "No :(";
    }
}


static bool biguint_reserve(BigUInt *number, size_t capacity) {
    if (capacity <= number->capacity) {
        return true;
    }

    unsigned char *new_digits =
        realloc(number->digits, capacity * sizeof(*new_digits));

    if (new_digits == NULL) {
        return false;
    }

    number->digits = new_digits;
    number->capacity = capacity;
    return true;
}


static bool biguint_set_one(BigUInt *number) {
    if (!biguint_reserve(number, 1)) {
        return false;
    }

    number->digits[0] = 1;
    number->length = 1;
    return true;
}


static bool biguint_multiply_small(BigUInt *number, unsigned int factor) {
    unsigned long long carry = 0;

    for (size_t i = 0; i < number->length; ++i) {
        unsigned long long product =
            (unsigned long long) number->digits[i] * factor + carry;

        number->digits[i] = (unsigned char) (product % 10);
        carry = product / 10;
    }

    while (carry > 0) {
        if (number->length == number->capacity) {
            size_t new_capacity =
                number->capacity == 0 ? 1 : number->capacity * 2;

            if (!biguint_reserve(number, new_capacity)) {
                return false;
            }
        }

        number->digits[number->length] =
            (unsigned char) (carry % 10);

        ++number->length;
        carry /= 10;
    }

    return true;
}


static bool factorial(unsigned int num, BigUInt *result) {
    if (num <= 1) {
        return biguint_set_one(result);
    }

    if (!factorial(num - 1, result)) {
        return false;
    }

    return biguint_multiply_small(result, num);
}


static void biguint_print(const BigUInt *number) {
    for (size_t i = number->length; i > 0; --i) {
        putchar('0' + number->digits[i - 1]);
    }
}


static void biguint_free(BigUInt *number) {
    free(number->digits);

    number->digits = NULL;
    number->length = 0;
    number->capacity = 0;
}


static bool read_line(char *buffer, size_t size) {
    if (fgets(buffer, (int) size, stdin) == NULL) {
        return false;
    }

    size_t newline = strcspn(buffer, "\n");
    buffer[newline] = '\0';

    return true;
}


static bool parse_unsigned(const char *text, unsigned int *value) {
    char *end = NULL;

    unsigned long parsed = strtoul(text, &end, 10);

    if (end == text) {
        return false;
    }

    while (isspace((unsigned char) *end)) {
        ++end;
    }

    if (*end != '\0') {
        return false;
    }

    if (parsed > 0xffffffffUL) {
        return false;
    }

    *value = (unsigned int) parsed;
    return true;
}


int main(void) {
    Person people[MAX_PASSENGERS];
    unsigned int num_passengers;

    char line[128];

    printf(
        "How many people are you going into the bar with? "
        "(maximum %d)\n",
        MAX_PASSENGERS
    );

    if (!read_line(line, sizeof line) ||
        !parse_unsigned(line, &num_passengers) ||
        num_passengers > MAX_PASSENGERS) {

        fprintf(stderr, "Invalid passenger count.\n");
        return EXIT_FAILURE;
    }

    for (unsigned int i = 0; i < num_passengers; ++i) {
        printf("What is your name?\n");

        if (!read_line(people[i].name, sizeof people[i].name)) {
            fprintf(stderr, "Could not read name.\n");
            return EXIT_FAILURE;
        }

        greet(people[i].name);

        printf("How old are you (in human years)?\n");

        if (!read_line(line, sizeof line) ||
            !parse_unsigned(line, &people[i].age)) {

            fprintf(stderr, "Invalid age.\n");
            return EXIT_FAILURE;
        }

        puts(age_check(people[i].age));
    }

    puts(
        "Fun fact! For everyone here, "
        "their ages in factorial would be:"
    );

    for (unsigned int i = 0; i < num_passengers; ++i) {
        BigUInt result = {0};

        if (!factorial(people[i].age, &result)) {
            fprintf(stderr, "Not enough memory for factorial.\n");
            return EXIT_FAILURE;
        }

        printf("- %s is ", people[i].name);
        biguint_print(&result);
        putchar('\n');

        biguint_free(&result);
    }

    printf(
        "Interactive museum entry for %s completed.\n",
        LANGUAGE
    );

    return EXIT_SUCCESS;
}
```

A typical GCC command is:

```sh
gcc -std=c23 -Wall -Wextra -pedantic museum.c -o museum
```

Some GCC versions still spell the C23 mode:

```sh
-std=c2x
```

Clang has similar language-mode flags.

Then:

```sh
./museum
```

On Windows, depending on the compiler and shell:

```powershell
.\museum.exe
```

---

# Why is C's specimen longer than C++'s?

This may look backward.

C++ is a much larger language.

But its standard library gives us abstractions such as:

```cpp
std::string
std::vector
```

and the exhibit used Boost for big integers.

C instead gives us lower-level building blocks.

So the C version has to decide explicitly:

- how long a name can be;
- how people are stored;
- how text input is buffered;
- how numbers are parsed;
- how large factorials are represented;
- how that representation grows;
- when memory is freed.

The language itself is smaller.

The *programmer's job* can therefore be larger.

---

# `struct`

The pseudocode's record:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE
```

maps naturally to:

```c
typedef struct {
    char name[MAX_NAME_LENGTH + 1];
    unsigned int age;
} Person;
```

A `struct` groups fields together.

Without the `typedef`, we could write:

```c
struct Person {
    ...
};
```

and refer to the type as:

```c
struct Person
```

The `typedef` gives it the shorter alias:

```c
Person
```

Unlike C++, C traditionally keeps the `struct` tag namespace and typedef names conceptually distinct.

This tiny syntax difference is one of the many ways C and C++ look almost identical until they do not.

---

# C does not have a built-in string type

This is a big one.

Python has:

```python
name = "Mika"
```

JavaScript has:

```javascript
const name = "Mika";
```

C has **arrays of characters** and pointers to characters.

A writable fixed-size name buffer:

```c
char name[101];
```

is 101 `char` elements.

A C string is conventionally a sequence of characters terminated by a zero byte:

```text
'M' 'i' 'k' 'a' '\0'
```

That final:

```c
'\0'
```

is the **null terminator**.

Forget it and many standard string functions stop knowing where the text ends.

This has produced approximately eight billion security advisories.

---

# Arrays know less than you might expect

Inside the same scope:

```c
char name[101];
```

the compiler can determine the whole array's size:

```c
sizeof name
```

But when an array is passed to a function, it normally **decays to a pointer** to its first element.

So:

```c
void f(char text[101])
```

does not mean the function receives a magical length-tracked 101-character array value.

For function parameters, that declaration is adjusted to pointer-like semantics.

C programmers therefore frequently pass lengths separately.

Example:

```c
void process(const char *data, size_t length);
```

The pointer says where.

The length says how much.

Lose either piece and the machine becomes an escape room.

---

# Pointers

A pointer stores an address.

```c
int age = 22;
int *pointer = &age;
```

`&age` means:

> address of `age`

and:

```c
*pointer
```

means:

> the object pointed to by `pointer`

So:

```c
*pointer = 23;
```

changes `age`.

Pointers enable:

- dynamic memory;
- arrays;
- linked data structures;
- output parameters;
- callbacks;
- memory-mapped hardware;
- low-level systems interfaces.

They also enable pointing at invalid memory.

C does not put bumpers on the bowling lane.

---

# `const`

This:

```c
const char *name
```

means the function accesses characters through a pointer that promises not to modify those characters through that pointer.

This:

```c
char *const pointer
```

means the pointer itself cannot be changed to point somewhere else.

This:

```c
const char *const pointer
```

means both restrictions apply.

The placement is systematic.

It also provides excellent material for interview whiteboards.

---

# Static typing

C is statically typed.

This:

```c
unsigned int age;
```

declares `age` as an unsigned integer.

The compiler knows the type before execution.

Unlike Python:

```python
age = 18
age = "eighteen"
```

you cannot simply reuse a C integer variable as a string.

The machine has paperwork.

---

# Integer sizes are not all universally fixed

A common beginner mistake is to assume:

```text
int = 32 bits
long = 64 bits
```

everywhere.

The C standard specifies **minimum ranges and relationships**, not one universal bit width for every traditional integer type.

For exact-width types where available, `<stdint.h>` provides names such as:

```c
int8_t
uint16_t
int32_t
uint64_t
```

But even those exact-width typedefs exist only when the implementation has a corresponding type with exactly that width and no padding bits.

C is portable partly because it refuses to pretend every machine is identical.

---

# `sizeof`

C can ask how much storage a type or object occupies:

```c
sizeof(int)
sizeof age
sizeof people
```

The result is measured in units of `char`, conventionally called bytes.

By definition:

```c
sizeof(char) == 1
```

A C byte is at least 8 bits, though modern mainstream systems overwhelmingly use 8-bit bytes.

The type of `sizeof`'s result is:

```c
size_t
```

which is an unsigned integer type designed to represent object sizes.

---

# The preprocessor

C source often begins with:

```c
#include <stdio.h>
```

`#include` is handled by the **preprocessor** before ordinary C compilation.

The preprocessor also supports macros:

```c
#define MAX_PASSENGERS 100
```

Conditional compilation:

```c
#ifdef _WIN32
    ...
#endif
```

and function-like macros:

```c
#define SQUARE(x) ((x) * (x))
```

Macros are textual/token-level transformations rather than normal functions.

This distinction matters.

For example:

```c
SQUARE(i++)
```

is a tiny haunted house.

---

# Header files

C projects conventionally put declarations shared across source files into headers:

```c
museum.h
```

and implementation into source files:

```c
museum.c
```

A header might contain:

```c
#ifndef MUSEUM_H
#define MUSEUM_H

void greet(const char *name);

#endif
```

The `#ifndef` / `#define` pattern is an **include guard**, preventing repeated inclusion from defining the same declarations multiple times.

C23 does not abolish decades of header culture.

---

# Compilation and linking

A simplified C build looks like:

```text
source files
    ↓
preprocessing
    ↓
compilation
    ↓
object files
    ↓
linking
    ↓
executable or library
```

Compile one source file:

```sh
gcc -c museum.c
```

produces an object file.

Link:

```sh
gcc museum.o helper.o -o museum
```

The linker resolves references between separately compiled units and libraries.

This means an error such as:

```text
undefined reference to ...
```

may not be a C-language syntax error at all.

It may be the linker informing you that your function has vanished into the bureaucratic void.

---

# Functions

A C function has an explicit return type:

```c
const char *age_check(unsigned int age)
```

A no-value function returns:

```c
void
```

such as:

```c
static void greet(const char *name)
```

The `static` here gives the function **internal linkage** at file scope.

Roughly:

> this function belongs to this translation unit; do not export it as a global linker-visible symbol.

This is unrelated to `static` local variables, which use the same keyword for a different storage-duration concept.

C enjoys making keywords multitask.

---

# Pass-by-value

C passes function arguments **by value**.

Always.

This:

```c
void change(int x) {
    x = 10;
}
```

changes only the function's local copy.

To modify a caller's object, pass its address:

```c
void change(int *x) {
    *x = 10;
}
```

called with:

```c
change(&age);
```

People sometimes describe this as "passing by reference".

C itself is still passing a **pointer value by value**.

This distinction becomes useful once pointer behaviour stops being theoretical.

---

# Dynamic memory

The factorial representation uses:

```c
realloc(...)
```

and later:

```c
free(...)
```

The standard dynamic-memory family includes:

```c
malloc
calloc
realloc
free
```

Example:

```c
int *numbers = malloc(100 * sizeof *numbers);
```

If allocation fails:

```c
numbers == NULL
```

When finished:

```c
free(numbers);
```

Forgetting to free allocations can leak memory.

Freeing them twice can corrupt the program.

Using them after `free()` is undefined behaviour.

C's garbage collector is the programmer remembering things.

---

# Why `sizeof *pointer` is a useful idiom

The museum does:

```c
capacity * sizeof(*new_digits)
```

rather than:

```c
capacity * sizeof(unsigned char)
```

Why?

Because the first form automatically tracks the pointed-to type.

If `new_digits` later changes type, the allocation expression remains correct.

C contains many small idioms like this that emerged from generations of programmers stepping on land mines and leaving helpful little flags.

---

# Standard library I/O

C's classic output function is:

```c
printf(...)
```

Example:

```c
printf("%s is %u\n", name, age);
```

The format string describes the argument types.

`%s` expects a C string.

`%u` expects an `unsigned int`.

If the format does not match the actual argument type, behaviour can be incorrect or undefined.

Modern compilers often warn about mismatches when the format string is visible.

Please listen to them.

---

# `fgets()` instead of `gets()`

The museum uses:

```c
fgets(buffer, size, stdin)
```

because it receives the size of the destination buffer.

The historical function:

```c
gets(buffer)
```

had no way to know how large `buffer` was.

It was so fundamentally unsafe that C11 **removed it from the standard**.

That is an extraordinary achievement.

Most obsolete APIs merely become deprecated.

`gets()` was escorted from the building.

---

# Why not just use `scanf("%s", ...)`?

Because:

```c
scanf("%s", name)
```

stops at whitespace.

Someone named:

```text
Mary Jane
```

would mysteriously become Mary.

Unchecked `%s` can also overflow a buffer unless a width is supplied.

The museum uses `fgets()` so entire lines can be read with an explicit maximum size.

Input parsing in C is less glamorous than pointers but responsible for at least as many ruined afternoons.

---

# `strtoul()`

The museum parses text using:

```c
strtoul(...)
```

rather than:

```c
atoi(...)
```

`atoi()` provides poor error reporting.

`strtoul()` exposes where parsing stopped, making validation possible.

Serious code would also inspect:

```c
errno
```

for range errors and compare against `UINT_MAX` from `<limits.h>` rather than using a literal upper bound.

The exhibit keeps the parser readable rather than turning passenger-count entry into its own standards committee.

---

# Recursion

The canonical factorial remains recursive:

```c
static bool factorial(unsigned int num, BigUInt *result) {
    if (num <= 1) {
        return biguint_set_one(result);
    }

    if (!factorial(num - 1, result)) {
        return false;
    }

    return biguint_multiply_small(result, num);
}
```

C supports recursion naturally.

Each call gets its own automatic local variables.

But the language does not promise infinite stack space.

A gigantic `num` can overflow the call stack long before arithmetic itself becomes conceptually impossible.

Real factorial code would normally use a loop.

The museum keeps recursion because that is one of the specimen's comparison dimensions.

---

# The big-integer implementation

Our `BigUInt`:

```c
typedef struct {
    unsigned char *digits;
    size_t length;
    size_t capacity;
} BigUInt;
```

has:

- a pointer to heap-allocated digits;
- the number of digits currently in use;
- the amount of allocated space.

This pattern appears constantly in C.

It is the skeleton behind many dynamic containers:

```text
pointer + length + capacity
```

C does not ship a `std::vector`.

You can build one.

That sentence summarizes a remarkable amount of C culture.

---

# Why decimal digits?

A serious big-integer library would usually store chunks in a much larger base, often aligned with machine words.

Our exhibit stores one decimal digit per byte.

That makes multiplication simple:

```c
product = digit * factor + carry;
digit = product % 10;
carry = product / 10;
```

It wastes space and performance.

But you can explain it without opening a 700-page algorithms book.

Museum specimens are allowed to choose transparent bones over racing aerodynamics.

---

# `NULL`

C has historically used:

```c
NULL
```

for a null pointer constant.

C23 additionally introduces the keyword:

```c
nullptr
```

which provides a dedicated null pointer constant with improved type properties.

Much existing C code still uses `NULL`, and older compilers may not yet support every C23 feature.

This is a good example of modern C evolving without instantly erasing fifty years of source code.

---

# Boolean values

Older C code often uses integers as truth values:

```c
0   // false
1   // true
```

C99 introduced `_Bool` and `<stdbool.h>` provided the convenient spelling:

```c
bool
true
false
```

C23 promotes `bool`, `true`, and `false` into the modern language environment rather than relying on the same compatibility mechanism.

The museum includes:

```c
#include <stdbool.h>
```

for broad compiler compatibility.

---

# Arrays and pointer arithmetic

Given:

```c
int values[4] = {10, 20, 30, 40};
```

this:

```c
values[2]
```

is defined in terms closely related to:

```c
*(values + 2)
```

Array indexing and pointer arithmetic are deeply connected in C.

And because addition is commutative:

```c
2[values]
```

is also valid C.

Do not write this.

Knowing you *can* do something and deciding society should endure it are different questions.

---

# Zero-based indexing

C arrays begin at index zero:

```c
values[0]
```

through:

```c
values[3]
```

for a four-element array.

This convention became massively influential.

A considerable fraction of modern programmers now instinctively think the first item is number zero.

Mathematicians nod approvingly.

Spreadsheet users remain unconvinced.

---

# Undefined behaviour

C's most famous sharp edge is **undefined behaviour**.

Some operations fall outside what the standard defines.

Examples include:

- accessing outside an array's bounds;
- dereferencing an invalid pointer;
- using an object after it has been freed;
- signed integer overflow;
- modifying a string literal;
- many invalid type-punning situations.

Once undefined behaviour occurs, the C standard places no requirements on the result.

It does **not** mean:

> the machine will definitely crash.

It means:

> the language contract has ended.

The program may crash.

It may appear to work.

An optimizer may remove code based on the assumption that valid programs do not invoke undefined behaviour.

Your tests passing is therefore not a standards document.

---

# Why does C allow this?

Performance and machine-level flexibility are central to C's design and history.

Bounds checks, garbage collection and dynamic safety metadata would impose semantics that C historically does not require.

This allows implementations to map C operations efficiently onto a huge range of hardware.

The price is that many safety properties are the programmer's responsibility.

Modern languages such as Rust exist partly because people looked at this trade-off and asked whether systems programming could retain low-level performance while enforcing more memory safety statically.

Language evolution is often one long reply thread.

---

# Signed integer overflow

This:

```c
int x = INT_MAX;
x += 1;
```

has undefined behaviour.

By contrast, unsigned integer arithmetic wraps modulo a power of two according to the range of the type.

So:

```c
unsigned int x = UINT_MAX;
x += 1;
```

wraps to zero.

This distinction matters enormously in security-sensitive arithmetic.

"Integer overflow" is not one single behaviour in C.

---

# `char` is weirdly important

`char` is C's smallest ordinary integer type and the unit used for object representation.

But plain:

```c
char
```

may be signed or unsigned depending on the implementation.

If you specifically need one or the other:

```c
signed char
unsigned char
```

For raw object bytes, `unsigned char` has particularly important guarantees.

This is why our decimal digits use:

```c
unsigned char
```

rather than plain `char`.

---

# Strings versus bytes

C does not make a rich distinction between a text string object and arbitrary byte storage at the language level.

A string is a null-terminated sequence of `char`.

Encoding is a separate issue.

Modern C has facilities related to wide and multibyte characters, Unicode-oriented literal prefixes, locale, and newer standard revisions.

But handling Unicode text robustly often relies on external libraries and explicit encoding decisions.

`char *` does not magically mean UTF-8.

It means pointer to char.

Culture is somebody else's header file.

---

# Function pointers

C can store pointers to functions:

```c
int (*operation)(int, int);
```

Then:

```c
operation = add;
result = operation(2, 3);
```

This enables:

- callbacks;
- dispatch tables;
- plugin-style interfaces;
- state machines;
- object-like designs built manually.

The syntax is famously dense.

A typedef can improve things:

```c
typedef int (*BinaryOperation)(int, int);
```

C programmers discovered abstraction after all.

They just make you assemble it yourself.

---

# `void *`

A:

```c
void *
```

is a generic pointer to object storage.

Standard allocation functions such as:

```c
malloc(...)
```

return `void *`.

In C, a `void *` can convert to and from other object pointer types without an explicit cast.

That is why idiomatic C usually writes:

```c
int *values = malloc(count * sizeof *values);
```

rather than casting the `malloc()` result.

The latter habit often comes from C++, where the conversion rules differ.

This is another small but revealing C-versus-C++ split.

---

# Manual polymorphism

C has no classes or virtual methods.

But C programmers can build equivalent patterns using structs and function pointers.

For example:

```c
typedef struct Shape Shape;

struct Shape {
    double (*area)(const Shape *self);
};
```

Then different structures can provide different function implementations.

Many major C libraries use designs like this.

Object-oriented programming is not synonymous with a `class` keyword.

C merely makes you see the scaffolding.

---

# Error handling

C has no exceptions in the C++/Python sense.

Common error patterns include:

- special return values;
- null pointers;
- integer status codes;
- `errno`;
- output parameters.

Example:

```c
FILE *file = fopen("data.txt", "r");

if (file == NULL) {
    perror("data.txt");
}
```

This encourages explicit control flow around failure.

It can also encourage forgetting to check errors.

Again, the language supplies mechanisms rather than a parent.

---

# C and the operating system

Many operating-system APIs expose C interfaces.

On Unix-like systems, calls and libraries use types and conventions directly friendly to C.

Windows exposes enormous C-compatible API surfaces too.

Even higher-level languages frequently communicate with native libraries through a **C ABI**, because C calling conventions and data representations are a common interoperability denominator.

C is less a universal language than a universal electrical adapter.

Possibly with exposed copper.

---

# The standard library

C's standard library is intentionally modest compared with Python, JavaScript runtimes or C++.

It includes facilities for:

- I/O;
- strings and memory;
- allocation;
- mathematics;
- time;
- sorting/searching;
- locale;
- signals;
- atomics;
- threads in supporting implementations;
- numeric conversions.

It does **not** provide standard:

- dynamic arrays;
- hash maps;
- networking sockets;
- GUI widgets;
- JSON;
- arbitrary-precision integers.

Those belong to external libraries or platform APIs.

C gives you a kitchen knife.

Python gives you a supermarket.

---

# C versus C++

C++ began as an extension of C, and substantial source-level similarity remains.

But this:

```c
malloc(...)
```

this:

```c
struct Person
```

this:

```c
void *data
```

and this:

```c
char name[100]
```

live in different idiomatic ecosystems once you cross the language boundary.

Modern C++ tends to prefer:

- constructors/destructors;
- RAII;
- `std::string`;
- `std::vector`;
- references;
- templates;
- smart pointers;
- exceptions or richer result types depending on codebase;
- standard generic algorithms.

Modern C tends toward:

- explicit data structures;
- explicit ownership conventions;
- pointers;
- manual allocation;
- plain function interfaces;
- ABI stability;
- small runtime assumptions.

Neither is simply a strict superset of the other in modern standards.

Some valid C is invalid C++.

Some constructs mean different things.

Some C++ features have no C counterpart at all.

See:

**[`cpp.md`](cpp.md)**

---

# Why C is still used

C remains attractive when developers need:

- minimal runtime assumptions;
- predictable memory layout;
- small binaries;
- direct hardware access;
- embedded environments;
- stable binary interfaces;
- interoperability;
- mature toolchains;
- decades-old portable libraries.

It is also entrenched.

Linux kernel development, for example, is overwhelmingly C-oriented.

A language does not need to win a syntax beauty contest when it already owns the basement.

---

# Security

C's memory model enables entire vulnerability classes:

- buffer overflows;
- use-after-free;
- double-free;
- dangling pointers;
- uninitialized memory use;
- format-string bugs;
- integer overflow leading to memory errors.

Modern toolchains fight back with:

- warnings;
- sanitizers;
- stack protectors;
- control-flow protections;
- static analysis;
- fuzzing;
- hardened allocators;
- safer library APIs.

C did not disappear when safer languages arrived.

Instead, an industrial ecosystem evolved around asking:

> Can we please stop this specific pointer from becoming international news?

---

# Compiler warnings are part of the culture

Compile with warnings:

```sh
gcc -Wall -Wextra -Wpedantic ...
```

or the equivalent for your compiler.

Warnings can catch:

- suspicious conversions;
- unused variables;
- missing declarations;
- format-string mismatches;
- unreachable code;
- questionable control flow.

Good C development treats warnings less like unsolicited criticism and more like the smoke alarm politely mentioning that the curtains are on fire.

---

# Sanitizers

Clang and GCC support runtime instrumentation such as:

```sh
-fsanitize=address,undefined
```

AddressSanitizer can detect many memory errors.

UndefinedBehaviorSanitizer can catch several categories of undefined behaviour.

For debugging C, this can transform:

```text
segmentation fault
```

into:

```text
here is the exact line where your pointer achieved transcendence
```

Use the tools.

---

# The factorial pressure test

Our museum factorial is especially useful in C because it exposes **two separate resource limits**.

## Ordinary machine integers

If we naïvely wrote:

```c
unsigned long long factorial(unsigned int n)
```

then on the common 64-bit case:

```text
20!
```

fits.

```text
21!
```

does not.

That is even earlier than the C++ Boost-backed exhibit's exact arithmetic.

## Our handmade big integer

The custom `BigUInt` can keep growing until:

- memory allocation fails;
- recursion exhausts the call stack;
- or practical time becomes unreasonable.

This is much closer to the pseudocode's conceptual integer model.

But notice what happened:

**we had to implement the representation ourselves.**

C did not fail to compute factorial.

It simply made data representation our problem.

That is extremely C.

---

# Shitpost cabinet

## `2["hello"]`

This is valid C:

```c
"hello"[2]
```

It evaluates to:

```text
'l'
```

Because indexing is defined through pointer arithmetic, this is also valid:

```c
2["hello"]
```

It also evaluates to:

```text
'l'
```

Please leave immediately after the demonstration.

---

## The declaration spiral

```c
int *p;
```

pointer to int.

```c
int (*p)(double);
```

pointer to function taking double and returning int.

```c
int (*p[10])(double);
```

array of ten pointers to functions taking double and returning int.

At some point:

```c
typedef int (*Callback)(double);
```

becomes less a style preference and more a humanitarian intervention.

---

## Segmentation fault

A vast amount of C debugging history can be summarized by:

```text
Segmentation fault (core dumped)
```

The machine has declined to elaborate.

---

# Learn more

Language Museum is not a full C course.

Good starting points:

- [ISO/IEC 9899:2024](https://www.iso.org/standard/82075.html)
- [cppreference: C](https://en.cppreference.com/w/c)
- [cppreference: C23](https://en.cppreference.com/w/c/23)
- [GCC C documentation](https://gcc.gnu.org/onlinedocs/gcc/C-Dialect-Options.html)
- [Clang](https://clang.llvm.org/)

Books and deeper learning:

- *The C Programming Language* by Brian Kernighan and Dennis Ritchie
- *Modern C* by Jens Gustedt

Related museum exhibits:

- [C++](cpp.md)
- [Ghidra's decompiler C](../other/ghidra-c.md)
- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

C takes the pseudocode and strips away a surprising number of assumed conveniences.

There is no built-in:

- string object;
- growable vector;
- garbage collector;
- arbitrary-precision integer;
- exception system;
- record constructor machinery.

Instead we get:

- arrays;
- structs;
- pointers;
- functions;
- explicit sizes;
- explicit allocation;
- explicit cleanup.

The result is not "primitive C++".

It is a language with a different centre of gravity.

C++ asks:

> What abstractions can we build while retaining systems-level performance?

C more often asks:

> What is the smallest portable mechanism I need to express this directly?

Then the museum asks for `50!`, and C quietly hands you `malloc()` and waits to see what kind of programmer you are.
