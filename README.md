# Language Museum

> A museum of programming languages, esolangs, almost-languages, and things standing suspiciously close to the concept of a language.

**Language Museum** is an edutainment project about programming languages and related systems.

The idea is simple: take the same small reference program and see how different languages express it.

Along the way, each exhibit explains things such as:

* what the language is;
* where it came from;
* what it was designed for;
* how its syntax and basic concepts work;
* what makes it different from other languages;
* how it implements the Language Museum's reference program;
* and, where appropriate, whatever bizarre historical footnote, cursed syntax, or shitpost the language has generously provided us with.

The goal is to be **educational enough that you come away understanding something useful**, while remaining **entertaining enough that reading documentation does not feel like being slowly laminated alive**.

---

## This is not a tutorial

Language Museum is **not intended to teach you an entire programming language from scratch**.

An exhibit should give you a useful understanding of the language, including its basic syntax, concepts, history, ecosystem, and unusual features. By the end, you should have a decent idea of what the language *is* and what writing it feels like.

That does **not** necessarily mean you will be able to build a complete program in it without further learning.

Instead, exhibits link to external resources such as:

* official documentation;
* tutorials;
* language specifications;
* interactive playgrounds;
* books;
* community resources;
* and other places where you can continue learning.

Think of this repository as a **museum**, not a classroom.

You can inspect the exhibits, compare them, learn why things look the way they do, and discover something you might want to study properly afterwards.

---

## Start here

If this is your first time here, begin with:

**[`other/pseudocode.md`](other/pseudocode.md)**

This contains the museum's canonical reference program.

Most exhibits ultimately derive from that program, translating the same behaviour into the language being exhibited.

This gives the museum a common point of comparison.

Rather than:

> "Here is some random Python."

followed by:

> "Here is some completely unrelated C."

you get:

> "Here is the same program in Python."

> "Here is how C expresses the same ideas."

> "Here is how Haskell does it."

> "Here is what happens when we ask INTERCAL to do it, and may God have mercy on us."

The reference program deliberately includes a range of basic programming concepts so that languages have something interesting to disagree about, including functions, procedures, variables, input and output, conditionals, loops, collections, recursion, strings, integers, and factorials.

---

## Repository structure

The museum is currently divided into three main collections:

```text
language-museum/
├── languages/
├── recreational/
└── other/
```

### `languages/`

The main collection.

This contains conventional programming languages: languages intended to actually be used for software development, scripting, systems programming, data processing, application development, or similar purposes.

Examples might include:

```text
languages/python.md
languages/c.md
languages/rust.md
languages/javascript.md
languages/haskell.md
```

"Conventional" does not mean "boring".

Programming language designers have seen to that.

---

### `recreational/`

Languages created primarily for recreation, experimentation, artistic purposes, jokes, puzzles, language-design research, or the noble scientific pursuit of making computers suffer.

This includes **esoteric programming languages**, commonly known as **esolangs**, as well as similar experimental languages.

Examples might eventually include things such as:

```text
recreational/brainfuck.md
recreational/befunge.md
recreational/intercal.md
recreational/malbolge.md
```

Some recreational languages are surprisingly influential or technically interesting.

Others were designed specifically to be horrible.

These categories are not mutually exclusive.

---

### `other/`

The cabinet labelled **"well, it's KIND OF a language"**.

This section contains systems that resemble programming languages, notation systems, machine interfaces, pseudolanguages, or other things worth examining alongside programming languages without necessarily being conventional programming languages themselves.

For example:

```text
other/pseudocode.md
other/ai-prompt.md
```

This category intentionally has fuzzy edges.

If something has syntax, semantics, instructions, expressions, or some other language-like method of telling a machine or human what you mean, there is at least a chance it will eventually end up in here.

---

## How exhibits work

Although different languages require different explanations, exhibits generally follow the same basic idea.

An exhibit may cover:

### Overview

What the language is and what it is normally used for.

### History

Who created it, when it appeared, why it was created, and how it developed.

### Language concepts

An explanation of the language's important syntax and features.

Depending on the language, this could include things such as:

* variables;
* data types;
* functions;
* procedures;
* objects;
* modules;
* pattern matching;
* memory management;
* type systems;
* concurrency;
* metaprogramming;
* or whatever strange machinery the language has hidden under the floorboards.

### The museum program

A translation of the canonical program from [`other/pseudocode.md`](other/pseudocode.md).

The goal is to preserve the **behaviour and intent** of the reference program while writing reasonably idiomatic code for the language being demonstrated.

A translation therefore does not need to mechanically reproduce every pseudocode construct if doing so would be unnatural in that language.

The interesting part is often precisely **how the language chooses to solve the same problem differently**.

### Explanation

A walkthrough of the translated program and the language features it demonstrates.

### Weird stuff

Where appropriate, exhibits may also cover historical accidents, strange syntax, implementation quirks, famous bugs, unusual language features, programming folklore, terrible ideas, magnificent terrible ideas, and other things worthy of preservation.

### Learn more

Links to resources where you can actually continue learning the language.

Again:

**Language Museum is an introduction and comparison resource, not a replacement for proper language documentation or a complete course.**

---

## How to use the museum

There is no required route through the collection.

You can:

1. **Start with the pseudocode exhibit.**
   This is recommended if you want to understand the common program used throughout the museum.

2. **Pick a language you already know.**
   Seeing a familiar language first makes it easier to understand how the exhibits are structured.

3. **Compare languages.**
   Open several exhibits and see how each one approaches the same reference program.

4. **Go somewhere weird.**
   Wander into `recreational/` and discover what happens when language design stops being supervised.

5. **Follow the learning links.**
   If an exhibit makes you genuinely interested in a language, use its external resources to go beyond what the museum covers.

You do **not** need to read the repository in any particular order.

---

## Why use the same program?

Programming languages are difficult to compare when every example demonstrates something different.

A "Hello, World!" program is easy to compare, but it tells you almost nothing about the language.

A complete real-world application tells you plenty, but is far too large to understand at a glance.

Language Museum uses a program somewhere in the middle.

It is deliberately large enough to expose meaningful differences between languages while remaining small enough that the same program can reasonably be implemented again and again.

As a result, differences become visible.

One language may have dictionaries built directly into its syntax.

Another may require a library.

One may have arbitrary-precision integers.

Another may overflow.

One may express a loop in a single line.

Another may decide that loops are philosophically questionable.

Those differences are the point.

---

## Accuracy

The museum tries to distinguish between:

* properties of the **language itself**;
* properties of a particular **implementation**;
* historical versions of a language;
* modern conventions;
* and things programmers merely tend to do.

Programming languages change over time, and some have existed for decades with multiple competing implementations.

Where that distinction matters, exhibits should say which version, standard, implementation, interpreter, compiler, or runtime they are talking about.

If an exhibit contains an error, correction, missing context, or questionable claim, contributions are welcome.

---

## The important bit

Language Museum exists because programming languages are interesting.

Not just as tools, but as artifacts.

Every language represents a pile of decisions about how humans should communicate instructions, how computers should interpret those instructions, what should be easy, what should be difficult, and what should result in twelve pages of compiler errors because you misplaced a semicolon.

Some of those decisions became the foundations of modern computing.

Some disappeared.

Some were resurrected forty years later under a new name.

Some produced Brainfuck.

They all deserve an exhibit.