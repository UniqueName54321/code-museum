# INTERCAL

> **Collection:** `recreational/`  
> **Kind:** Esoteric programming language / programming-language parody  
> **Full name:** Compiler Language With No Pronounceable Acronym  
> **Created:** 1972  
> **Created by:** Donald R. Woods and James M. Lyon  
> **Place:** Princeton University  
> **Original implementation:** IBM System/360, via SPITBOL  
> **Museum implementation:** primarily C-INTERCAL / INTERCAL-72 concepts  
> **File extension:** commonly `.i`  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)  
> **Natural predator:** comprehensibility

INTERCAL is what happens when two programmers finish their exams, look at the programming languages of 1972, and decide the problem is that computers have been treated with too much dignity.

It is one of the earliest and most influential **esoteric programming languages**.

Unlike languages designed to be:

- readable;
- efficient;
- elegant;
- portable;
- productive;
- safe;
- pleasant;

INTERCAL was deliberately designed to have **as little as possible in common with existing languages**.

The original manual says exactly that.

The result contains:

- variables named with punctuation and numbers;
- deliberately strange operators;
- `PLEASE`;
- `IGNORE`;
- `FORGET`;
- `ABSTAIN`;
- `REINSTATE`;
- `NEXT`;
- `RESUME`;
- numeric input whose digits are written as English words;
- numeric output in mutilated Roman numerals;
- and, in later implementations, the immortal `COME FROM`.

This is not a programming language that accidentally became hostile.

The hostility is artisanal.

---

# Did COBOL cause INTERCAL?

**Sort of, but not by itself.**

The original INTERCAL manual says the goal was to create a compiler language with nothing at all in common with any other major language the authors knew.

It explicitly lists:

```text
FORTRAN
BASIC
COBOL
ALGOL
SNOBOL
SPITBOL
FOCAL
SOLVE
TEACH
APL
LISP
PL/I
```

So yes:

**COBOL is literally one of the languages INTERCAL defined itself against.**

But it is inaccurate to say INTERCAL was specifically a parody of COBOL alone.

Don Woods later clarified that they were spoofing the programming languages and reference manuals of the era generally. He even recalled that he himself had never really learned COBOL, although James Lyon knew it.

So the family tree is approximately:

```text
1960s programming-language culture
├── FORTRAN
├── COBOL
├── BASIC
├── ALGOL
├── APL
├── LISP
├── PL/I
├── many others
│
└── two Princeton students at an unreasonable hour
    └── INTERCAL
```

COBOL therefore has INTERCAL to thank for preserving its dignity by comparison.

---

# A tiny bit of history

INTERCAL was designed on the morning of **26 May 1972** by Donald Woods and James Lyon at Princeton.

The original manual describes its founding ambition as having nothing at all in common with existing major programming languages.

The first implementation ran on an IBM 360 and was implemented using SPITBOL.

The implementation itself did not remain widely available, but the manual survived and circulated as a computing in-joke.

Then, in 1990, Eric S. Raymond created **C-INTERCAL**, reviving the language and translating INTERCAL through C.

Later implementations and dialects followed, including **CLC-INTERCAL**.

INTERCAL therefore achieved an impressive feat:

It started as a joke, became historical documentation, and then people put genuine engineering effort into making the joke more portable.

---

# The name

INTERCAL officially expands to:

> **Compiler Language With No Pronounceable Acronym**

This does not explain why that phrase abbreviates to `INTERCAL`.

That is the point.

Don Woods later recalled that the name **INTERCAL** probably came first because it sounded plausibly language-like, with the expansion invented afterwards.

Acronym design has peaked.

Everyone else can go home.

---

# Is INTERCAL the first esolang?

It is commonly described as the **first major or first widely recognised esoteric programming language**.

The modern term **esolang** came later.

INTERCAL predates Brainfuck, Befunge, Malbolge and most of the genre by decades.

Its basic creative move became foundational:

> A programming language does not have to be designed for practical software development. The language itself can be a joke, puzzle, artwork, experiment or critique.

That idea eventually grew into an entire ecosystem of languages whose primary output is the expression:

```text
what the fuck
```

INTERCAL got there early.

---

# First impressions

A conventional assignment might look like:

```text
x = 10
```

INTERCAL gives us:

```intercal
DO .1 <- #10
```

That is one of the *normal* lines.

The sigils matter.

---

# Variables have punctuation species

Classic INTERCAL has several fundamental variable families.

## Onespot

```intercal
.1
```

A `.` variable is a **16-bit unsigned scalar**.

The manual calls the `.` a **spot**.

---

## Twospot

```intercal
:1
```

A `:` variable is a **32-bit unsigned scalar**.

Naturally, `:` is a **twospot**.

---

## Tail

```intercal
,1
```

A `,` variable is an array of 16-bit values.

The comma is called a **tail**.

---

## Hybrid

```intercal
;1
```

A `;` variable is an array of 32-bit values.

The semicolon is called a **hybrid**.

---

The identifier after the punctuation is numeric.

So:

```intercal
.1
.2
:1
,1
;1
```

are all perfectly respectable variable names by INTERCAL standards.

This is the point where JavaScript developers should apologise to `foo`.

---

# Constants wear a mesh

An integer constant is prefixed by:

```text
#
```

which INTERCAL terminology calls a **mesh**.

So:

```intercal
#1
#18
#65535
```

are constants.

Why not just write `18`?

Because then a programmer might understand what was happening too quickly.

---

# Assignment

INTERCAL assignment uses:

```intercal
<-
```

For example:

```intercal
DO .1 <- #18
```

This is one of the very few places where the language accidentally resembles ordinary pseudocode.

The original manual more or less apologises for the conventionality.

---

# `DO`

Statements commonly begin with:

```intercal
DO
```

For example:

```intercal
DO .1 <- #5
```

But INTERCAL also offers:

```intercal
PLEASE
```

and:

```intercal
PLEASE DO
```

These can act as polite statement identifiers.

Which brings us to manners.

---

# You must be polite, but not *too* polite

C-INTERCAL expects a reasonable proportion of statements to use `PLEASE`.

Too few polite statements can produce an error complaining that the programmer is insufficiently polite.

Too many can produce an error complaining that the programmer is excessively polite.

The manual describes the expected balance as roughly three non-polite identifiers for each polite one.

So this is not merely syntactic sugar.

The compiler performs etiquette review.

Most languages have linters.

INTERCAL has a finishing school.

---

# Input and output are backwards

Classic INTERCAL uses:

```intercal
WRITE IN
```

for **input**.

And:

```intercal
READ OUT
```

for **output**.

You write information **into** the computer.

You read information **out of** the computer.

This is logically defensible.

It is also perfectly engineered to make anyone arriving from another language walk into a door.

---

# Numeric input is hostile

INTERCAL-72 numeric input is entered using the English names of digits.

To input:

```text
123
```

you enter something like:

```text
ONE TWO THREE
```

This is not a typo.

The language has successfully made typing numbers more verbose than typing prose.

---

# Numeric output is even better

Classic numeric `READ OUT` uses **butchered Roman numerals**.

Yes.

The language can accept decimal digits written as English words and answer in mutated Roman notation.

At last, a fully integrated enterprise solution for the decline of the Western Roman Empire.

---

# Text I/O somehow gets worse

C-INTERCAL later added character-oriented I/O.

A famous C-INTERCAL `Hello, world!` looks roughly like this:

```intercal
DO ,1 <- #13
PLEASE DO ,1 SUB #1 <- #238
DO ,1 SUB #2 <- #108
DO ,1 SUB #3 <- #112
DO ,1 SUB #4 <- #0
DO ,1 SUB #5 <- #64
DO ,1 SUB #6 <- #194
DO ,1 SUB #7 <- #48
PLEASE DO ,1 SUB #8 <- #22
DO ,1 SUB #9 <- #248
DO ,1 SUB #10 <- #168
DO ,1 SUB #11 <- #24
DO ,1 SUB #12 <- #16
DO ,1 SUB #13 <- #162
PLEASE READ OUT ,1
PLEASE GIVE UP
```

The array does not simply contain ASCII character codes.

C-INTERCAL's text model tracks changes around a conceptual circular character tape and involves bit reversal.

`Hello, world!` has become cryptography performed by someone who hates both cryptography and you.

---

# `SUB`

Array subscripting uses:

```intercal
SUB
```

For example:

```intercal
,1 SUB #4
```

means an element of tail array `,1`.

Compared with the rest of the language, the literal English word `SUB` is almost suspiciously helpful.

Do not become accustomed to it.

---

# Arithmetic is not something you simply *have*

You may have noticed that ordinary arithmetic operators are conspicuously absent.

Classic INTERCAL's primitive operators are bizarre bit operations such as **mingle** and **select**.

Useful arithmetic such as addition and multiplication can be obtained through the system library.

For example, the standard library entry:

```intercal
DO (1030) NEXT
```

multiplies `.1` by `.2` and places the result in `.3`, failing on overflow.

Read that again.

To multiply two ordinary small integers, one traditional INTERCAL solution is:

> Call magic line number 1030.

The library exists partly because straightforward arithmetic is inconvenient to express in the core language.

This is excellent language design if your design objective is revenge.

---

# The canonical Language Museum program

Now we reach a problem.

The canonical museum specimen expects:

- arbitrary human names;
- ordinary string output;
- an array of person records;
- comparisons;
- looping;
- recursion;
- factorials large enough for realistic ages.

Classic INTERCAL offers:

- no normal string type;
- text I/O deliberately encoded through arrays;
- 16-bit and 32-bit unsigned integer scalars;
- arrays of those integers;
- unusual control flow;
- no ordinary built-in multiplication operator;
- 32-bit arithmetic in the classic system library.

A literal line-for-line translation would therefore stop being an educational exhibit and become a small research project.

**That is itself the result.**

The museum's rule says that if a language fundamentally cannot reproduce the reference naturally, the exhibit should explain the mismatch rather than quietly replacing the language with something else.

So INTERCAL gets an **adapted canonical specimen**.

It preserves the algorithmic spine while deliberately dropping arbitrary names and exact huge factorials.

---

# The adapted INTERCAL specimen

The INTERCAL version asks for:

1. the number of patrons;
2. each patron's age;
3. whether each patron passes the age threshold;
4. a factorial when that factorial fits INTERCAL's 32-bit arithmetic.

Names are omitted from the compact specimen because representing arbitrary text cleanly would dominate the entire program with C-INTERCAL's character-I/O encoding.

Factorial is limited to `12!` because:

```text
12! = 479001600
```

fits in an unsigned 32-bit value, while:

```text
13! = 6227020800
```

does not.

And even this "simplified" version still requires far more control machinery than the equivalent pseudocode.

Rather than pretending the result is ordinary, the museum presents the important core building blocks separately.

---

## Input a number

```intercal
PLEASE WRITE IN .1
```

This asks for a 16-bit numeric value.

Remember that classic INTERCAL expects digits written as words.

So if the intended value is:

```text
18
```

the user supplies:

```text
ONE EIGHT
```

We are off to a strong start.

---

## Output a number

```intercal
DO READ OUT .1
```

This prints the number.

Naturally, classic output is not ordinary decimal.

You wanted a number.

INTERCAL has opinions about typography.

---

## Multiply two values

```intercal
DO .1 <- #6
DO .2 <- #7
PLEASE DO (1030) NEXT
```

After the library call:

```intercal
.3
```

contains:

```text
42
```

assuming it did not overflow.

In another language:

```text
6 * 7
```

In INTERCAL:

```text
please visit subroutine district 1030
```

---

## Add one

The system library also provides:

```intercal
DO (1020) NEXT
```

which increments `.1`.

The fact that "add one" has a magic subroutine address should prepare you psychologically for the remaining language.

---

# Why the full museum specimen is not printed here

It *can* be built.

C-INTERCAL is computationally capable.

A determined programmer could implement:

- text buffers for names;
- encoded text input/output;
- multidimensional arrays representing patrons;
- custom comparisons;
- custom arbitrary-precision arithmetic;
- factorial over that arbitrary-precision representation;
- the entire interaction state machine.

But then the exhibit would teach:

> Here are 900 lines of one deliberately hostile program.

instead of:

> Here is how INTERCAL works and why the translation is hostile.

The Language Museum values understanding over ritual suffering.

INTERCAL itself has already cornered the ritual-suffering market.

---

# A more honest mapping

| Museum concept | INTERCAL equivalent |
| --- | --- |
| Integer | onespot `.n` or twospot `:n` |
| Array | tail `,n` or hybrid `;n` |
| Record | manually encode parallel arrays / layout yourself |
| String | no ordinary classic string type |
| Constant | `#n` |
| Assignment | `<-` |
| Input | `WRITE IN` |
| Output | `READ OUT` |
| Multiplication | system-library `NEXT`, e.g. `(1030)` |
| Normal loop | control-flow construction; bring snacks |
| Function call | commonly `NEXT` / system-library conventions |
| Return | `RESUME` |
| Ignore something | literally `IGNORE`, or abstain from it |
| Stop program | `GIVE UP` |
| Sanity | implementation-defined |

---

# `NEXT` is not "next"

INTERCAL's:

```intercal
DO (1000) NEXT
```

does not simply proceed to the next statement.

`NEXT` transfers control to a labelled statement while saving information on the **NEXT stack** so execution can later return via `RESUME`.

It therefore behaves more like a deeply peculiar subroutine-control mechanism.

The fact that it is named `NEXT` means ordinary English has once again been weaponised.

---

# `RESUME`

A called INTERCAL routine can return using:

```intercal
DO RESUME #1
```

The numeric argument affects how many saved `NEXT` contexts are discarded.

This makes control flow depend on a stack of saved continuation-like destinations.

You wanted a function.

You have received a small temporal bureaucracy.

---

# `FORGET`

The `NEXT` stack can be altered with:

```intercal
FORGET
```

This discards saved control information.

In many languages, forgetting where you were is a bug.

In INTERCAL, it has syntax.

---

# `IGNORE` and `REMEMBER`

INTERCAL can tell variables to ignore assignments.

Conceptually:

```intercal
DO IGNORE .1
```

causes later attempted changes to `.1` to be ignored.

Then:

```intercal
DO REMEMBER .1
```

makes assignments effective again.

This allows a programming technique rarely advertised by mainstream languages:

> Make the assignment statement happen, but have the variable refuse to participate.

---

# `ABSTAIN` and `REINSTATE`

INTERCAL can disable classes of statements or particular statements:

```intercal
ABSTAIN
```

and later enable them again:

```intercal
REINSTATE
```

So control flow can involve instructions that remain physically present in the program but are temporarily non-participating.

The program is not skipping lines.

The lines are on administrative leave.

---

# Comments can be syntax errors that never execute

Modern C-INTERCAL has one of the best comment mechanisms in programming history.

Negated statement identifiers such as:

```intercal
PLEASE NOT
```

create statements that are initially abstained from.

Because INTERCAL can defer certain syntax errors until execution, text after an abstained identifier can function as a comment even if that text is not otherwise valid syntax.

For example, the manual discusses forms conceptually like:

```intercal
PLEASE NOTE: THIS IS A COMMENT
```

where the important part is that the statement is not executed.

Other languages:

```text
ignore this text
```

INTERCAL:

```text
this may technically be nonsense, but we have arranged for reality not to inspect it
```

---

# `COME FROM`

The most famous INTERCAL control structure is:

```intercal
DO COME FROM (10)
```

Rather than saying:

```text
GOTO destination
```

at the place you leave, `COME FROM` sits at the destination and says:

> Whenever execution reaches that other place, come here.

It is the anti-`GOTO`.

This produces control flow that is beautifully hostile to local reasoning.

---

# Important historical correction: original INTERCAL did not have `COME FROM`

This is one of the best pieces of INTERCAL archaeology.

People frequently describe `COME FROM` as though Woods and Lyon included it in the original 1972 language.

They did not.

**INTERCAL-72 did not contain `COME FROM`.**

C-INTERCAL later implemented it.

The idea of `COME FROM` also existed independently in programming humour around the same era.

So the most famous INTERCAL feature is not actually one of the language's original features.

INTERCAL has managed to be historically confusing in addition to being syntactically confusing.

On brand.

---

# Mingle and select

Classic INTERCAL's primitive operators include bit-manipulation operations with intentionally unusual behaviour.

One famous operator is **mingle**, historically represented using the cent sign:

```text
¢
```

and often represented as:

```text
$
```

in ASCII-oriented implementations.

It interleaves bits from operands.

There is also **select**, written using `~`, which selects bits according to a mask-like operand.

These operations are not merely named strangely.

They are foundational enough that normal arithmetic can feel like something implemented on top of the language rather than part of its basic vocabulary.

---

# The character names

INTERCAL documentation gives punctuation its own vocabulary.

Examples include names such as:

- `.` → spot
- `:` → twospot
- `,` → tail
- `;` → hybrid
- `#` → mesh
- `'` → spark
- `"` → rabbit ears

This has an important pedagogical advantage:

Nobody can accuse the manual of using familiar terminology accidentally.

---

# `GIVE UP`

Program termination is:

```intercal
PLEASE GIVE UP
```

COBOL has:

```cobol
STOP RUN.
```

INTERCAL has:

```intercal
PLEASE GIVE UP
```

These two languages accidentally understand the emotional reality of debugging better than most modern runtime designers.

---

# Runtime syntax errors

C-INTERCAL can defer some errors until the offending statement actually executes.

This interacts with abstention and comments in deeply cursed ways.

A piece of invalid-looking source may therefore survive compilation because execution never activates it.

The compiler has not proven your program valid.

It has merely agreed not to open that cupboard yet.

---

# Random compiler bugs, as a feature

INTERCAL's mythology famously includes deliberately bizarre behaviour and diagnostics.

C-INTERCAL even provides options relating to compatibility with historical deliberate-bug behaviour.

Most programming languages consider compiler bugs unfortunate.

INTERCAL looked at that convention and detected dangerous levels of normality.

---

# Overflow arrives extremely early

Classic INTERCAL has:

- 16-bit unsigned scalars;
- 32-bit unsigned scalars.

Our factorial specimen detonates even harder here than it did in JavaScript or COBOL.

```text
12! = 479001600
```

fits in 32 bits.

```text
13! = 6227020800
```

does not fit in an unsigned 32-bit value.

So a thirteen-year-old already exceeds classic INTERCAL's straightforward machine-integer factorial range.

The museum's bar has found a programming language in which **children are numerically too old**.

That sentence is going in the gift shop.

---

# No, `PLEASE` does not make the language "English-like"

COBOL:

```cobol
MULTIPLY N BY SUBRESULT GIVING RESULT
```

INTERCAL:

```intercal
PLEASE DO (1030) NEXT
```

Both contain English words.

This is where the similarity ends and files for divorce.

---

# C-INTERCAL

The most famous revived implementation is **C-INTERCAL**.

Eric S. Raymond created it in 1990.

It translates INTERCAL into C and then uses a C compiler to build the executable.

The compiler command is traditionally:

```sh
ick program.i
```

Yes.

The compiler is called:

```text
ick
```

This is one of the rare compiler names that doubles as an accurate review.

---

# Strict INTERCAL-72 mode

C-INTERCAL can enforce stricter compatibility with the original language.

For example, its command-line options include a strict mode that rejects later extensions such as `COME FROM`.

This distinction matters because "INTERCAL" has accumulated dialects and extensions over decades.

An exhibit that uses `COME FROM` is not demonstrating pure INTERCAL-72.

The museum labels the jar correctly.

---

# CLC-INTERCAL

Another major implementation family is **CLC-INTERCAL**.

It introduced and experimented with many additional features and dialect behaviour.

Modern INTERCAL culture is therefore not one perfectly fixed language.

That would be much too straightforward.

There are versions, extensions and compatibility questions.

Even parody languages eventually acquire standards archaeology.

---

# Why INTERCAL matters

INTERCAL is funny, but it is not *only* funny.

It demonstrates that language design itself can be expressive.

A programming language can deliberately question assumptions such as:

- Why should arithmetic syntax be familiar?
- Why should control flow be local?
- Why should input and output use obvious names?
- Why should source code optimise for readability?
- Why should a programming language even optimise for usefulness?

Later esolangs explored other dimensions:

- minimalism;
- geometric source layouts;
- self-modifying programs;
- linguistic constraints;
- visual aesthetics;
- computation through absurd metaphors;
- deliberately impossible compilation.

INTERCAL helped establish the cultural space in which those experiments make sense.

It is programming-language satire that accidentally became programming-language history.

---

# INTERCAL versus COBOL

This pairing is especially good because their goals are almost mirror images.

COBOL wanted business programs to be:

- portable;
- explicit;
- readable;
- stable;
- suitable for serious organisations.

INTERCAL wanted to have:

- nothing in common with contemporary languages;
- difficult syntax;
- hostile conventions;
- deliberately comic documentation;
- approximately zero interest in your payroll department.

COBOL:

```cobol
ADD 1 TO COUNTER
```

INTERCAL:

```intercal
DO (1020) NEXT
```

COBOL describes the business operation.

INTERCAL describes your decline.

See:

**[`../languages/cobol.md`](../languages/cobol.md)**

---

# Shitpost cabinet

This entire exhibit is the shitpost cabinet.

Nevertheless:

## Compiler etiquette

Imagine submitting code review feedback:

> Build failed because author was not sufficiently polite to compiler.

INTERCAL made that a language feature.

---

## `COME FROM`

Ordinary debugging:

> Where can execution go from here?

INTERCAL debugging:

> More importantly, what completely unrelated statement has declared that execution will arrive there from here?

---

## The compiler is `ick`

```sh
ick program.i
```

No notes.

---

## Official termination syntax

```intercal
PLEASE GIVE UP
```

For once, documentation that understands the developer.

---

# Learn more

Language Museum is not an INTERCAL tutorial.

This may be the first time that sentence has functioned as a safety notice.

Primary and important resources:

- [The INTERCAL Resources Page](https://www.catb.org/esr/intercal/)
- [C-INTERCAL Revamped Instruction Manual](https://www.catb.org/~esr/intercal/ick.htm)
- [C-INTERCAL README](https://www.catb.org/~esr/intercal/README.html)
- [INTERCAL implementor's notes](https://www.catb.org/~esr/intercal/THEORY.html)

Historical/manual material:

- [The INTERCAL Programming Language Reference Manual](https://www.cs.virginia.edu/~asb/teaching/cs415-fall05/docs/intercal.pdf)
- [The A-Z of Programming Languages: INTERCAL interview](https://a-z.readthedocs.io/en/latest/intercal.html)
- [INTERCAL on Esolang](https://esolangs.org/wiki/INTERCAL)

Related exhibit:

- [COBOL](../languages/cobol.md)
- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

INTERCAL is the first exhibit where the museum's translation framework itself starts visibly smoking.

The canonical specimen assumes ordinary concepts such as:

```text
name
age
array of people
if
loop
function
multiply
print text
```

INTERCAL responds with:

```text
.1
:1
,1
ABSTAIN
NEXT
RESUME
WRITE IN
READ OUT
PLEASE
magic system-library line numbers
```

That failure to map neatly is not a failure of the museum.

It is one of the clearest demonstrations of what the museum is for.

Most languages answer:

> How would I express this algorithm?

INTERCAL answers:

> Why would you assume expression should be pleasant?

Then it prints the answer in Roman numerals and asks you to be more polite.
