# COBOL

> **Collection:** `languages/`  
> **Kind:** General-purpose business-oriented programming language  
> **Name:** Common Business-Oriented Language  
> **Paradigm:** Primarily imperative / procedural, with later object-oriented additions  
> **First developed:** 1959  
> **First version:** 1960  
> **Standard:** ISO/IEC 1989:2023  
> **File extensions:** commonly `.cob`, `.cbl`, `.cpy` for copybooks  
> **Museum implementation:** GnuCOBOL-compatible free-format COBOL  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)

COBOL is one of those languages that people repeatedly declare dead while it continues processing money behind their backs.

It was designed around **business data processing**: records, files, reports, transactions, decimal arithmetic and programs intended to survive longer than several of the companies using them.

COBOL source is famously English-like:

```cobol
IF AGE >= 18
    DISPLAY "You may legally drink alcohol here."
ELSE
    DISPLAY "No :("
END-IF
```

It is verbose by modern standards.

That is not an accident.

One of COBOL's original goals was to make programs comparatively readable to people working with business systems rather than requiring everything to be expressed in compact mathematical notation.

Whether COBOL has achieved "readable" or merely invented **legal-document programming** is left as an exercise for the visitor.

---

# Why does COBOL exist?

In the 1950s, computers from different manufacturers were extremely incompatible.

A business program written for one machine could be difficult or expensive to move to another.

In 1959, a group called **CODASYL**, the Conference on Data Systems Languages, formed with support from the United States Department of Defense and worked on a common business programming language.

That language became COBOL.

The design was heavily influenced by earlier business languages, especially **FLOW-MATIC**, associated with Grace Hopper.

The first COBOL specification appeared in 1960.

A central goal was portability: business logic should not have to be rewritten from scratch every time an organisation changed hardware.

That ambition eventually made COBOL one of the most successful long-lived programming languages ever created.

---

# Grace Hopper and the "inventor of COBOL" problem

Grace Hopper is frequently described as the **inventor of COBOL**.

That is an oversimplification.

COBOL was designed by a **committee**, not by one person.

Hopper was enormously influential because her earlier work, especially FLOW-MATIC, helped establish the idea of English-like business programming languages and directly influenced COBOL's design.

So:

> "Grace Hopper invented COBOL."

is too simple.

But:

> "Grace Hopper was one of the most important influences on COBOL."

is absolutely fair.

The language itself emerged from CODASYL's collaborative work.

---

# The current standard

COBOL did not freeze in 1960.

It has been revised repeatedly.

Important standard generations include:

- COBOL 68
- COBOL 74
- COBOL 85
- COBOL 2002
- COBOL 2014
- COBOL 2023

The current published international standard is:

**ISO/IEC 1989:2023**

Modern COBOL therefore contains features that somebody maintaining a 1970s payroll program would not recognise.

The museum nevertheless concentrates on the procedural core because that is both historically characteristic and widely understandable across implementations.

---

# First impressions

A tiny COBOL program can look like this:

```cobol
>>SOURCE FORMAT FREE

IDENTIFICATION DIVISION.
PROGRAM-ID. HELLO.

PROCEDURE DIVISION.
    DISPLAY "Hello, world!".
    STOP RUN.
END PROGRAM HELLO.
```

Already we have several concepts that look very unlike Python or JavaScript.

COBOL programs are traditionally organised into **divisions**.

The major divisions are:

```text
IDENTIFICATION DIVISION
ENVIRONMENT DIVISION
DATA DIVISION
PROCEDURE DIVISION
```

Not every program needs meaningful contents in every division, but this large-scale organisation is fundamental to COBOL's style.

---

# The four divisions

## `IDENTIFICATION DIVISION`

Describes the program itself.

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. LANGUAGE-MUSEUM.
```

Historically this division could contain considerable documentary metadata.

Modern compilers do not require you to reenact an entire filing cabinet.

---

## `ENVIRONMENT DIVISION`

Describes aspects of the computing environment, especially files and external resources.

Our museum program does not need it.

A traditional file-heavy business application very much might.

---

## `DATA DIVISION`

Defines the program's data.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.

01 AGE PIC 9(3).
01 NAME PIC X(40).
```

COBOL's data description system is one of the language's defining features.

---

## `PROCEDURE DIVISION`

Contains executable behaviour.

```cobol
PROCEDURE DIVISION.
    DISPLAY "Hello".
    STOP RUN.
```

This is where the algorithm lives.

---

# The canonical Language Museum program

There is one important numerical problem before we begin.

The museum's factorial grows *extremely* quickly.

COBOL's ordinary numeric fields have declared finite sizes. GnuCOBOL, the implementation used for this exhibit, supports numeric fields up to 38 decimal digits.

That means a field large enough for 38 digits can hold:

```text
33!
```

but not:

```text
34!
```

So this exhibit does something deliberately visible:

- ages from `0` through `33` receive an exact factorial;
- ages above `33` report that the value exceeds this implementation's chosen COBOL numeric field.

This is a real language difference, not something to hide.

The recursive structure is retained.

```cobol
>>SOURCE FORMAT FREE

IDENTIFICATION DIVISION.
PROGRAM-ID. LANGUAGE-MUSEUM.

DATA DIVISION.
WORKING-STORAGE SECTION.

01 LANGUAGE-NAME             PIC X(5) VALUE "COBOL".
01 MAX-PASSENGERS            PIC 9(3) VALUE 100.
01 NUM-PASSENGERS            PIC 9(3).
01 PERSON-INDEX              PIC 9(3).

01 PEOPLE.
   05 PERSON-ENTRY OCCURS 100 TIMES.
      10 PERSON-NAME         PIC X(40).
      10 PERSON-AGE          PIC 9(3).

01 FACTORIAL-RESULT          PIC 9(38).

PROCEDURE DIVISION.
MAIN.
    DISPLAY
        "How many people are you going into the bar with? "
        "(maximum " MAX-PASSENGERS ")"
    ACCEPT NUM-PASSENGERS

    PERFORM VARYING PERSON-INDEX FROM 1 BY 1
        UNTIL PERSON-INDEX > NUM-PASSENGERS

        DISPLAY "What is your name?"
        ACCEPT PERSON-NAME(PERSON-INDEX)

        PERFORM GREET

        DISPLAY "How old are you (in human years)?"
        ACCEPT PERSON-AGE(PERSON-INDEX)

        PERFORM AGE-CHECK
    END-PERFORM

    DISPLAY
        "Fun fact! For everyone here, "
        "their ages in factorial would be:"

    PERFORM VARYING PERSON-INDEX FROM 1 BY 1
        UNTIL PERSON-INDEX > NUM-PASSENGERS

        DISPLAY "- " FUNCTION TRIM(PERSON-NAME(PERSON-INDEX))
            WITH NO ADVANCING
        DISPLAY " is " WITH NO ADVANCING

        IF PERSON-AGE(PERSON-INDEX) > 33
            DISPLAY
                "[too large for this implementation's "
                "38-digit numeric field]"
        ELSE
            CALL "FACTORIAL"
                USING
                    BY CONTENT PERSON-AGE(PERSON-INDEX)
                    BY REFERENCE FACTORIAL-RESULT

            DISPLAY FUNCTION TRIM(FACTORIAL-RESULT)
        END-IF
    END-PERFORM

    DISPLAY
        "Interactive museum entry for "
        FUNCTION TRIM(LANGUAGE-NAME)
        " completed."

    STOP RUN.

GREET.
    DISPLAY "Hi "
        FUNCTION TRIM(PERSON-NAME(PERSON-INDEX))
        "!"
    DISPLAY "I hope you are well, "
        FUNCTION TRIM(PERSON-NAME(PERSON-INDEX))
    DISPLAY "Nice to meet you, "
        FUNCTION TRIM(PERSON-NAME(PERSON-INDEX)).

AGE-CHECK.
    IF PERSON-AGE(PERSON-INDEX) >= 18
        DISPLAY "You may legally drink alcohol here."
    ELSE
        DISPLAY "No :("
    END-IF.

END PROGRAM LANGUAGE-MUSEUM.


IDENTIFICATION DIVISION.
PROGRAM-ID. FACTORIAL RECURSIVE.

DATA DIVISION.

LOCAL-STORAGE SECTION.
01 NEXT-N                     PIC 9(3).
01 SUBRESULT                  PIC 9(38).

LINKAGE SECTION.
01 N                          PIC 9(3).
01 RESULT                     PIC 9(38).

PROCEDURE DIVISION USING N RESULT.
    IF N <= 1
        MOVE 1 TO RESULT
    ELSE
        COMPUTE NEXT-N = N - 1

        CALL "FACTORIAL"
            USING
                BY CONTENT NEXT-N
                BY REFERENCE SUBRESULT

        MULTIPLY N BY SUBRESULT GIVING RESULT
    END-IF

    GOBACK.

END PROGRAM FACTORIAL.
```

A typical GnuCOBOL compile command is:

```sh
cobc -x -free museum.cob
```

Then, on a Unix-like system:

```sh
./museum
```

The precise executable name and commands vary by platform.

---

# Wait, why is the program enormous?

Welcome to COBOL.

But the extra length is not random.

The source explicitly describes:

- the program;
- its data layout;
- the table of people;
- each field's size;
- the executable procedure;
- the recursive factorial subprogram;
- how arguments are passed.

COBOL strongly favours visible structure.

A three-character variable declaration has never been its spiritual objective.

---

# `PIC`: COBOL describes the *shape* of data

This:

```cobol
01 PERSON-AGE PIC 9(3).
```

does more than say:

> age is an integer.

`PIC`, short for **PICTURE**, describes the format of the field.

```text
9
```

means a decimal digit.

So:

```cobol
PIC 9(3)
```

describes three decimal digit positions.

Similarly:

```cobol
PIC X(40)
```

describes forty alphanumeric character positions.

This data-centric approach makes sense when you remember COBOL's natural habitat:

**business records**.

---

# Level numbers

COBOL data declarations use level numbers:

```cobol
01 PEOPLE.
   05 PERSON-ENTRY OCCURS 100 TIMES.
      10 PERSON-NAME PIC X(40).
      10 PERSON-AGE  PIC 9(3).
```

The numbers describe hierarchy.

Conceptually:

```text
PEOPLE
└── PERSON-ENTRY
    ├── PERSON-NAME
    └── PERSON-AGE
```

The exact numbers do not have to be consecutive.

This:

```text
01
05
10
```

is conventional and leaves room to insert additional levels later.

COBOL has been doing structured record layouts since before JSON's grandparents were born.

---

# Records before records were cool

Our pseudocode has:

```text
TYPE Person
    DECLARE Name : STRING
    DECLARE Age : INTEGER
ENDTYPE
```

COBOL expresses the same idea structurally:

```cobol
05 PERSON-ENTRY OCCURS 100 TIMES.
   10 PERSON-NAME PIC X(40).
   10 PERSON-AGE  PIC 9(3).
```

COBOL's powerful record descriptions are one reason it became so useful for business systems.

Banking, insurance, payroll and government processing are full of highly structured records.

COBOL speaks that language fluently.

---

# `OCCURS`: arrays, COBOL edition

This:

```cobol
05 PERSON-ENTRY OCCURS 100 TIMES.
```

creates repeated instances.

The museum accesses them with:

```cobol
PERSON-NAME(PERSON-INDEX)
PERSON-AGE(PERSON-INDEX)
```

This corresponds to the fixed array in the pseudocode.

Unlike the Python and JavaScript exhibits, the COBOL translation does **not** casually grow a dynamic list.

A maximum size is part of the record definition.

---

# Assignment is `MOVE`

Python:

```python
age = 18
```

JavaScript:

```javascript
age = 18;
```

C++:

```cpp
age = 18;
```

COBOL:

```cobol
MOVE 18 TO AGE
```

This is wonderfully characteristic.

The language very often reads as an instruction rather than an algebraic expression.

---

# Arithmetic can also read like English

COBOL can write:

```cobol
ADD 1 TO COUNTER
SUBTRACT 1 FROM COUNTER
MULTIPLY PRICE BY QUANTITY GIVING TOTAL
DIVIDE TOTAL BY COUNT GIVING AVERAGE
```

There is also `COMPUTE`:

```cobol
COMPUTE NEXT-N = N - 1
```

which looks more familiar to programmers from algebraic languages.

COBOL gives you both styles.

---

# `PERFORM`: one keyword, several lives

Our loop is:

```cobol
PERFORM VARYING PERSON-INDEX FROM 1 BY 1
    UNTIL PERSON-INDEX > NUM-PASSENGERS
    ...
END-PERFORM
```

`PERFORM` is extremely important in COBOL.

It can perform an inline block.

It can also invoke a paragraph:

```cobol
PERFORM GREET
```

Older COBOL code often uses paragraph-oriented control flow extensively.

Modern structured COBOL can use explicit scope terminators such as:

```text
END-IF
END-PERFORM
```

which make the structure much easier to see.

---

# Periods are more powerful than they look

A period in COBOL can terminate a **sentence**, not merely one tiny statement in the C-family sense.

This means punctuation can affect the scope of multiple operations.

Older COBOL styles relied heavily on periods and implicit scope.

Modern style usually favours explicit terminators:

```cobol
END-IF
END-PERFORM
END-EVALUATE
```

because "one full stop unexpectedly ended more structure than you mentally intended" is not an exciting debugging experience.

---

# Input and output

The museum uses:

```cobol
DISPLAY "What is your name?"
ACCEPT PERSON-NAME(PERSON-INDEX)
```

`DISPLAY` outputs data.

`ACCEPT` receives input.

Real COBOL applications are famously file- and transaction-oriented, so terminal interaction is hardly the language's most historically representative workload.

But it maps nicely to the museum specimen.

---

# Strings are often fixed-width fields

This declaration:

```cobol
PIC X(40)
```

creates a forty-character field.

If a short name is stored there, the remaining positions are padded.

That is why the specimen often uses:

```cobol
FUNCTION TRIM(...)
```

before displaying names.

COBOL's fixed-format data worldview feels different from languages where a string naturally means "however many characters happen to be in this dynamically allocated object".

Again:

**business records**.

The language keeps returning to its natural habitat.

---

# The factorial problem is spectacular here

The pseudocode conceptually treats integers as large enough for the calculation.

COBOL usually makes numeric storage explicit.

GnuCOBOL permits numeric fields up to 38 decimal digits, so our result is:

```cobol
PIC 9(38)
```

Now:

```text
33! = 8683317618811886495518194401280000000
```

fits.

But `34!` needs 39 digits.

Therefore this implementation explicitly refuses to pretend:

```cobol
IF PERSON-AGE(PERSON-INDEX) > 33
    DISPLAY "[too large ...]"
```

This is precisely the kind of difference the Language Museum exists to expose.

Python silently grows the integer.

JavaScript asks for `BigInt`.

C++ needed Boost in our exact implementation.

COBOL asks:

> Exactly how many decimal positions did you reserve on the form?

---

# Decimal arithmetic is a feature, not a limitation

Do not walk away with the impression that COBOL is numerically primitive.

COBOL's decimal arithmetic is enormously useful for business.

Financial systems often care deeply that:

```text
0.10
```

means an exact decimal quantity rather than an approximation in binary floating point.

COBOL lets programmers define fields with explicit decimal layouts:

```cobol
01 PRICE PIC 9(5)V99.
```

`V` represents an implied decimal point.

This can represent values conceptually shaped like:

```text
12345.67
```

without storing a literal decimal-point character in the field.

That is extremely on-brand for a language designed to process money.

---

# Recursion

Early COBOL was not the language you would choose to demonstrate recursive functional elegance.

Modern COBOL supports recursive programs.

Our factorial subprogram declares:

```cobol
PROGRAM-ID. FACTORIAL RECURSIVE.
```

and calls itself:

```cobol
CALL "FACTORIAL"
```

Its temporary data lives in:

```cobol
LOCAL-STORAGE SECTION.
```

so each invocation receives its own local copy.

This is considerably more ceremony than:

```python
return n * factorial(n - 1)
```

but it demonstrates something important:

Modern COBOL is not frozen at the feature set of 1960.

---

# `CALL` and separate programs

The factorial routine is represented as another COBOL program in the same source:

```cobol
PROGRAM-ID. FACTORIAL RECURSIVE.
```

The main program invokes it using:

```cobol
CALL "FACTORIAL" USING ...
```

COBOL has a strong tradition of separately callable program units.

Large business systems are often composed from many such components.

---

# `BY CONTENT` versus `BY REFERENCE`

The call uses:

```cobol
BY CONTENT NEXT-N
BY REFERENCE SUBRESULT
```

`BY CONTENT` supplies a value whose modifications inside the called program do not modify the caller's original item.

`BY REFERENCE` gives the called program access to the caller's data item.

So the factorial receives its input as content and places its result into a referenced output field.

The interface is explicit.

COBOL is fond of explicit paperwork.

---

# Fixed format versus free format

Classic COBOL source has one of the language's most recognisable historical quirks:

**columns mattered.**

Traditional fixed-format source was designed around punched cards.

Different column ranges had different purposes.

That heritage produced source code layouts that can look bizarre on a modern widescreen monitor.

Modern COBOL implementations support **free-format source**, which is what this exhibit uses:

```cobol
>>SOURCE FORMAT FREE
```

So no, you do not need to pretend your laptop is an IBM card punch just to write modern COBOL.

You *can* study fixed format, though.

The museum basement contains several boxes of columns.

---

# Why is COBOL written in uppercase?

It does not have to be.

COBOL has traditionally been shown in uppercase largely because of historical computing environments and source conventions.

Modern compilers generally accept lowercase source just fine.

This:

```cobol
display "hello".
```

can be valid.

The museum uses uppercase keywords because they make the language's distinctive syntax easier to recognise and match most historical examples.

---

# Reserved words: all of them have entered the meeting

COBOL famously contains many English-like reserved words.

Programs contain phrases such as:

```cobol
WORKING-STORAGE SECTION
PERFORM VARYING
WITH NO ADVANCING
BY REFERENCE
STOP RUN
END-IF
```

This makes code longer but often unusually self-describing.

The downside is that COBOL source sometimes looks like a technical committee successfully taught English to compile.

Which, historically, is not entirely inaccurate.

---

# Files: COBOL's home turf

The museum example barely touches one of COBOL's defining strengths.

COBOL has extensive facilities for record-oriented file processing.

Historically, applications commonly worked with:

- sequential files;
- indexed files;
- relative files;
- fixed record structures;
- sorted and merged data.

A traditional COBOL program often spends much more time reading customer records than recursively factorialising customers.

The latter would raise questions in an audit.

---

# Copybooks

COBOL commonly shares declarations or code using **copybooks**.

A copybook might contain a record layout:

```cobol
01 CUSTOMER-RECORD.
   05 CUSTOMER-ID   PIC 9(8).
   05 CUSTOMER-NAME PIC X(40).
```

Another source file can include it with `COPY`.

This is conceptually related to textual inclusion mechanisms in other languages, although COBOL copybooks have their own ecosystem and conventions.

They are extremely important in large legacy systems because common business record formats may be shared across vast amounts of code.

---

# Mainframes

COBOL is strongly associated with **mainframes**, especially IBM systems.

But COBOL itself is not "an IBM mainframe language".

Implementations exist for many platforms.

For example, **GnuCOBOL** can compile COBOL on modern Unix-like systems and other environments.

The language was specifically designed around portability.

Its reputation became tied to mainframes because that is where enormous amounts of high-value COBOL software lived and continued living.

---

# GnuCOBOL

GnuCOBOL is a free COBOL implementation.

It commonly translates COBOL into C and then uses a C compiler to produce native code.

That makes it a convenient implementation for museum visitors who do not happen to have an enterprise mainframe leaning against the wall.

Typical compilation:

```sh
cobc -x -free museum.cob
```

`-x` builds an executable.

`-free` requests free-format source.

GnuCOBOL supports multiple COBOL dialects and compatibility modes because "COBOL" has accumulated decades of implementation history.

---

# Why is COBOL still around?

Because rewriting critical software is not automatically an improvement.

A decades-old system may encode:

- business rules;
- legal requirements;
- edge cases;
- institutional knowledge;
- transaction semantics;
- integrations with dozens of other systems.

The source language being old does not make those requirements disappear.

Replacing a mature system can be expensive and risky.

Sometimes modernization means rewriting.

Sometimes it means wrapping, integrating, refactoring, migrating data or replacing infrastructure around the COBOL application.

"Just rewrite it in Java/Python/Rust" is easy to say when you are not personally responsible for ensuring several million financial transactions remain correct on Monday morning.

---

# Y2K and COBOL

COBOL is heavily associated with the **Year 2000 problem**.

Many older business systems represented years using two digits:

```text
98
99
00
01
```

When `00` arrived, software could interpret it incorrectly relative to `99`.

COBOL was not uniquely responsible for Y2K, but its enormous presence in long-lived business systems made COBOL remediation highly visible.

The successful repair effort had an ironic consequence:

Because Y2K did **not** produce the apocalyptic failures people feared, later generations sometimes concluded that the problem had been exaggerated.

A large reason catastrophe was avoided was that people spent enormous effort fixing it.

Preventive maintenance suffers from terrible public relations.

---

# Modern COBOL is not COBOL 68

Modern standards have added features over time, including:

- explicit scope terminators;
- intrinsic functions;
- object-oriented facilities;
- newer numeric and interoperability features;
- improved standardisation around modern environments.

But implementation support varies.

A feature existing in the newest ISO standard does not guarantee that every compiler on a production mainframe supports it today.

This is especially important with a language whose deployed code can be older than the programmers maintaining it.

---

# COBOL and readability

A COBOL statement might say:

```cobol
MULTIPLY QUANTITY BY UNIT-PRICE GIVING TOTAL-PRICE
```

That is verbose.

It is also difficult to misunderstand.

Compare:

```text
t = q * p
```

The latter is compact, but only useful if you know what `t`, `q` and `p` represent.

COBOL's original design culture strongly favoured meaningful business names and English-like operations.

Of course, COBOL programmers remain fully capable of choosing terrible variable names.

No language standard can save us from ourselves.

---

# Shitpost cabinet

## `STOP RUN`

Many languages end with:

```text
return
exit
```

COBOL can end with:

```cobol
STOP RUN.
```

Not "exit".

Not "terminate".

**STOP RUN.**

The computer has finished doing business for the day.

---

## COBOL naming

COBOL lets identifiers contain hyphens:

```cobol
NUM-PASSENGERS
FACTORIAL-RESULT
PERSON-AGE
```

This is so aesthetically compatible with screaming uppercase business terminology that a sufficiently large COBOL program begins to resemble a government department's internal org chart.

---

## COBOL versus INTERCAL

The original INTERCAL manual states that its goal was to have nothing in common with languages its creators knew.

The list explicitly included:

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

So COBOL is one of the languages that helped define INTERCAL through **negative space**.

INTERCAL looked at the programming languages of its era and asked:

> What if absolutely not?

For the resulting archaeological incident, see:

**[`../recreational/intercal.md`](../recreational/intercal.md)**

---

# Learn more

Language Museum is not a COBOL course.

Useful next stops:

- [ISO/IEC 1989:2023](https://www.iso.org/standard/74527.html)
- [GnuCOBOL](https://gnucobol.sourceforge.io/)
- [GnuCOBOL Programmer's Guide](https://gnucobol.sourceforge.io/guides.html)
- [IBM: What is COBOL?](https://www.ibm.com/think/topics/cobol)
- [Open Mainframe Project](https://openmainframeproject.org/)

For historical context:

- [Computer History Museum: Grace Hopper](https://computerhistory.org/profile/grace-hopper/)
- [Wikipedia: COBOL](https://en.wikipedia.org/wiki/COBOL)

For the source program this exhibit translates:

- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

COBOL turns the canonical specimen into something much more data-shaped.

Compared with Python, JavaScript and C++:

- data fields have explicit decimal or character layouts;
- hierarchical records are fundamental;
- fixed-size tables are natural;
- assignment often reads as `MOVE X TO Y`;
- loops and subroutines revolve around `PERFORM`;
- decimal business arithmetic is a first-class concern;
- program structure is divided into named divisions and sections;
- verbosity is a feature as much as a reputation.

Then factorial reveals the museum's recurring numerical fault line.

Python says:

> sure, the integer can get bigger.

JavaScript says:

> use `BigInt`.

C++ says:

> bring a multiprecision library.

COBOL says:

> Please specify exactly how many digits this field is authorised to contain.

And somewhere, a bank approves the form.
