# Pseudocode

> **Collection:** `other/`  
> **Kind:** Algorithm notation / "language-like thing"  
> **Museum role:** Canonical reference exhibit  
> **Reference convention:** Cambridge International AS & A Level Computer Science (9618) pseudocode

Pseudocode is code that is not quite code.

It is a way of writing an algorithm so that a human can understand its structure without committing to the exact syntax, standard library, compiler, runtime, package manager, build system, dependency graph, or ritual sacrifice demanded by a real programming language.

That makes pseudocode the natural starting point for the **Language Museum**.

Every normal exhibit in this repository ultimately asks the same question:

> How would *this language* express the ideas in the canonical pseudocode program?

If this is your first exhibit, start here.

---

## Is pseudocode a programming language?

Not really.

There is no single universal language called **Pseudocode**, no official compiler that all pseudocode targets, and no one grammar used by every programmer, textbook, school or university.

"Pseudocode" is a family of informal and semi-formal notations used to describe algorithms.

That freedom is useful, but it creates an obvious problem for a museum built around comparing languages: if the reference pseudocode changes style every five minutes, the reference specimen is made of soup.

So Language Museum needs a house dialect.

---

## The museum's pseudocode dialect

Language Museum uses the pseudocode conventions from **Cambridge International AS & A Level Computer Science (9618)** as its reference style.

At the time this exhibit was written, the latest published Cambridge guide was the **2027-2029 Pseudocode Guide**.

Cambridge pseudocode is useful here because it defines a reasonably broad, consistent notation for things including:

- variables and constants;
- primitive data types;
- arrays;
- records and other user-defined types;
- assignment;
- input and output;
- conditionals;
- loops;
- procedures;
- functions;
- parameters;
- comments;
- file handling;
- and some object-oriented programming.

It is still pseudocode, not a real executable programming language. The point is not that Cambridge has discovered the One True Pseudocode. The point is that Language Museum needs a stable reference, and Cambridge gives us one that is documented rather than invented five minutes before lunch.

### A few important conventions

| Idea | Cambridge-style pseudocode |
| --- | --- |
| Declare an integer | `DECLARE Age : INTEGER` |
| Assignment | `Age ← 18` |
| Equality test | `Age = 18` |
| Output | `OUTPUT "Hello"` |
| Input | `INPUT Name` |
| Comment | `// hello, future archaeologist` |
| Conditional | `IF ... THEN` / `ELSE` / `ENDIF` |
| Counted loop | `FOR ... TO ...` / `NEXT` |
| Procedure | `PROCEDURE ...` / `ENDPROCEDURE` |
| Function | `FUNCTION ... RETURNS ...` / `ENDFUNCTION` |
| Return a value | `RETURN Value` |

Notice that assignment uses `←`, while equality uses `=`.

This is a small distinction with an enormous talent for becoming important later.

---

## The canonical Language Museum program

Here it is: the specimen that the rest of the museum gets to translate, reinterpret, fight with, or stare at in horror.

```text
CONSTANT Language = "Pseudocode"
CONSTANT MaxPassengers = 100

TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE

PROCEDURE Greet(Name : STRING)
    OUTPUT "Hi ", Name, "!"
    OUTPUT "I hope you are well, ", Name
    OUTPUT "Nice to meet you, ", Name
ENDPROCEDURE

FUNCTION AgeCheck(Age : INTEGER) RETURNS STRING
    IF Age >= 18 THEN
        RETURN "You may legally drink alcohol here."
    ELSE
        RETURN "No :("
    ENDIF
ENDFUNCTION

FUNCTION Factorial(Num : INTEGER) RETURNS INTEGER
    IF Num <= 1 THEN
        RETURN 1
    ENDIF

    RETURN Num * Factorial(Num - 1)
ENDFUNCTION

DECLARE People : ARRAY[1:MaxPassengers] OF Person
DECLARE NumPassengers : INTEGER
DECLARE Index : INTEGER

OUTPUT "How many people are you going into the bar with? (maximum ", MaxPassengers, ")"
INPUT NumPassengers

FOR Index ← 1 TO NumPassengers
    OUTPUT "What is your name?"
    INPUT People[Index].Name

    CALL Greet(People[Index].Name)

    OUTPUT "How old are you (in human years)?"
    INPUT People[Index].Age

    OUTPUT AgeCheck(People[Index].Age)
NEXT Index

OUTPUT "Fun fact! For everyone here, their ages in factorial would be:"

FOR Index ← 1 TO NumPassengers
    OUTPUT "- ", People[Index].Name, " is ", Factorial(People[Index].Age)
NEXT Index

OUTPUT "Interactive museum entry for ", Language, " completed."
```

For the purposes of the specimen, assume that `NumPassengers` is an integer from `1` to `100`, and that ages are non-negative integers.

The `18` in `AgeCheck` is an example threshold for the fictional bar in the program. It is part of the specimen, not a universal statement about alcohol law.

---

## What does the program actually do?

The program asks how many people are entering a fictional bar.

For each person, it:

1. asks for their name;
2. greets them;
3. asks for their age;
4. performs a simple age check;
5. stores their name and age.

After everyone has been entered, the program calculates the factorial of each person's age and prints the result.

Finally, it announces that the museum entry is complete.

This is intentionally a slightly ridiculous job for a computer.

Nobody has ever stood at the entrance to a bar and thought:

> Before I go inside, I desperately need to know `37!`.

That is why it is here.

---

## Why this program?

A one-line `Hello, World!` program is excellent for proving that a language can print text and considerably less excellent for learning anything else about it.

At the opposite extreme, a full application would drown the comparison in unrelated complexity.

The Language Museum specimen sits in the middle. It is small enough to translate repeatedly, but large enough to make languages reveal their opinions.

It deliberately exercises:

- strings and integers;
- constants and variables;
- input and output;
- parameters;
- procedures;
- functions and return values;
- conditionals;
- arrays or equivalent collections;
- structured data;
- counted iteration;
- recursion;
- comparisons;
- arithmetic;
- and text formatting.

A language does **not** have to implement these concepts using the same mechanism as the pseudocode.

That would defeat half the point.

---

## Translation rules for other exhibits

The pseudocode is the **behavioural reference**, not holy scripture engraved into a silicon tablet.

An exhibit should preserve the important behaviour of the specimen while using reasonably natural constructs for the language being demonstrated.

For example:

- a language with lists may use a list instead of a fixed array;
- a language with dictionaries may store names and ages in a dictionary;
- a language without procedures may use functions;
- a functional language may replace an imperative loop with mapping, folding or recursion;
- a language with algebraic data types may model `Person` differently;
- a language without interactive console input may adapt the interface and explain why;
- an esolang may do something unspeakable.

If a language fundamentally cannot reproduce part of the specimen, the exhibit should explain the mismatch rather than quietly pretending it never happened.

The translation should teach us about the language.

That is the entire point of making the poor thing calculate somebody's age factorial.

---

## Procedures versus functions

Cambridge pseudocode distinguishes between **procedures** and **functions**.

A procedure performs actions but does not return a value:

```text
PROCEDURE Greet(Name : STRING)
    OUTPUT "Hello ", Name
ENDPROCEDURE

CALL Greet("Mika")
```

A function returns a value and is used as part of an expression:

```text
FUNCTION Double(Number : INTEGER) RETURNS INTEGER
    RETURN Number * 2
ENDFUNCTION

OUTPUT Double(5)
```

That is why `Greet` is a procedure while `AgeCheck` and `Factorial` are functions.

Many real programming languages do not make this distinction in the same way. Python, JavaScript and Rust, for example, would normally express both using their ordinary function mechanism.

Excellent. The exhibit already has something to talk about.

---

## Records and arrays

The original rough Language Museum specimen used a dictionary mapping names to ages.

The canonical Cambridge version instead uses an array of `Person` records:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE

DECLARE People : ARRAY[1:100] OF Person
```

This is deliberate.

Cambridge discusses dictionaries as an abstract data type, but its published pseudocode conventions give us direct notation for arrays and records. Using those constructs keeps the reference program close to the documented dialect.

It also avoids treating a name as a unique database key. Two people are, inconveniently for hash maps, allowed to have the same name.

---

## Recursion, or: why are we factorialising humans?

`Factorial` is recursive:

```text
FUNCTION Factorial(Num : INTEGER) RETURNS INTEGER
    IF Num <= 1 THEN
        RETURN 1
    ENDIF

    RETURN Num * Factorial(Num - 1)
ENDFUNCTION
```

Mathematically:

```text
5! = 5 × 4 × 3 × 2 × 1 = 120
```

The function calls itself with a smaller value until it reaches the base case.

This gives the museum a compact way to demonstrate recursion.

It also creates an accidental stress test for number systems.

Factorials become enormous very quickly. A language with arbitrary-precision integers may happily calculate results that overflow the ordinary integer types of another language. A floating-point-only number type may lose integer precision. A language may require a big-integer library.

This is not a bug in the museum specimen.

This is the specimen poking the language with a stick to see what happens.

---

## Pseudocode has no runtime

You cannot meaningfully ask:

```sh
pseudocode museum.psc
```

and expect the universe to cooperate.

Pseudocode is intended for humans. It describes an algorithm without defining all the implementation details necessary for a machine to execute it.

That means questions which have precise answers in real languages can be intentionally unspecified here.

For example:

- How many bits are in an `INTEGER`?
- What exact encoding does a `STRING` use?
- What happens if the user enters text when an integer is expected?
- What is the maximum recursion depth?
- How is memory allocated for `People`?

A real language eventually has to answer questions like these.

Pseudocode gets to point at the algorithm and say, "you know what I mean."

---

## A tiny bit of history

Pseudocode does not have a single inventor, specification or birthday.

It grew out of the broader practice of describing algorithms in notation that resembles programming code while remaining independent of a specific machine language. Different textbooks, institutions and programmers developed their own conventions, and that remains true today.

This is why two pieces of perfectly understandable pseudocode can look noticeably different.

One author might write:

```text
IF age >= 18 THEN
```

while another writes:

```text
if age >= 18:
```

and a third writes something halfway between English, Pascal and whatever they last had open in their editor.

The algorithm can be the same even when the notation is not.

Language Museum standardises its reference version purely so that later comparisons have a fixed starting point.

---

## Things to notice before leaving this exhibit

Pseudocode already exposes several ideas that will mutate dramatically as you move through the museum:

**Types.** Cambridge explicitly names types such as `INTEGER`, `STRING` and `BOOLEAN`. Some real languages require similarly explicit types. Others infer them. Others barely seem interested in your paperwork.

**Blocks.** Cambridge closes structures with words such as `ENDIF`, `NEXT`, `ENDPROCEDURE` and `ENDFUNCTION`. Other languages use braces, indentation, keywords, parentheses, S-expressions, or worse.

**Assignment.** Cambridge uses `←`. Real languages variously use `=`, `:=`, `<-`, `let`, pattern binding, mutation operators, or concepts that make the word "assignment" start sweating.

**Collections.** Our reference uses an array of records. Later exhibits may naturally use lists, vectors, tuples, maps, objects, tables, structs, cons cells or home-grown data structures.

**Execution.** Pseudocode describes what should happen. A real implementation has to make it happen.

That gap is where a lot of the museum lives.

---

## Learn more

Language Museum is not a full pseudocode course. If you want the exact conventions used by this exhibit, read the primary source:

- [Cambridge International AS & A Level Computer Science (9618)](https://www.cambridgeinternational.org/programmes-and-qualifications/cambridge-international-as-and-a-level-computer-science-9618/)
- [Cambridge 2027-2029 Pseudocode Guide](https://www.cambridgeinternational.org/Images/721401-2027-2029-pseudocode-guide.pdf)
- [Pseudocode on Wikipedia](https://en.wikipedia.org/wiki/Pseudocode)

---

## Next exhibit

Once you understand the reference specimen, you can go almost anywhere in the museum.

If you want to immediately abandon the comforting world where instructions have reasonably crisp meanings, continue to:

**[AI Prompt](ai-prompt.md)**

There, the "code" is natural language, the runtime is a generative model, and `please do this exactly` is less of a command than programmers may initially hope.
