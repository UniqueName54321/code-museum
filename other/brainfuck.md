# Brainfuck

> **Collection:** `other/`  
> **Kind:** Esoteric programming language / Turing tarpit  
> **Created:** 1993  
> **Created by:** Urban Müller  
> **Original platform:** Commodore Amiga / AmigaOS  
> **Commands:** 8  
> **File extensions:** commonly `.b` and `.bf`  
> **Museum target:** Classic eight-command Brainfuck semantics, with implementation differences explicitly called out  
> **Reference exhibit:** [`pseudocode.md`](pseudocode.md)  
> **Related recreational exhibit:** [`../recreational/intercal.md`](../recreational/intercal.md)  
> **Natural predator:** abstraction

Brainfuck is what happens when a programming language sees all the conveniences accumulated over decades of computer science and says:

> no thanks, I have a pointer

A Brainfuck program operates on a row of memory cells.

You can:

- move left;
- move right;
- increment a cell;
- decrement a cell;
- read one value;
- write one value;
- loop while the current cell is nonzero.

That is almost the entire language.

There are **eight commands**.

Not eight keywords.

Not eight statement types.

Eight individual characters.

And yet, given the appropriate theoretical memory model, Brainfuck can perform arbitrary computation.

This is less a demonstration that Brainfuck is powerful and more a demonstration that **Turing completeness is an alarmingly low bar for usability**.

---

# Is Brainfuck actually a programming language?

**Yes. Absolutely.**

This distinguishes it from several residents of `other/`.

Pseudocode is algorithm notation.

AI prompts are natural-language instructions interpreted probabilistically.

Ghidra's C-like output is a reconstruction generated from machine code.

Brainfuck is an honest-to-goodness executable programming language.

A source file such as:

```brainfuck
+++++[>+++++++++++++<-]>.
```

has defined computational behaviour.

An interpreter can execute it.

A compiler can translate it.

People have written:

- games;
- interpreters;
- compilers;
- mathematical programs;
- graphics generators;
- quines;
- Brainfuck interpreters written in Brainfuck;
- and assorted acts of punctuation-based self-harm.

The fact that doing so is spectacularly inconvenient does not make the language less real.

It merely explains the name.

---

# A tiny bit of history

Brainfuck was created by **Urban Müller** in **1993**.

Its origin is closely tied to an old programming-language hobby:

> How small can a compiler possibly be?

Müller was influenced by **FALSE**, another tiny esoteric programming language whose implementation demonstrated that a surprisingly capable language could have a very small compiler.

Brainfuck pushed this idea considerably further.

An early compiler occupied only a few hundred bytes.

A later version distributed by Müller was just:

```text
240 bytes
```

That is not the size of a Brainfuck program.

That is the compiler.

For perspective, this Markdown paragraph describing the compiler is substantially larger than the compiler.

The original package also contained:

- an interpreter;
- example programs;
- arithmetic routines;
- a prime-number program;
- a `Hello World`;
- and small reusable Brainfuck idioms.

The language has since escaped its original Amiga habitat and been reimplemented approximately everywhere a programmer has had fifteen spare minutes and poor impulse control.

---

# The entire language

Here it is.

The complete instruction set.

| Command | Meaning |
| --- | --- |
| `>` | Move the data pointer one cell to the right |
| `<` | Move the data pointer one cell to the left |
| `+` | Increment the current cell |
| `-` | Decrement the current cell |
| `.` | Output the current cell |
| `,` | Read input into the current cell |
| `[` | If the current cell is zero, jump past the matching `]` |
| `]` | If the current cell is nonzero, jump back to the matching `[` |

Everything else is ignored.

That is it.

There is no hidden chapter where we introduce:

```text
variables
functions
classes
strings
exceptions
modules
objects
records
integers
booleans
for loops
switch statements
imports
return
```

The museum has already given you the whole language.

We may now spend the rest of the exhibit discovering how much suffering eight characters can contain.

---

# The machine model

A conventional Brainfuck machine looks roughly like this:

```text
          data pointer
               |
               v
+-----+-----+-----+-----+-----+-----+-----+
|  0  |  0  |  0  |  0  |  0  |  0  | ... |
+-----+-----+-----+-----+-----+-----+-----+
 cell0 cell1 cell2 cell3 cell4 cell5
```

There are two important moving parts:

1. an **instruction pointer**, which moves through the Brainfuck program;
2. a **data pointer**, which moves along the memory tape.

The source code tells the data pointer what to do.

For example:

```brainfuck
+++
```

leaves the first cell containing:

```text
3
```

Then:

```brainfuck
>
```

moves to the next cell.

So:

```brainfuck
+++>+++++
```

produces:

```text
pointer
   |
   v
+-----+-----+-----+
|  3  |  5  |  0  |
+-----+-----+-----+
```

Actually, after that exact program the pointer is on the second cell:

```text
      pointer
         |
         v
+-----+-----+-----+
|  3  |  5  |  0  |
+-----+-----+-----+
```

Congratulations.

We have invented variables by remembering where things are.

---

# The classic memory layout

Müller's original compiler used:

```text
30,000 cells
```

initialized to zero.

Classic Brainfuck is therefore often described as having a tape something like:

```text
cell[0]
cell[1]
cell[2]
...
cell[29999]
```

with the pointer initially at:

```text
cell[0]
```

However, this is where Brainfuck develops its first important archaeological problem.

There is **no single modern specification implemented identically everywhere**.

Different interpreters disagree about things such as:

- how many cells exist;
- whether the tape grows automatically;
- whether cells are 8, 16, 32 or more bits wide;
- whether integers wrap on overflow;
- what happens if the pointer moves left of the first cell;
- what happens if the pointer moves beyond available memory;
- what `,` does at end-of-file.

So:

```brainfuck
+
```

is straightforward.

But sufficiently strange Brainfuck can behave differently depending on the interpreter.

The language has eight instructions and still found room for portability problems.

Respect.

---

# About that Turing-complete claim

Brainfuck is commonly described as **Turing-complete**.

There is an important footnote.

A literal machine with exactly:

```text
30,000 cells
```

each containing exactly:

```text
8 bits
```

has only a finite number of possible states.

A genuinely Turing-complete theoretical machine requires unbounded storage.

Even Müller's original documentation acknowledged this qualification when discussing the language's computational universality.

So when computer scientists say:

> Brainfuck is Turing-complete

the model usually assumes that memory can grow without a fixed finite bound, or otherwise supplies an equivalent source of unbounded storage.

This distinction matters because Brainfuck is an excellent demonstration of two separate ideas:

```text
tiny instruction set
        ≠
weak computational model
```

and:

```text
Turing-complete
        ≠
pleasant
```

The second lesson may be the more useful one.

---

# First impressions

Suppose we want to add three to a value.

A normal language might write:

```python
value += 3
```

Brainfuck writes:

```brainfuck
+++
```

Okay.

Not bad.

Suppose we want to move to the next variable.

A normal language might write:

```python
other_value
```

Brainfuck writes:

```brainfuck
>
```

Still manageable.

Suppose we now want:

```text
an array of 100 records
each containing a variable-length human name
and an arbitrary non-negative age
then recursively calculate exact factorials
and print them as decimal text
```

Brainfuck quietly closes the museum door behind us.

---

# Loops

The only built-in control-flow structure is:

```brainfuck
[
]
```

Conceptually:

```brainfuck
[
    stuff
]
```

means something similar to:

```text
WHILE current_cell != 0
    stuff
ENDWHILE
```

For example:

```brainfuck
+++[-]
```

starts with:

```text
3
```

and repeatedly decrements the cell:

```text
3
2
1
0
```

Once it reaches zero, the loop ends.

Therefore:

```brainfuck
[-]
```

is one of the most famous Brainfuck idioms.

It means:

> set the current cell to zero

There is no assignment operator.

So instead of:

```text
x = 0
```

we write:

```brainfuck
[-]
```

The language has already started developing its own higher-level vocabulary entirely out of punctuation.

---

# Moving values around

Suppose the tape contains:

```text
pointer
   |
   v
+-----+-----+
|  5  |  0  |
+-----+-----+
```

We can move the value to the next cell:

```brainfuck
[->+<]
```

Watch what happens.

Each loop:

1. subtracts one from the current cell;
2. moves right;
3. adds one;
4. moves left.

Eventually:

```text
+-----+-----+
|  0  |  5  |
+-----+-----+
```

The first cell has been destroyed in the process.

If we need to copy rather than move, we need temporary cells and another restoration loop.

This is how Brainfuck programming works.

Concepts that would normally be language primitives become **algorithms over memory cells**.

---

# There are no integers

This statement sounds strange because obviously the cells contain numbers.

But Brainfuck does not define an `INTEGER` type in the sense used by the pseudocode exhibit.

A cell is just a machine value.

If your interpreter uses wrapping 8-bit cells, one cell can represent:

```text
0 through 255
```

But whether that represents:

```text
255
```

or:

```text
-1
```

or:

```text
'ÿ'
```

or part of a much larger number

is your program's problem.

Want an unsigned 64-bit integer?

Reserve several cells and implement one.

Want arbitrary precision?

Reserve however many cells you need and implement:

- carrying;
- addition;
- subtraction;
- multiplication;
- comparison;
- decimal conversion.

Python gives you enormous integers automatically.

C makes you choose a representation.

Brainfuck gives you a row of tiny boxes and wishes you luck.

---

# There are no strings

Brainfuck also has no built-in string type.

This:

```text
"hello"
```

does not exist as a language primitive.

To represent text, a program typically stores character values in consecutive cells or generates their values when needed.

Output happens one cell at a time using:

```brainfuck
.
```

Therefore even:

```text
OUTPUT "Hello"
```

means constructing the character codes for:

```text
H
e
l
l
o
```

and emitting them individually.

The machine does not know that the cells form a word.

It knows that numbers happened and then `.` happened.

Human civilization supplies the rest.

---

# `.` does not mean "print this number"

This is one of the first traps.

Suppose the current cell contains:

```text
65
```

Then:

```brainfuck
.
```

normally outputs the character represented by that byte.

Under ASCII-compatible text conventions:

```text
65
```

is:

```text
A
```

So Brainfuck's output model is closer to:

```c
putchar(cell);
```

than:

```python
print(cell)
```

If the cell contains:

```text
37
```

then:

```brainfuck
.
```

does **not** print:

```text
37
```

It outputs the character with code 37:

```text
%
```

If you want to print the decimal digits:

```text
37
```

you must implement integer-to-decimal conversion yourself.

Because of course you do.

---

# `,` does not mean "input an integer"

The same problem happens in reverse.

```brainfuck
,
```

reads an input unit into the current cell.

If the user types:

```text
37
```

the program does not magically receive the integer thirty-seven.

It receives character data corresponding to:

```text
'3'
'7'
```

usually followed by some form of newline.

A program that wants the number `37` therefore needs to:

1. read `'3'`;
2. convert it from its character encoding to the numeric value `3`;
3. multiply the running result by ten;
4. read `'7'`;
5. convert it to `7`;
6. add it;
7. detect the end of the number.

We have not yet begun the actual application.

We are still trying to understand what the user typed.

---

# Hello World

The traditional Brainfuck rite of passage looks something like this:

```brainfuck
++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+.+++++++..+++.>++.<<+++++++++++++++.>.+++.------.--------.>+.>.
```

Output:

```text
Hello World!
```

followed by a newline.

At first glance this resembles somebody losing a fight with their keyboard.

There is structure in it.

The first section:

```brainfuck
++++++++++
```

places `10` in the first cell.

Then:

```brainfuck
[
    >+++++++
    >++++++++++
    >+++
    >+
    <<<<-
]
```

uses that value as a loop counter to construct several useful character values in neighbouring cells.

After ten iterations the tape contains values close to the ASCII codes needed for the message.

The remainder tweaks those values and emits characters using `.`.

Brainfuck programmers therefore spend significant effort exploiting nearby character values.

After printing:

```text
l
```

for example, it can be cheaper to modify that cell slightly to obtain another character than to rebuild a new character from zero.

Text output becomes arithmetic choreography.

---

# A much smaller program

The smallest interesting input/output program is:

```brainfuck
,.
```

That means:

```text
read one input unit
output that input unit
```

Congratulations.

We built `cat` for one byte.

A repeated echo program can be written using a loop, although the exact safest form depends on what the interpreter does when input reaches EOF.

And that brings us to portability.

---

# EOF is a whole thing

What should this do when there is no more input?

```brainfuck
,
```

Reasonable answers include:

- set the current cell to zero;
- set it to `-1` or the cell's wrapped equivalent;
- leave the cell unchanged.

Different interpreters have historically chosen different behaviours.

That matters enormously for programs such as:

```brainfuck
,[.,]
```

which attempt to read and echo data until a zero-like termination condition occurs.

Brainfuck is minimal enough that one tiny implementation decision can substantially change program behaviour.

There are only eight commands.

We are litigating one eighth of the language.

---

# Cell width matters too

Consider:

```brainfuck
+[+]
```

On a wrapping 8-bit implementation:

1. the first `+` produces `1`;
2. the loop repeatedly increments the cell;
3. eventually `255 + 1` wraps to `0`;
4. the loop terminates.

On an implementation with unbounded positive integers:

```text
1
2
3
4
5
...
```

never reaches zero.

The program runs forever.

Same source.

Different machine model.

Completely different result.

That is why serious Brainfuck programs often document assumptions such as:

```text
8-bit wrapping cells
30,000 cells
zero on EOF
```

The source language is tiny.

The execution environment is where the dragons moved.

---

# Comments are hilarious

Brainfuck ignores characters other than:

```text
> < + - . , [ ]
```

This means you can write:

```text
hello world
```

inside a Brainfuck source file.

The interpreter sees:

```text
nothing
```

Every character is ignored.

That sounds like comments.

There is a catch.

This:

```text
hello world.
```

contains:

```brainfuck
.
```

And `.` is an instruction.

So the sentence is no longer merely a comment.

It outputs the current cell.

Likewise:

```text
remember - this is important
```

contains:

```brainfuck
-
```

which decrements the current cell.

And:

```text
array[index]
```

contains:

```brainfuck
[
]
```

which the interpreter sees as loop syntax.

Therefore Brainfuck technically has comments in the delightful sense that:

> every character is a comment except the eight characters that absolutely are not.

You can write annotations.

Just inspect your punctuation like it owes you money.

---

# There are no variables

Suppose another language has:

```python
age = 22
count = 5
temporary = 0
```

A Brainfuck programmer might decide:

```text
cell 0 = age
cell 1 = count
cell 2 = temporary
```

The names:

```text
age
count
temporary
```

exist only in the programmer's head, comments, documentation or tooling.

The running program sees:

```text
cell 0
cell 1
cell 2
```

And because accessing another "variable" means moving the pointer:

```brainfuck
>>
```

the **physical layout of values becomes part of the algorithm**.

If two values frequently interact, placing them close together can reduce source length and execution time.

Variable allocation has become urban planning.

---

# There are no functions

Brainfuck has no built-in:

```text
FUNCTION
RETURN
CALL
PROCEDURE
```

Reusable routines can be simulated in several ways:

- copy the same instruction sequence;
- use preprocessing or macros outside the core language;
- create control structures using cells as state;
- manually implement call-stack-like mechanisms.

But none of these are ordinary built-in function calls.

This matters enormously for the canonical museum specimen because the pseudocode contains:

```text
Greet(...)
AgeCheck(...)
Factorial(...)
```

In Brainfuck those abstractions do not simply translate into different syntax.

They disappear.

Their behaviour has to be reconstructed out of tape manipulation.

---

# There are no records

The canonical pseudocode has:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE
```

Brainfuck responds:

```text
>
```

If we want records, we invent a memory layout.

For example, perhaps:

```text
person 0
+------+-----+-----+-----+-----+-----+
| age  | n0  | n1  | n2  | n3  | ... |
+------+-----+-----+-----+-----+-----+

person 1
+------+-----+-----+-----+-----+-----+
| age  | n0  | n1  | n2  | n3  | ... |
+------+-----+-----+-----+-----+-----+
```

Now we must decide:

- maximum name length;
- string terminators;
- record width;
- where the next record begins;
- how ages are encoded;
- how indexes are converted into pointer movement.

Nothing prevents this.

Nothing helps either.

---

# Arrays are simultaneously built in and nonexistent

This one is fun.

Brainfuck's tape is already an indexed sequence of cells.

So in one sense the language is practically **made of an array**.

In another sense, it has no operation corresponding to:

```text
People[Index]
```

There is only:

```brainfuck
>
>
>
<
<
```

To dynamically access an element by numeric index, the program has to implement pointer traversal using loops and temporary cells.

So Brainfuck gives you an enormous array and then declines to give you an array subscript operator.

---

# Comparisons

The canonical program asks:

```text
Age >= 18
```

Normal languages generally have operators such as:

```text
<
>
<=
>=
==
!=
```

Brainfuck has none.

It has one primitive condition:

```text
is the current cell zero?
```

That condition only exists as part of:

```brainfuck
[
]
```

Everything else must be derived.

To compare values, Brainfuck algorithms typically:

- copy operands;
- subtract values;
- track underflow or exhaustion;
- use extra cells as flags;
- restore values if necessary.

Even Boolean logic becomes memory choreography.

This sounds ridiculous.

It is also an excellent way to discover how much machinery higher-level languages normally hide.

---

# Arithmetic

Brainfuck directly supplies:

```brainfuck
+
-
```

for increment and decrement.

That is the arithmetic instruction set.

Addition of two arbitrary cells?

Loop one value into another.

Multiplication?

Repeated addition.

Division?

Enjoy your afternoon.

Modulo?

See previous answer.

Exponentiation?

Perhaps bring food.

Arbitrary-precision factorial?

Tell your family you love them.

---

# Factorial has entered the building

The museum's canonical program deliberately computes:

```text
Age!
```

This already exposes interesting numeric differences among ordinary languages.

Python:

```text
integers grow automatically
```

JavaScript:

```text
use BigInt
```

C++:

```text
bring a multiprecision library
```

C:

```text
build or import a big integer representation
```

COBOL:

```text
state exactly how many decimal digits you require
```

Brainfuck:

```text
what is an integer
```

For small values, factorial can be implemented using repeated multiplication across cells.

But human ages become enormous very quickly.

For example:

```text
18! = 6402373705728000
```

That already requires far more than one byte.

A Brainfuck implementation producing exact factorials for arbitrary realistic ages therefore needs a multi-cell integer representation.

Then it needs:

- multi-cell multiplication;
- carrying;
- decimal conversion;
- output formatting.

The recursion used in the pseudocode is almost beside the point.

Before reproducing the algorithm, Brainfuck must first build the numeric civilization in which that algorithm can live.

---

# The canonical Language Museum specimen

Now we reach the museum stress test.

The reference program expects:

- constants;
- strings;
- integers;
- records;
- arrays;
- variable-length human input;
- procedures;
- functions;
- comparisons;
- counted loops;
- recursion;
- multiplication;
- exact factorials;
- decimal number formatting.

Brainfuck natively provides:

```text
>
<
+
-
.
,
[
]
```

A fully equivalent implementation is theoretically possible under a sufficiently capable memory model.

That does **not** mean a giant wall of punctuation would be a useful educational translation.

The museum's rule is that an exhibit should reveal how the language thinks rather than smuggle an entire runtime into the page and pretend nothing unusual happened.

So Brainfuck gets the same treatment as other languages whose natural model clashes violently with the reference specimen:

**we examine exactly where the translation explodes.**

---

# Pseudocode versus Brainfuck

| Pseudocode concept | Brainfuck equivalent |
| --- | --- |
| Named variable | Reserved tape cell or region |
| Integer | Programmer-defined cell representation |
| String | Sequence of character-valued cells |
| Constant | Value reconstructed or preserved in cells |
| Record | Manually designed tape layout |
| Array | Tape region plus manually implemented indexing |
| Assignment | Clear/copy/move loops |
| Addition | Repeated `+`, or transfer loops |
| Multiplication | Algorithm built from loops |
| Comparison | Algorithm using subtraction, flags and loops |
| `IF` | Conditional loop patterns |
| `WHILE` | `[` and `]` |
| Counted `FOR` | Counter cell plus loop |
| Function | No direct equivalent |
| Procedure | No direct equivalent |
| Recursion | Manually implemented stack/state machine |
| Input string | Repeated `,` plus storage and termination handling |
| Output string | Repeated `.` after constructing character values |
| Input integer | Decimal parser |
| Output integer | Decimal formatter |
| Arbitrary integer | Multi-cell arithmetic library |
| `Person` | Memory-layout agreement |
| Happiness | implementation-defined |

---

# An adapted museum specimen

A useful Brainfuck exhibit should demonstrate the core machine without pretending that manually building a complete text-processing runtime is ordinary Brainfuck programming.

So here is a tiny relative of the canonical greeting.

It:

1. constructs the characters needed for `Hi `;
2. reads one character as the visitor's "name";
3. outputs that character;
4. prints `!`;
5. prints a newline.

```brainfuck
++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+++++.>++.>>,.<<+.>.
```

If the visitor enters:

```text
X
```

the output is:

```text
Hi X!
```

This is drastically less capable than:

```text
Greet(Name)
```

from the canonical specimen.

That is intentional.

The missing capability is the exhibit.

---

# Dissecting the adapted specimen

Start with:

```brainfuck
++++++++++
```

Cell zero becomes:

```text
10
```

Then:

```brainfuck
[>+++++++>++++++++++>+++>+<<<<-]
```

runs ten times.

Each iteration distributes values into neighbouring cells.

After the loop, the useful cells contain approximately:

```text
cell 1 = 70
cell 2 = 100
cell 3 = 30
cell 4 = 10
```

Now:

```brainfuck
>++.
```

turns `70` into:

```text
72
```

and outputs:

```text
H
```

Then:

```brainfuck
>+++++.
```

turns `100` into:

```text
105
```

and outputs:

```text
i
```

Then:

```brainfuck
>++.
```

turns `30` into:

```text
32
```

and outputs a space.

Next:

```brainfuck
>>,.
```

moves to a spare cell, reads one character, and immediately outputs it.

Finally:

```brainfuck
<<+.>.
```

reuses existing cells to construct:

```text
!
```

and:

```text
newline
```

Nothing in the program says:

```text
H
i
!
```

Those characters emerge from arithmetic.

---

# Why not translate the whole specimen literally?

Because the result would teach the wrong lesson.

A giant generated Brainfuck program could absolutely implement:

```text
names
ages
records
factorials
decimal I/O
```

But most of the source would represent machinery that languages such as Python already provide.

Instead of comparing:

```text
how Python represents a Person
```

with:

```text
how Brainfuck represents a Person
```

we would mostly be comparing Python with a bespoke virtual machine and standard library that happened to be written in Brainfuck.

The interesting observation is simpler:

> Brainfuck has almost no abstractions above the raw tape machine.

Everything the canonical specimen assumes must therefore be constructed manually.

That is the comparison.

---

# Brainfuck is a Turing tarpit

A **Turing tarpit** is a computational system that is theoretically capable of arbitrary computation but makes useful programs unnecessarily difficult because the available primitives are extremely low-level.

Brainfuck is one of the canonical examples.

This gives us a useful distinction:

```text
expressive power
```

is not the same thing as:

```text
expressiveness
```

Two languages may both be able to compute the same set of computable functions.

Yet one might let you write:

```python
people.append(Person(name, age))
```

while another asks you to decide where the third byte of the second person's name lives relative to your multiplication scratch cells.

Computability theory considers those machines equivalent in an important sense.

Software engineers may nevertheless develop preferences.

---

# Minimal language, enormous programs

Brainfuck relocates complexity.

A complicated language may have:

- hundreds of syntax rules;
- elaborate type systems;
- modules;
- generic programming;
- object systems;
- pattern matching;
- async runtimes;
- enormous standard libraries.

Brainfuck has eight commands.

That does not delete complexity.

It transfers complexity from:

```text
the language implementation
```

into:

```text
the programs
```

This is one reason tiny languages are so educational.

They demonstrate that abstraction is not decorative.

Abstraction is how humans stop every software project from turning into a custom memory-management religion.

---

# Brainfuck interpreters are wonderfully small

Because there are only eight commands, a basic Brainfuck interpreter is extremely straightforward to implement.

In rough pseudocode:

```text
FOR each instruction
    CASE instruction
        ">" : pointer ← pointer + 1
        "<" : pointer ← pointer - 1
        "+" : memory[pointer] ← memory[pointer] + 1
        "-" : memory[pointer] ← memory[pointer] - 1
        "." : output memory[pointer]
        "," : input memory[pointer]

        "[" :
            IF memory[pointer] = 0
                jump to matching "]"
            ENDIF

        "]" :
            IF memory[pointer] != 0
                jump to matching "["
            ENDIF
    ENDCASE
NEXT
```

The most substantial bookkeeping task is efficiently matching:

```brainfuck
[
]
```

pairs.

A naïve interpreter can search for the matching bracket whenever it encounters a loop.

A better interpreter can scan the source first and build a jump table.

From there, optimizers can become surprisingly sophisticated.

---

# Optimizing punctuation

Consider:

```brainfuck
++++++++++
```

A naïve interpreter executes ten separate increment instructions.

An optimizer can recognize:

```text
add 10
```

Likewise:

```brainfuck
>>>>>
```

can become:

```text
move pointer right 5
```

And:

```brainfuck
[-]
```

can often become:

```text
set cell to zero
```

More advanced optimizers can recognize transfer loops such as:

```brainfuck
[->+<]
```

and translate them into higher-level arithmetic operations.

So an optimizing Brainfuck compiler may internally transform:

```text
a dense punctuation swamp
```

into something resembling a conventional intermediate representation.

The source language has almost no abstractions.

The compiler quietly puts some back.

---

# Brainfuck as an intermediate language

Because Brainfuck is tiny, it is also popular as a target for experiments.

People have written compilers from larger languages into Brainfuck.

At that point the situation becomes gloriously backwards:

```text
nice source language
        ↓
compiler
        ↓
Brainfuck
        ↓
Brainfuck compiler
        ↓
machine code
```

This is not usually an efficient route to production software.

It is, however, an excellent demonstration that:

```text
ugly target language
```

does not mean:

```text
incapable target language
```

Compiler construction contains many such crimes against aesthetics.

---

# Dialects and extensions

Because there is no single central Brainfuck standard, implementations have accumulated variations and extensions.

Possible differences include:

- tape size;
- cell size;
- wrapping behaviour;
- pointer bounds;
- EOF semantics;
- debugging commands;
- alternate input models;
- optimization assumptions.

Some implementations also recognize additional characters for debugging or metaprogramming.

Portable Brainfuck therefore means deciding what subset of behaviour you actually expect.

The core eight commands are extremely stable.

The universe around them is less so.

---

# Brainfuck derivatives

Brainfuck has inspired an entire ecological disaster of derivative languages.

Some merely replace the eight commands with different tokens.

Others change the machine model.

Still others preserve the computational structure while making the syntax even more ridiculous.

A famous example is **Ook!**, which represents Brainfuck-like operations through combinations of:

```text
Ook.
Ook?
Ook!
```

because somebody correctly identified that Brainfuck's biggest usability problem was insufficient orangutan representation.

Other derivatives replace the commands with:

- words;
- syllables;
- emoji;
- Pokémon names;
- animal noises;
- whitespace;
- increasingly questionable jokes.

Because Brainfuck's instruction set is so small, creating a trivial syntactic substitution is easy.

The difficult part is resisting the temptation.

Programming-language history suggests resistance is uncommon.

---

# Things Brainfuck makes easy

This is a short section.

Brainfuck makes several things genuinely elegant:

## Writing an interpreter

There are only eight instructions.

The implementation model is excellent for teaching:

- parsing;
- instruction pointers;
- memory models;
- loop matching;
- interpreters;
- simple compilation;
- optimization.

## Understanding a tiny abstract machine

The relationship between source and machine state is unusually direct.

Every command visibly changes:

- the data pointer;
- the current cell;
- input/output;
- or control flow.

## Code golf and puzzles

The constrained instruction set creates an interesting optimization game.

A shorter Brainfuck program often requires clever:

- constant generation;
- cell reuse;
- memory layout;
- loop construction;
- arithmetic tricks.

## Discovering why abstractions exist

Spend enough time implementing decimal output from scratch and suddenly:

```python
print(number)
```

looks less like syntax and more like a humanitarian achievement.

---

# Things Brainfuck makes less easy

Most other things.

Particularly:

- strings;
- structured data;
- random access;
- named variables;
- numeric parsing;
- numeric formatting;
- multiplication;
- division;
- comparisons;
- reusable functions;
- recursion;
- dynamic allocation;
- maintaining someone else's code;
- maintaining your own code after lunch.

This is not a design failure.

The inconvenience is largely the point.

---

# Brainfuck versus INTERCAL

Both are esoteric languages.

They are hostile in very different directions.

INTERCAL contains deliberate absurdity:

```text
PLEASE
ABSTAIN
REINSTATE
COME FROM
```

Its syntax and semantics parody programming-language conventions.

Brainfuck instead pursues **extreme minimalism**.

INTERCAL asks:

> How bizarre can a programming language become?

Brainfuck asks:

> How little language can we get away with?

INTERCAL attacks comprehensibility through excess weirdness.

Brainfuck attacks comprehensibility through starvation.

One gives you an instruction called `COME FROM`.

The other refuses to give you multiplication.

Different methodologies.

Same museum wing.

---

# Brainfuck versus C

There is an interesting relationship here because the original Brainfuck description maps its operations neatly onto C-like actions.

Conceptually:

| Brainfuck | C-like idea |
| --- | --- |
| `>` | `pointer++` |
| `<` | `pointer--` |
| `+` | `(*pointer)++` |
| `-` | `(*pointer)--` |
| `.` | output `*pointer` |
| `,` | input into `*pointer` |
| `[` | `while (*pointer) {` |
| `]` | `}` |

This makes Brainfuck feel almost like somebody extracted:

```text
one pointer
one array
one while loop
byte I/O
```

from C and declared the remaining language unnecessary paperwork.

The difference is that C then gives you:

- expressions;
- functions;
- types;
- structs;
- declarations;
- operators;
- libraries;
- pointer arithmetic involving actual values rather than repeated punctuation.

Brainfuck gives you the conceptual skeleton and leaves you to regrow the organs.

For the proper C exhibit, see:

[`../languages/c.md`](../languages/c.md)

---

# Brainfuck versus pseudocode

The canonical pseudocode is designed to maximize human understanding.

Brainfuck minimizes machine concepts.

Pseudocode says:

```text
DECLARE People : ARRAY[1:MaxPassengers] OF Person
```

Brainfuck says:

```text
you have cells
```

Pseudocode says:

```text
RETURN Num * Factorial(Num - 1)
```

Brainfuck says:

```text
you have cells
```

Pseudocode says:

```text
OUTPUT "Nice to meet you, ", Name
```

Brainfuck says:

```text
you have cells
```

There is a pattern.

---

# Shitpost cabinet

## The complete vocabulary

English:

```text
hundreds of thousands of words
```

C++:

```text
many keywords, operators, library facilities and several geological strata of history
```

Brainfuck:

```text
><+-.,[]
```

Brainfuck programmers:

```text
yeah that's enough
```

---

## `[-]`

This:

```brainfuck
[-]
```

means:

```text
set current cell to zero
```

Brainfuck does not have assignment.

It therefore invented assignment culturally.

---

## Comments

Programmer:

```text
This is a helpful comment.
```

Brainfuck notices:

```brainfuck
.
```

Brainfuck:

```text
OUTPUT BYTE
```

The period betrayed you.

---

## Valid source code

This is technically a valid Brainfuck program:

```text
hello world
```

It does nothing.

Every character is ignored.

This means the language can contain arbitrarily beautiful prose as long as the author remains terrified of eight pieces of punctuation.

---

## The portability speedrun

```brainfuck
+[+]
```

8-bit wrapping cells:

```text
eventually terminates
```

unbounded positive cells:

```text
see you at the heat death of the universe
```

Eight instructions.

Still managed to have implementation-dependent landmines.

---

## Hello World

Other language:

```python
print("Hello World!")
```

Brainfuck:

```brainfuck
++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+.+++++++..+++.>++.<<+++++++++++++++.>.+++.------.--------.>+.>.
```

The computer understands both.

Only one of us has been inconvenienced.

---

## Variable naming

Python:

```python
customer_age = 22
```

C:

```c
int customer_age = 22;
```

Brainfuck:

```text
the seventh cell from that other cell I promised myself I would remember
```

Documentation has become RAM for the programmer.

---

## Debugging

Normal debugger:

```text
customer_age = 22
passenger_count = 4
index = 2
```

Brainfuck debugger:

```text
0 72 105 33 10 88 0 0 0 0 0 0 0
            ^
```

You know what you did.

Probably.

---

## Eight commands

Brainfuck:

```text
I only have eight commands.
```

Programmer:

```text
wow, easy language!
```

Brainfuck:

```text
I said eight commands. I did not say eight problems.
```

---

# Learn more

## Inside the museum

* [`pseudocode.md`](pseudocode.md) — the deliberately human-readable reference language Brainfuck spends this entire exhibit violently disagreeing with
* [`ghidra-c.md`](ghidra-c.md) — another low-level-looking representation, although Ghidra C is reconstructed output rather than an actual source language
* [`../languages/c.md`](../languages/c.md) — pointers, arrays and byte-level operations, except with the radical luxury of names and expressions
* [`../languages/cpp.md`](../languages/cpp.md) — what happens when the evolutionary tree takes almost the exact opposite approach to language size
* [`../languages/python.md`](../languages/python.md) — useful for appreciating just how much machinery hides behind something as innocent as `print(number)`
* [`../recreational/intercal.md`](../recreational/intercal.md) — another foundational esolang, hostile to the programmer for almost completely opposite reasons

## Outside the museum

* [brainfuck — Esolang Wiki](https://esolangs.org/wiki/Brainfuck) — extensive documentation of the language, implementations, computational properties, examples, derivatives and historical quirks
* [Yet another brainfuck reference — brainfuck.org](https://www.brainfuck.org/brainfuck.html) — a detailed reference concentrating on the language itself and the differences between the original model and later implementations
* [Urban Müller's original `brainfuck-2` distribution — Aminet](https://aminet.net/package/dev/lang/brainfuck-2) — the actual 1993 Amiga package, including the famous 240-byte compiler, interpreter, source code and example programs
* [Brainfuck — Wikipedia](https://en.wikipedia.org/wiki/Brainfuck) — general history, instruction-set overview and background on the language's influence
* [The Epistle to the Implementors — brainfuck.org](https://brainfuck.org/epistle.html) — an entertainingly thorough discussion of the annoying edge cases that appear when you try to implement a language that supposedly has only eight instructions

If you want to go directly to the archaeological specimen rather than merely reading about it, **the Aminet archive is the especially fun one**. That is not a recreation of Müller's original package. That is the package.


---

# Museum summary

Brainfuck takes the canonical Language Museum specimen and reveals just how many assumptions that specimen normally gets for free.

The pseudocode casually expects:

- named variables;
- integers;
- strings;
- arrays;
- records;
- input parsing;
- formatted output;
- comparisons;
- procedures;
- functions;
- multiplication;
- recursion;
- arbitrary numeric growth.

Brainfuck provides:

```text
a tape
a pointer
eight commands
```

Everything else is architecture.

That is what makes Brainfuck more than merely a joke language.

It is an unusually pure demonstration of abstraction.

A normal language might let us say:

```python
print(f"{person.name} is {factorial(person.age)}")
```

Brainfuck forces us to ask what every piece of that sentence actually requires a machine to do.

What is a string?

What is an integer?

How is decimal input parsed?

How is multiplication implemented?

Where does a variable live?

How do we remember multiple people?

How does control flow know where to return?

How does a number larger than one memory cell work?

Those questions normally hide beneath layers of language and library design.

Brainfuck removes the layers.

Then it removes the floor.

Then it hands you:

```text
><+-.,[]
```

and asks whether you still believe programming languages are mostly syntax.