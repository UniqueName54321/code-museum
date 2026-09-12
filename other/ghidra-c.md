# Ghidra's "C"

> **Collection:** `other/`  
> **Kind:** C-like decompiler output / recovered high-level representation  
> **Produced by:** Ghidra's Decompiler  
> **Actual programming language:** No  
> **Looks like:** C after losing its source code, debug symbols, variable names, types, comments, dignity, and several important memories  
> **Museum target:** Ghidra 12.1.x decompiler behaviour  
> **Reference exhibit:** [`pseudocode.md`](pseudocode.md)  
> **Related language:** [`../languages/c.md`](../languages/c.md)

Ghidra has a version of C that nobody writes.

You feed Ghidra a compiled program:

```text
machine code
```

and its decompiler attempts to reconstruct something resembling:

```c
int add(int param_1, int param_2)
{
    return param_1 + param_2;
}
```

This output is usually called **decompiled C**.

Ghidra's own API exposes the decompiler's result as "C code", and the standard decompilation mode performs analysis intended to produce C-like output.

But this is **not a C dialect in the normal programming-language sense**.

There is no ISO standard for:

```c
undefined8
FUN_00101230
DAT_00104020
CONCAT44(...)
SUB84(...)
```

You are not meant to open a text editor, write beautiful Ghidra-C from scratch, and ship it to customers.

Ghidra-C is a **human-readable reconstruction of binary behaviour**.

It belongs in `other/` because it is language-shaped without actually being a source programming language.

It is forensic C.

---

# What is Ghidra?

**Ghidra** is a software reverse-engineering framework originally developed by the United States National Security Agency and publicly released in 2019.

It includes tools for:

- disassembly;
- decompilation;
- data-type analysis;
- symbol recovery and editing;
- scripting;
- binary comparison;
- program analysis;
- processor-language descriptions;
- debugging and emulation.

As of **September 2026**, the current release line is **Ghidra 12.1**, with **12.1.3** the latest published release when this exhibit was written.

The feature we care about here is the **Decompiler**.

---

# Disassembly versus decompilation

Suppose a program originally contains:

```c
int double_it(int x) {
    return x * 2;
}
```

After compilation for an x86-64 machine, the processor does not receive:

```c
return x * 2;
```

It receives machine instructions.

A disassembler may render those instructions as assembly:

```asm
lea eax, [rdi+rdi]
ret
```

A decompiler tries to raise that back into a higher-level representation:

```c
int double_it(int param_1)
{
    return param_1 * 2;
}
```

That looks like C.

But the compiler did not preserve a little hidden copy of:

```c
int double_it(int x)
```

for Ghidra to retrieve later.

The decompiler is **inferring**.

That distinction is the entire exhibit.

---

# The pipeline

Ghidra's own training material summarizes the decompiler pipeline roughly as:

```text
Assembly
   ↓
P-Code
   ↓
C code
```

P-Code is Ghidra's intermediate representation for machine semantics.

Processor descriptions translate machine instructions into P-Code operations.

The decompiler then performs analysis over those operations and emits a higher-level C representation.

So Ghidra-C is not:

```text
original source recovered from executable
```

It is closer to:

```text
a C-like explanation of what Ghidra currently believes
the machine code is doing
```

Those are very different promises.

---

# Source code is lossy

Compilation destroys or transforms enormous amounts of source-level information.

Depending on build settings and binary format, the executable may no longer contain:

- local variable names;
- source comments;
- typedef names;
- formatting;
- macro boundaries;
- exact loop syntax;
- inline function boundaries;
- generic/template abstractions;
- original source-file organization;
- whether an expression used one equivalent spelling or another.

Optimization can go further and:

- inline functions;
- remove variables;
- merge variables;
- eliminate branches;
- unroll loops;
- replace arithmetic with cheaper equivalent operations;
- reorder calculations;
- eliminate entire functions;
- propagate constants.

Ghidra cannot recover information that simply is not present.

It can infer.

It can pattern-match.

It can use symbols and debug metadata when available.

It can let the reverse engineer annotate types and names.

But the decompiler is not a time machine.

---

# A tiny example

Original source:

```c
int age_check(int age) {
    if (age >= 18) {
        return 1;
    }

    return 0;
}
```

A compiler may turn the important operation into something conceptually equivalent to:

```asm
cmp edi, 17
setg al
movzx eax, al
ret
```

A decompiler may reconstruct:

```c
int age_check(int param_1)
{
    return 17 < param_1;
}
```

That is not textually the original source.

But it is semantically equivalent for the relevant inputs.

The decompiler's job is not plagiarism.

It is archaeology.

---

# The canonical Language Museum specimen

There is an immediate philosophical problem.

Every normal exhibit asks:

> How would this language implement the canonical program?

Ghidra-C cannot answer that question.

You do not implement the program **in Ghidra-C**.

Instead:

1. implement the program in a compiled language such as C;
2. compile it;
3. strip away useful source-level information;
4. load the binary into Ghidra;
5. ask the decompiler what it can reconstruct.

So the Ghidra-C exhibit inverts the museum.

Most exhibits go:

```text
pseudocode
    ↓
source language
    ↓
program
```

This one goes:

```text
pseudocode
    ↓
C source
    ↓
compiler
    ↓
machine code
    ↓
Ghidra
    ↓
C-shaped archaeological reconstruction
```

Welcome to reverse engineering.

---

# Representative decompiler output

The exact output depends on:

- CPU architecture;
- operating system ABI;
- compiler;
- compiler version;
- optimization level;
- linked libraries;
- debug information;
- symbol stripping;
- Ghidra version;
- imported function signatures;
- data types the analyst has already applied;
- manual renaming and retyping.

So there is **no single canonical Ghidra rendering** of the museum program.

That would be like asking for the canonical photograph of a crime scene after letting five different cleaning crews visit first.

Instead, this exhibit uses **representative Ghidra-style output** to explain recurring features.

Imagine the normal C museum program has been compiled as a 64-bit native executable with symbols stripped.

A small recovered function might initially look like:

```c
void FUN_00101280(char *param_1)
{
    printf("Hi %s!\n", param_1);
    printf("I hope you are well, %s\n", param_1);
    printf("Nice to meet you, %s\n", param_1);
    return;
}
```

The source might originally have called this:

```c
greet
```

Ghidra does not know that.

So it invents:

```text
FUN_00101280
```

based on the function's address.

The analyst can then rename it back to something meaningful.

---

# `FUN_...`

A name such as:

```text
FUN_00101280
```

usually means:

> Ghidra identified a function here, but no better symbol name is currently known.

The hexadecimal-looking suffix corresponds to an address.

After reverse engineering the function, you might rename it:

```text
greet
```

The decompiled view updates.

This is an important part of Ghidra workflow.

The decompiler output is **interactive analysis**, not immutable generated prose.

---

# `param_1`

If Ghidra sees a function parameter but does not know the original source name, it may call it:

```text
param_1
param_2
param_3
```

Original:

```c
void greet(const char *name)
```

Recovered:

```c
void FUN_00101280(char *param_1)
```

The name:

```text
name
```

was not encoded into ordinary optimized machine instructions.

The reverse engineer has to recover its *meaning*.

---

# `local_...`

Unknown local variables often receive names such as:

```text
local_18
local_24
local_30
```

These names are commonly related to their recovered stack location rather than their original source identity.

For example:

```c
int local_14;
char local_78[100];
```

may eventually turn out to be:

```c
unsigned int age;
char name[100];
```

Decompiler output begins as evidence.

The analyst turns it into understanding.

---

# `DAT_...`

Ghidra may name data at a memory address:

```text
DAT_00104020
```

This roughly means:

> there is data here, and I do not yet have a meaningful symbol name for it.

It might be:

- a global variable;
- a constant;
- part of a table;
- an object;
- an encoded pointer;
- something else entirely.

Reverse engineering consists partly of replacing:

```text
DAT_00104020
```

with:

```text
max_passengers
```

when the evidence supports that interpretation.

---

# `LAB_...`

Decompiler output can contain labels such as:

```text
LAB_0010139a:
```

with:

```c
goto LAB_0010139a;
```

Compilers naturally produce branches.

Ghidra tries to recover structured:

```c
if
for
while
switch
```

constructs where possible.

But not every control-flow graph can be neatly expressed that way, especially after optimization or with hand-written assembly.

When necessary, labels survive.

The compiler has returned C to its primordial `goto` swamp.

---

# `undefined`

One of Ghidra-C's signature features is the type:

```c
undefined
```

and sized relatives such as:

```c
undefined1
undefined2
undefined4
undefined8
```

These do **not** mean:

> C has a type called `undefined8`.

They mean Ghidra knows the storage size but has not established a more specific semantic data type.

Ghidra's API describes `Undefined8DataType` as an eight-byte data type that has not yet been defined as a particular type of program data.

So:

```c
undefined8 uVar1;
```

means roughly:

> I know this is eight bytes wide. I do not yet confidently know whether you should think of it as a pointer, unsigned integer, signed integer, part of a structure, or something else.

This is epistemology with a semicolon.

---

# `byte`, `word`, `dword`, `qword`

Decompiler/type systems often expose width-oriented names.

Depending on context and Ghidra's current type information, you may see things like:

```c
byte
ushort
uint
ulong
undefined4
undefined8
```

These are useful for describing storage precisely when source-language intent is unknown.

The reverse engineer should not assume:

```text
8 bytes = original source used uint64_t
```

The binary preserves representation much better than it preserves *meaning*.

---

# Why Ghidra sometimes guesses the "wrong" signedness

Suppose the machine performs a 32-bit operation.

Was the source variable:

```c
int
```

or:

```c
unsigned int
```

?

Sometimes machine instructions reveal the distinction.

Sometimes later operations constrain it.

Sometimes there is not enough evidence.

Decompiler type recovery is an inference process.

A misleading type can make otherwise simple code look bizarre.

Fixing a function signature or variable type can dramatically simplify the decompiled output.

This is why reverse engineering is interactive rather than:

```text
click Decompile → receive source repository
```

---

# Function signatures are hypotheses

Ghidra tries to infer:

- how many parameters a function takes;
- which registers or stack slots contain them;
- their types;
- the return type;
- calling convention.

But optimized machine code can make this difficult.

If the decompiler believes the wrong prototype, it may produce:

- extra parameters;
- strange casts;
- `CONCAT` operations;
- nonsensical pointer arithmetic.

Correcting the signature often cleans up the whole function.

One wrong type can turn a neat decompile into spaghetti generated by a mathematically gifted raccoon.

---

# `CONCATxy(...)`

Sometimes Ghidra needs to express a low-level P-Code operation that has no clean natural C operator.

It displays an internal pseudo-function.

For example:

```c
CONCAT44(high, low)
```

conceptually concatenates:

- 4 bytes from `high`;
- 4 bytes from `low`;

into an 8-byte value.

The digits indicate operand sizes in bytes.

For example:

```c
CONCAT31(x, y)
```

takes a 3-byte left portion and a 1-byte right portion to form a 4-byte result.

These are **not ordinary library calls in the binary**.

They are notation emitted by the decompiler to explain a P-Code operation.

This is where Ghidra-C stops merely looking "slightly weird C" and openly reveals that it is a decompiler language.

---

# `SUBxy(...)`

Likewise:

```c
SUB84(x, 4)
```

is an internal decompiler notation for extracting a subpiece.

The digits describe input and output sizes.

A documented example:

```text
SUB84(x, 4)
```

extracts the high four bytes from an eight-byte value.

Again:

```c
SUB84
```

is not a function your original programmer necessarily wrote.

It is Ghidra saying:

> the machine did something for which ordinary C syntax is currently an awkward description, so here is my internal operation wearing function-call cosplay.

---

# These pseudo-functions are clues

Seeing:

```c
CONCAT44(...)
SUB84(...)
```

often means:

- the decompiler has incomplete type information;
- machine registers are being assembled/disassembled across widths;
- a compiler performed a low-level transformation;
- the target architecture naturally splits wider values;
- optimized code is obscuring a higher-level operation.

Sometimes better type information makes the pseudo-function disappear.

Sometimes it reflects a real low-level implementation detail that genuinely has no prettier C equivalent.

Do not automatically translate every `CONCAT44` into a meaningful source-level operation.

It may be decompiler scar tissue.

---

# `in_FS_OFFSET` and friends

On some architectures and operating systems, Ghidra may expose synthetic-looking variables such as:

```text
in_FS_OFFSET
```

or register-derived names.

These can represent special machine state used by:

- thread-local storage;
- stack canaries;
- ABI machinery;
- runtime support.

For example, compiler-generated stack-protection code can appear in the decompile even though the original source never explicitly mentioned a stack canary.

You wrote:

```c
char name[100];
```

The compiler quietly added security machinery.

Ghidra faithfully digs it back up and leaves it on your desk.

---

# `extraout_...`

You may also encounter names such as:

```text
extraout_RDX
extraout_EAX
```

These can represent values the decompiler tracks as additional register outputs associated with operations or calls.

They are another sign that the recovered high-level model does not yet map cleanly onto an ordinary source-level abstraction.

The original programmer almost certainly did not type:

```c
undefined8 extraout_RDX_01;
```

before lunch.

---

# Stack variables and offsets

Machine code frequently accesses local storage relative to a stack pointer or frame pointer.

Decompiler analysis tries to turn those locations back into local variables.

That is why names such as:

```text
local_28
local_30
```

appear so often.

If several source variables reuse the same stack slot at different times, optimization may make source reconstruction particularly ambiguous.

At machine level, a memory slot has no obligation to remember that it was once named `age`.

---

# The decompiled museum program

A *representative* stripped decompile of a simple C implementation might look conceptually like this:

```c
int main(void)
{
    int iVar1;
    uint local_224;
    uint local_220;
    char local_218[128];
    undefined local_198[10400];

    puts(
        "How many people are you going into the bar with? "
        "(maximum 100)"
    );

    fgets(local_218, 128, stdin);
    iVar1 = FUN_00101520(local_218, &local_224);

    if ((iVar1 == 0) || (100 < local_224)) {
        fwrite("Invalid passenger count.\n", 1, 25, stderr);
        return 1;
    }

    local_220 = 0;

    while (local_220 < local_224) {
        puts("What is your name?");

        /* address arithmetic into the recovered PEOPLE storage */
        FUN_001011f0(...);

        puts("How old are you (in human years)?");
        ...
        local_220 = local_220 + 1;
    }

    puts(
        "Fun fact! For everyone here, "
        "their ages in factorial would be:"
    );

    ...
    puts("Interactive museum entry for C completed.");

    return 0;
}
```

This is intentionally illustrative rather than a claim about one exact compiler build.

A different compilation may:

- inline `greet`;
- eliminate `age_check`;
- transform loops;
- use `memset`;
- reorder locals;
- recognize standard-library calls;
- emit radically different control flow.

Decompiler output is a property of the **binary**, not just the source language.

---

# Optimization changes the archaeology

Compile with:

```sh
-O0
```

and the binary often preserves a structure closer to source.

Compile with:

```sh
-O2
```

or:

```sh
-O3
```

and the compiler may aggressively transform it.

For example:

```c
int square(int x) {
    return x * x;
}
```

might remain recognisable.

But:

```c
int multiply_by_eight(int x) {
    return x * 8;
}
```

may become a shift or scaled-address operation.

Ghidra may reconstruct:

```c
return param_1 << 3;
```

The original source used multiplication.

The decompiled source uses shifting.

Both describe the same machine-level result over the relevant domain.

Which one is "the real program"?

At reverse-engineering time, that question becomes philosophical very quickly.

---

# Debug symbols change everything

A binary built with debug information may preserve:

- function names;
- source variable names;
- source files;
- line mappings;
- type definitions;
- structure layouts.

A stripped release binary may preserve far less.

So Ghidra's output for the same algorithm can range from:

```c
int factorial(int age)
```

to:

```c
undefined8 FUN_00101370(uint param_1)
```

depending on metadata.

Decompiler quality is partly a function of how much archaeology survived the compiler and linker.

---

# Library identification

A binary may contain or call standard-library functions.

If Ghidra recognizes symbols and signatures, output becomes much clearer:

```c
printf(...)
malloc(...)
free(...)
```

rather than:

```c
FUN_00104560(...)
FUN_00104820(...)
FUN_00104910(...)
```

Imported symbols, type libraries, function ID analysis and manual signature work can all improve the decompile.

Names are leverage.

---

# Types are leverage too

Suppose Ghidra initially thinks:

```c
undefined8 param_1
```

but you determine it is actually:

```c
Person *param_1
```

After changing the type, expressions such as:

```c
*(int *)(param_1 + 0x68)
```

may become:

```c
param_1->age
```

That transformation is enormously important.

Reverse engineering often consists of gradually replacing:

```text
addresses + offsets + undefined widths
```

with:

```text
objects + fields + meaning
```

The decompiler is not just displaying code.

It is a projection of the analyst's current type model.

---

# Structures can emerge

At first:

```c
*(uint *)(param_1 + 0x68)
```

Later, after creating a structure:

```c
param_1->age
```

At first:

```c
param_1
```

Later:

```c
Person *person
```

The machine code never changed.

Only the model did.

This is why a polished reverse-engineering project can look dramatically cleaner than the first auto-analysis pass.

---

# Renaming is analysis

Renaming:

```text
FUN_00101280
```

to:

```text
greet
```

is not cosmetic.

It records a hypothesis:

> I believe this function greets a person.

Renaming:

```text
param_1
```

to:

```text
name
```

records another.

A reverse-engineering database gradually becomes a map of recovered semantics.

Good names are compressed research notes.

---

# Decompiled C may not compile

This is one of the most important points in the exhibit.

Even though Ghidra calls the result C code, the output is not guaranteed to be drop-in compilable source.

Reasons include:

- Ghidra-specific types such as `undefined8`;
- internal pseudo-functions such as `CONCAT44`;
- missing declarations;
- synthetic names;
- architecture-specific conventions;
- incompletely recovered prototypes;
- constructs representing machine semantics rather than standard C source.

You *can* manually massage decompiled code into compilable C.

That is a separate reconstruction task.

Decompiler output is optimized for reverse-engineering comprehension, not round-trip source regeneration.

---

# If it looks compilable, that still does not make it original

Suppose Ghidra emits:

```c
int FUN_00101230(int param_1)
{
    if (param_1 < 2) {
        return 1;
    }

    return param_1 * FUN_00101230(param_1 - 1);
}
```

You could rename it and compile:

```c
int factorial(int n)
```

The resulting new binary might behave equivalently.

But that does **not** prove the original source looked like that.

The original might have been:

```cpp
template <typename T>
T factorial(T n) { ... }
```

or:

```rust
fn factorial(...)
```

or hand-written assembly.

Decompiler C is a **target representation for understanding**, not provenance.

---

# C++ binaries become C-shaped too

This is especially funny after the C++ exhibit.

Compile:

```cpp
std::vector<Person>
```

and strip symbols.

Ghidra does not necessarily reconstruct:

```cpp
std::vector<Person>
```

as lovely idiomatic C++ source.

Instead you may see:

- raw pointers;
- allocation calls;
- constructor/destructor functions;
- vtable references;
- name-mangled functions;
- pointer arithmetic;
- exception machinery;
- runtime helpers.

A high-level C++ abstraction can collapse into extremely C-shaped wreckage.

This is not because the original program "was secretly C".

It is because C-like syntax is a practical language for representing recovered low-level operations.

---

# Rust, Go and other languages also become C-shaped

Ghidra can decompile machine code produced from many source languages.

The resulting view still commonly looks C-like.

A Rust executable may therefore display:

```c
void FUN_0010abcd(...)
```

even though no C source existed.

Likewise Go, Swift, Fortran or hand-written assembly.

This alone proves why Ghidra-C belongs in `other/`.

It is not the source language.

It is a **decompiler presentation language**.

---

# Calling conventions

At binary level, functions communicate through calling conventions.

These specify things such as:

- which registers hold parameters;
- where stack arguments live;
- where return values appear;
- which registers a callee must preserve;
- stack alignment.

Ghidra uses processor and compiler specifications to recover higher-level function signatures from this machinery.

A wrong calling convention can make parameter recovery explode.

What looks like a weird C signature may actually be a wrong ABI hypothesis.

---

# P-Code

P-Code is central to Ghidra's language-independent analysis.

Machine instructions from different architectures are translated into a common set of semantic operations.

For example, a machine load becomes a P-Code `LOAD`.

A copy becomes `COPY`.

Arithmetic becomes operations such as integer add, subtract, compare, and so on.

This lets Ghidra perform analysis without rewriting every high-level algorithm separately for x86, ARM, PowerPC, MIPS, RISC-V and every other supported processor.

P-Code is closer to being an actual intermediate language than Ghidra-C is.

Ironically, the thing that looks less like C is more formally language-like.

---

# SLEIGH

Ghidra processor specifications use a language called **SLEIGH** to describe instruction encoding and semantics.

SLEIGH translates processor instructions into P-Code semantics.

That means Ghidra's decompiler ecosystem contains several layers:

```text
machine language
    ↓
SLEIGH-described semantics
    ↓
P-Code
    ↓
decompiler analysis
    ↓
Ghidra-C
```

If the Language Museum ever wants another `other/` rabbit hole, SLEIGH is standing in the hallway holding paperwork.

---

# Internal decompiler functions

Ghidra documentation explicitly describes cases where P-Code operations cannot be represented naturally as:

- a normal operator;
- a cast;
- another ordinary source construct.

The decompiler then emits them using **function-like syntax**.

Examples include:

```c
SUB42(...)
SUB84(...)
CONCAT31(...)
CONCAT44(...)
```

This is one of the strongest pieces of evidence that Ghidra-C is its own peculiar notation.

Normal C has function-call syntax.

Ghidra borrows that syntax to display operations that are not actual source-level function calls.

The costume is C.

The actor is intermediate representation.

---

# Why `CONCAT` can appear after a bad type guess

Imagine a 64-bit register whose upper and lower halves are manipulated separately.

If Ghidra believes those pieces belong to one wider value but cannot express the reconstruction naturally, it may emit something like:

```c
CONCAT44(high, low)
```

If later you apply the correct structure, integer width or function prototype, the decompiler may be able to rewrite the same operation as ordinary C.

So bizarre output is not always bizarre machine code.

Sometimes it is the visible consequence of incomplete information.

Reverse engineering has a recurring lesson:

> Before blaming the compiler, check whether your types are lying.

---

# Compiler transformations leak through

Suppose source code performs:

```c
double x = (double) i;
```

On some architectures, the compiler may implement that conversion using a clever constant-and-bit-manipulation sequence.

Ghidra can expose that mechanism rather than recover the simple cast.

Real examples have produced output involving forms such as:

```c
(double)CONCAT44(0x43300000, i ^ 0x80000000)
```

followed by subtraction of a large floating-point constant.

The source-level intent was simple.

The machine-level implementation was clever.

The decompiler is forced to decide which story it can prove.

Sometimes you get the clever one.

---

# Decompiled code is architecture-sensitive

Compile the same C source for:

```text
x86-64
ARM64
PowerPC
RISC-V
MIPS
```

and Ghidra may emit different C-shaped reconstructions.

Why?

Because:

- registers differ;
- calling conventions differ;
- instructions differ;
- optimization opportunities differ;
- width handling differs;
- ABI-generated code differs.

Ordinary source C tries to be portable *across* architectures.

Ghidra-C is born *from* one architecture's compiled result.

It therefore carries machine fingerprints.

---

# The factorial pressure test, again

The museum factorial becomes especially fun after decompilation.

Our C exhibit implemented a tiny arbitrary-precision integer using:

```c
typedef struct {
    unsigned char *digits;
    size_t length;
    size_t capacity;
} BigUInt;
```

After compilation, Ghidra may initially see only:

```c
undefined8
```

fields and offsets.

A clean source operation such as:

```c
number->length
```

might initially look like:

```c
*(long *)(param_1 + 8)
```

The source tells us:

> this is the `length` field.

The binary tells us:

> load eight bytes at offset eight from the object whose address is in this register.

Both are true.

One is semantic.

One is structural.

Ghidra's job is to climb from the second back toward the first.

---

# What optimization can do to recursion

The pseudocode explicitly expresses recursive factorial.

A compiler may preserve recursive calls.

It may inline some calls.

It may transform recursion into a loop if optimization allows.

It may specialize cases.

So Ghidra might decompile a function that is **behaviourally factorial** but no longer appears recursive.

This is a gorgeous museum problem.

The source-language exhibit demonstrates:

```text
how the programmer expressed the algorithm
```

The Ghidra exhibit demonstrates:

```text
what survived compilation
```

These are not guaranteed to be the same structure.

---

# `return` values can get strange

On many ABIs, small values are returned in one register.

Larger structures may be:

- split across registers;
- returned through hidden pointers;
- placed in multiple machine locations.

If Ghidra has not recovered the intended aggregate type, a return statement can contain:

```c
CONCAT...
```

operations stitching pieces together.

The source might simply have returned:

```c
return result;
```

Machine calling conventions are where elegant source abstractions are sent for customs inspection.

---

# The decompiler and truth

A crucial reverse-engineering habit is:

> The decompiler is evidence, not truth.

It can be wrong about:

- types;
- signedness;
- function boundaries;
- prototypes;
- structure layouts;
- whether two storage locations are conceptually the same variable;
- what a call represents.

The assembly and underlying machine semantics are the final ground truth.

The decompiler is an extraordinarily useful interpretation layer.

Treating it as infallible source recovery is how you end up naming a cryptographic key `probably_string`.

---

# Improve the decompile instead of fighting it

Good Ghidra workflow often includes:

1. identify what a function appears to do;
2. rename it;
3. rename parameters and locals;
4. correct parameter types;
5. create structures and enums;
6. apply known library signatures;
7. mark constants and strings;
8. inspect callers and callees;
9. revisit the function.

Each improvement can trigger a cleaner decompilation.

The code becomes more readable because **your model became more accurate**.

Decompiler work is collaborative editing between analyst and inference engine.

---

# Example: from wreckage to meaning

Initial:

```c
undefined8 FUN_00101230(undefined8 param_1)
{
    undefined4 uVar1;

    uVar1 = *(undefined4 *)(param_1 + 0x68);

    if (uVar1 < 18) {
        return 0;
    }

    return 1;
}
```

After determining:

- `param_1` is `Person *`;
- offset `0x68` is an unsigned age field;
- the function checks age eligibility;

you might create the structure and rename things.

Then Ghidra can display something closer to:

```c
bool person_passes_age_check(Person *person)
{
    if (person->age < 18) {
        return false;
    }

    return true;
}
```

The binary stayed identical.

The decompiled "source" changed dramatically.

That is why calling Ghidra-C a programming language would be misleading.

Languages define programs.

This notation reflects **analysis state**.

---

# `code *`

Sometimes Ghidra represents a function pointer using something like:

```c
code *pcVar1;
```

Again, `code` is not an ordinary ISO C scalar type you learned from K&R.

It is part of Ghidra's data-type vocabulary for representing executable code/function-pointer-like storage when stronger type information is absent.

A reverse engineer may later replace it with a meaningful typed function pointer.

---

# Why names often contain type hints

Synthetic variable prefixes can look like:

```text
iVar
uVar
lVar
cVar
pcVar
puVar
```

They often encode Ghidra's inferred type category:

- `i` for int-like;
- `u` for unsigned/undefined-like;
- `l` for long-like;
- `c` for char-like;
- `p` prefixes for pointers.

These names are generated conveniences.

They are not evidence that the original programmer practiced Hungarian notation while experiencing a breakdown.

---

# Ghidra output can change when nearby information changes

The decompiler does not analyze each line in isolation.

Changing:

- a called function's prototype;
- data mutability;
- structure definitions;
- parameter types;
- symbol information;

can change the current decompilation.

Ghidra's own training material explicitly warns that decompiler output may change due to program changes elsewhere.

So Ghidra-C does not even have a perfectly fixed rendering for one binary database.

It is a live projection.

---

# Source reconstruction versus recompilation

There are projects whose goal is to reconstruct source code that compiles back into a matching or functionally equivalent binary.

That work may begin from Ghidra decompilation.

But it requires human effort to turn:

```c
undefined8
FUN_...
CONCAT44
```

into:

- meaningful types;
- legal source;
- correct control flow;
- correct declarations;
- correct build settings.

Ghidra gives you a map drawn from aerial photographs.

It does not rebuild the city.

---

# Ghidra-C versus real C

| Concept | ISO C | Ghidra decompiler C |
| --- | --- | --- |
| Purpose | Write programs | Understand compiled programs |
| Standard | ISO/IEC 9899 | No independent language standard |
| Input | Human-written source | Binary machine code + analysis metadata |
| Types | Defined by C implementation and program | Recovered types plus Ghidra synthetic types |
| Variable names | Chosen by programmer | Recovered from symbols or synthesized |
| `undefined8` | Not standard C | Unknown 8-byte data |
| `FUN_...` | Legal identifier but unusual | Auto-generated unknown function |
| `LAB_...` | Could be label | Auto-generated control-flow label |
| `DAT_...` | Could be identifier | Auto-generated unknown data symbol |
| `CONCAT44` | Ordinary call only if you define it | Internal decompiler operation notation |
| Guaranteed compilable | C source should be | No |
| Reflects original formatting | Yes | No |
| Reflects original source exactly | It *is* source | Almost never |

---

# Ghidra-C versus pseudocode

Pseudocode intentionally throws away machine details before implementation.

Ghidra-C tries to reconstruct high-level meaning **after** machine details have already destroyed some source information.

They approach the same abstraction boundary from opposite directions.

Pseudocode says:

> Do not worry about representation yet.

Ghidra says:

> Representation is all I have. Please help me work backwards.

This is why both belong in `other/`, but for almost opposite reasons.

---

# Ghidra-C versus AI prompts

The museum now has another fun comparison.

An AI prompt is fuzzy because its instructions do not have rigid formal semantics.

Ghidra-C is fuzzy for a different reason:

The underlying binary semantics are rigid, but the **high-level interpretation is inferred**.

AI prompt:

```text
clear human meaning → probabilistic machine interpretation
```

Ghidra decompiler:

```text
precise machine behaviour → uncertain human-level reconstruction
```

They are conceptual mirror images.

The `other/` cabinet is becoming dangerously powerful.

---

# Can you compile Ghidra output?

Sometimes pieces of it, yes.

A simple decompile may already be close to valid C:

```c
int add(int param_1, int param_2)
{
    return param_1 + param_2;
}
```

More complex output may require:

- defining Ghidra synthetic types;
- replacing internal pseudo-functions;
- reconstructing headers;
- replacing address-based globals;
- fixing prototypes;
- rebuilding data structures;
- accounting for ABI/runtime functions.

So:

> Ghidra outputs C.

is useful shorthand.

But:

> Ghidra recovers a compilable C source tree.

is false in general.

---

# Why use C syntax at all?

C is unusually suitable as a decompiler presentation language because it can naturally express:

- pointers;
- address arithmetic;
- integer widths;
- structures;
- casts;
- arrays;
- function calls;
- low-level memory access;
- bit operations;
- labels and `goto`.

It lives close enough to machine abstractions while still being widely readable.

A Python-shaped decompiler would spend half its time inventing syntax for pointer arithmetic.

C already owns the toolbox.

---

# The horror of indirect calls

Machine code may call through an address:

```c
(*function_pointer)(argument);
```

If Ghidra does not know the target set, you may see decompiled indirect calls with weak type information.

This is common in:

- C++ virtual dispatch;
- callback tables;
- interpreters;
- plugin systems;
- state machines.

Recovering the function-pointer type and surrounding structure can turn these from gibberish into architecture.

Reverse engineering often means discovering the object model from the footprints it left in memory.

---

# C++ vtables through Ghidra's eyes

A C++ method call such as:

```cpp
object->draw();
```

may become machine code that:

1. loads a vtable pointer;
2. loads a function pointer from an offset;
3. performs an indirect call.

Early decompilation may show something like:

```c
(**(code **)(*(long *)param_1 + 0x20))(param_1);
```

That is hideous.

It is also a brutally honest low-level description of virtual dispatch.

Once class structures and types are recovered, the display can improve.

Ghidra-C is excellent at showing where high-level sugar goes when the compiler digests it.

---

# Optimizers are source-code shredders with excellent intentions

A compiler's optimizer does not care whether reverse engineers can later recover:

```c
for (int i = 0; i < 10; ++i)
```

It cares about producing efficient machine code while preserving observable behaviour.

Therefore:

- loop counters can vanish;
- branches can become conditional moves;
- functions can disappear into callers;
- constants can propagate;
- variables can merge;
- dead code can disappear;
- arithmetic can become algebraically unrecognisable.

The better the optimizer, the more the decompiler sometimes has to reconstruct from ashes.

Performance engineering and source archaeology are natural enemies who happen to share an office.

---

# Decompiling malware

Ghidra is widely used for malware analysis and security research.

In that context, nobody expects:

```c
void steal_passwords(void)
```

to survive helpfully as a symbol.

Malware may additionally use:

- packing;
- obfuscation;
- control-flow flattening;
- self-modifying code;
- encryption;
- anti-debugging;
- deliberately misleading constructs.

Decompiler output can become extremely ugly because the binary was intentionally designed to resist exactly this process.

Ghidra-C's weirdest specimens often come from programs actively refusing museum admission.

---

# Decompiler mistakes are useful mistakes

Even incorrect output can guide investigation.

If Ghidra shows:

```c
undefined8
```

that tells you what it *doesn't* know.

If it emits:

```c
CONCAT44
```

that highlights width reconstruction.

If the prototype has six parameters but the callers only seem to set two meaningful values, that is evidence the signature may be wrong.

Good reverse engineers read uncertainty itself.

`undefined8` is not garbage.

It is a labelled blank on the map.

---

# Version differences

Decompiler algorithms improve over time.

A binary decompiled in Ghidra 9 may look different in Ghidra 12.

Improvements can affect:

- type recovery;
- control-flow structuring;
- calling conventions;
- architecture semantics;
- simplification rules;
- analysis passes.

So even if:

```text
binary = identical
```

the decompiled C can change after upgrading Ghidra.

Again:

not a language implementation executing a source program.

An analysis engine producing a model.

---

# The current Ghidra release

At the time this exhibit was written in September 2026, GitHub lists:

**Ghidra 12.1.3**

as the latest release.

This matters because screenshots and decompilation examples from older tutorials may differ from modern output.

The underlying concepts remain recognisable:

```text
FUN_
DAT_
LAB_
param_
local_
undefined*
CONCAT*
SUB*
```

are part of the visual dialect reverse engineers learn to read.

---

# Shitpost cabinet

## `undefined8`

C:

```c
uint64_t value;
```

Ghidra:

```c
undefined8 uVar3;
```

Translation:

> Eight bytes were definitely involved. Please stop asking emotionally difficult questions.

---

## `FUN_00101230`

Original programmer:

```c
calculate_tax()
```

Stripper/linker:

```text
:)
```

Ghidra:

```text
FUN_00101230
```

Reverse engineer:

```text
I have named you Steve until further notice.
```

---

## `CONCAT44`

Nothing says:

> perhaps my type information is incomplete

quite like:

```c
return CONCAT44(uVar1, uVar2);
```

appearing in what was probably once a clean structure return.

---

## Decompiled C++ template code

C++ source:

```cpp
std::sort(values.begin(), values.end());
```

Decompiler:

```text
the bones of seventeen abstractions arranged in a pentagram
```

The binary was always going to win.

---

# Learn more

Ghidra-C is not a programming language course because, importantly, **it is not a programming language**.

Useful Ghidra resources:

- [Official Ghidra repository](https://github.com/NationalSecurityAgency/ghidra)
- [Ghidra documentation](https://ghidra.re/ghidra_docs/)
- [Introduction to Ghidra student guide](https://ghidra.re/ghidra_docs/GhidraClass/Beginner/Introduction_to_Ghidra_Student_Guide.html)
- [Ghidra API: `DecompileResults`](https://ghidra.re/ghidra_docs/api/ghidra/app/decompiler/DecompileResults.html)
- [Ghidra P-Code operation reference](https://ghidra.re/ghidra_docs/languages/html/pcodedescription.html)

Related museum exhibits:

- [C](../languages/c.md)
- [C++](../languages/cpp.md)
- [Pseudocode](pseudocode.md)

---

# Museum summary

Ghidra-C looks like a programming language because that is useful.

It has:

```c
if
while
return
*
&
->
[]
casts
function calls
```

But mixed among them are archaeological labels:

```text
FUN_00101230
DAT_00104020
LAB_00101390
param_1
local_28
undefined8
CONCAT44
SUB84
```

Those tell a different story.

Normal C starts with human intent and becomes machine code.

Ghidra-C starts with machine code and tries to recover human intent.

The process is necessarily lossy.

A decompiler is therefore not a compiler played backwards.

Compilers are blenders.

Ghidra is standing over the smoothie with tweezers, saying:

> I'm reasonably confident this used to be a strawberry.
