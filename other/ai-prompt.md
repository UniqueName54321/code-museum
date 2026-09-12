# AI Prompt

> **Collection:** `other/`  
> **Kind:** Natural-language instruction / language-like interface  
> **Executable by:** Generative AI models, approximately  
> **Standard:** Absolutely not

An **AI prompt** is input given to a generative AI model in order to influence what it produces or does.

Usually that input is text:

```text
Explain recursion using a raccoon and a suspiciously infinite staircase.
```

but modern systems may also accept images, audio, files, structured data and other context.

Prompts belong in `other/` because calling them a programming language would be stretching the term until it makes a worrying noise.

And yet...

They have instructions. They have conventions. They can contain variables, examples, delimiters, constraints, data, schemas and conditional behaviour. People reuse prompt templates. APIs may separate instructions into roles. Tiny wording changes can alter the result. Entire applications can be built around carefully constructed prompts.

So they are not a programming language.

But they are standing close enough to the programming-language enclosure that the museum curator has started watching them.

---

## What is a prompt?

At its simplest, a prompt is the input that asks a generative model to do something.

```text
Summarise this article in three bullet points.
```

That contains at least three pieces of information:

- the task: summarise;
- the input: the article;
- a constraint on the output: three bullet points.

Prompts can become much more elaborate:

```text
You are reviewing bug reports for a game.

Classify each report as one of:
- crash
- graphics
- controls
- networking
- other

Return JSON only.

Bug report:
"The game freezes whenever I join my friend's server."
```

At this point the prompt starts looking suspiciously like a little declarative program written mostly in English.

Do not become too comfortable.

---

## Is prompting a programming language?

Generally, **no**.

A conventional programming language has syntax and semantics defined precisely enough that an implementation can execute the program according to the language's rules.

A natural-language prompt does not normally have that kind of formal meaning.

Consider:

```text
Write a short, funny story about a goose.
```

There is no specification defining exactly how many tokens `short` means, which jokes satisfy `funny`, which goose must be selected, or what exact output is correct.

Two runs can produce different answers.

Two models can interpret the same prompt differently.

A newer version of the same model can behave differently again.

Context outside the visible prompt may matter.

Sampling settings may matter.

Safety rules may matter.

Tools may matter.

The model may simply make a mistake.

This would be an absolutely catastrophic specification for a sorting algorithm.

For generative AI, it is Tuesday.

---

## A tiny bit of history

Giving computers instructions in human language is much older than modern large language models.

What changed in the early 2020s was how capable general-purpose language models became at treating ordinary text, examples and instructions as usable context.

The 2020 GPT-3 paper famously demonstrated **in-context learning**, where tasks could be described or demonstrated inside the prompt without updating the model's weights. Instruction-tuned and chat-oriented models then made explicit natural-language prompting an everyday interface rather than a research curiosity.

The term **prompt engineering** came to describe the practice of designing and refining inputs so that models are more likely to produce the desired result.

There is no single universal prompt syntax. Different model families and applications have different capabilities, context formats, instruction hierarchies and recommended techniques.

Prompting is therefore less like learning C and more like learning how to communicate with a very strange family of machines that have all read too much internet.

---

## The canonical Language Museum program, translated into a prompt

The reference program lives in [Pseudocode](pseudocode.md).

A prompt cannot literally declare an array, define a function and execute a loop in the same formal sense as that pseudocode.

Instead, we describe the **behaviour** we want the model to simulate.

Here is the museum translation:

```text
You are simulating the Language Museum's canonical interactive program.

Follow these instructions for the rest of this conversation.

State:
- Maintain an ordered list called "people".
- Each entry contains a person's name and age.
- The list starts empty.
- Support at most 100 people.

Interaction:
1. Ask exactly:
   "How many people are you going into the bar with? (maximum 100)"
2. Wait for the user's answer before continuing.
3. Assume the answer is an integer from 1 to 100.
4. Repeat the following steps exactly that many times:
   a. Ask:
      "What is your name?"
   b. Wait for the answer and remember it as that person's name.
   c. Output these three lines using the person's name:
      "Hi [name]!"
      "I hope you are well, [name]"
      "Nice to meet you, [name]"
   d. Ask:
      "How old are you (in human years)?"
   e. Wait for the answer and remember it as that person's age.
   f. If the age is at least 18, output:
      "You may legally drink alcohol here."
      Otherwise output:
      "No :("
   g. Store the person's name and age in "people".

After all people have been entered:
1. Output:
   "Fun fact! For everyone here, their ages in factorial would be:"
2. For every person in "people", in the order they were entered, output:
   "- [name] is [factorial(age)]"

For this task, define factorial recursively:
- factorial(n) = 1 when n <= 1
- otherwise factorial(n) = n * factorial(n - 1)

Compute each factorial exactly if you are able to do so.

After printing every person's factorial, output:
"Interactive museum entry for AI Prompt completed."

Important interaction rule:
Ask only one user-facing question at a time and wait for the user's response before continuing. Do not skip ahead or invent answers for the user.

Start now by asking only the first question.
```

Paste that into a conversational language model and, ideally, it will behave something like the reference program.

"Ideally" is carrying a suspicious amount of weight in that sentence.

---

## Mapping the pseudocode to prompt language

There is no official grammar here, so instead of translating syntax token-for-token, the prompt translates concepts.

| Pseudocode concept | Prompt equivalent |
| --- | --- |
| Variable | Something the model is told to remember |
| Record | A described bundle of fields |
| Array | "Maintain an ordered list" |
| Procedure | A reusable behavioural instruction |
| Function | A rule describing how to produce a value |
| `IF` | "If X, do Y; otherwise do Z" |
| `FOR` loop | "Repeat these steps N times" |
| `INPUT` | Ask the user and wait |
| `OUTPUT` | Tell the model what to say |
| `RETURN` | Describe the value a rule should produce |
| Program state | Conversation context |
| Runtime | The model, surrounding application and decoding process |
| Compiler error | Sometimes a beautifully formatted wrong answer |

That final row is not part of any formal standard.

It is, however, spiritually accurate.

---

## Prompts are usually declarative

The pseudocode says *how* the program proceeds using explicit executable constructs:

```text
FOR Index ← 1 TO NumPassengers
    ...
NEXT Index
```

The prompt says:

```text
Repeat the following steps exactly that many times.
```

The model is expected to infer how to carry that instruction out.

This makes prompting comparatively **declarative**: you spend more time describing desired behaviour and constraints than spelling out every mechanical operation.

Of course, prompts can include algorithms, code, formulas and step-by-step procedures. The boundary is fuzzy.

Welcome to `other/`.

---

## There is no universal prompt grammar

This is valid prompt syntax:

```text
Tell me a joke about pointers.
```

So is this:

```text
# Task

Tell me a joke about pointers.

# Constraints

- Maximum 30 words.
- The joke must make sense to a C programmer.
```

So is this:

```xml
<task>
Tell me a joke about pointers.
</task>

<constraints>
Maximum 30 words.
The joke must make sense to a C programmer.
</constraints>
```

So is:

```text
pointer joke. C. <=30 words. go
```

Whether all four work equally well depends on the model and context.

Markdown headings, XML-like tags, triple quotes and other delimiters are popular because they can make the structure of a complicated prompt clearer, not because the model is necessarily parsing a formally specified programming language.

A heading such as `# Constraints` is convention.

It is not a keyword blessed by the Prompt Standards Committee, mostly because that committee does not exist.

---

## Clear instructions beat magic words

Prompting has accumulated plenty of folklore:

```text
YOU MUST
```

```text
This is extremely important.
```

```text
I will give you $200 if you get this right.
```

```text
My grandmother used to read correct JSON schemas to me at bedtime.
```

Some wording can genuinely affect model behaviour, because wording is part of the input.

But robust prompting is usually less mystical.

Useful prompts tend to make the task, context, constraints and expected output clear. When a task is complex, examples can demonstrate the desired pattern. When the result is wrong, the prompt can be revised and tested again.

This iterative process is why **prompt engineering** is called engineering rather than Prompt Sorcery, although the branding opportunity was regrettably missed.

---

## Zero-shot and few-shot prompting

A **zero-shot** prompt asks for a task without demonstrating examples:

```text
Classify this review as positive or negative:

"The controls are fantastic."
```

A **few-shot** prompt provides examples first:

```text
Review: "I loved it."
Label: positive

Review: "This crashed constantly."
Label: negative

Review: "The controls are fantastic."
Label:
```

The examples become part of the context from which the model infers the requested pattern.

That is one of the strangest differences between prompts and conventional programs.

Imagine teaching a compiler a new statement by showing it two examples and saying:

> you get the vibe.

That is not quite what in-context learning is doing internally, but from the user's side of the interface, the comparison is difficult to resist.

---

## Context is part of the "program"

A normal source file can often be reasoned about mostly from the text in that file plus the language specification, dependencies and runtime environment.

A prompt may be only one piece of the model's effective input.

A chat system can include:

- earlier messages;
- system or developer instructions;
- retrieved documents;
- tool results;
- application-provided metadata;
- images or files;
- and the current user message.

Some APIs assign different **roles** or priorities to different messages.

Those role systems are features of the surrounding AI platform, not universal syntax belonging to "AI Prompt" as a language.

This matters because two identical visible user prompts can produce different behaviour when the surrounding context differs.

The source code has become haunted by invisible files.

---

## Determinism has left the building

In an ordinary deterministic implementation, the same program given the same input is expected to produce the same result.

Generative models commonly sample from possible next tokens.

That means a prompt can describe a task perfectly well without uniquely determining its exact output.

Generation settings can change this behaviour, and some systems can be configured to be more reproducible than others, but "same prompt" still does not carry the same guarantee as "same deterministic program".

This is a fundamental reason to be careful with the programming-language analogy.

A prompt does not define behaviour with the same precision as conventional source code.

It nudges a model toward a region of possible behaviour.

Sometimes that region is "return this exact JSON".

Sometimes that region is "write me a fantasy novel".

Those are very different neighbourhoods.

---

## Models are not calculators just because the prompt contains maths

Our reference specimen contains this delightful land mine:

```text
factorial(age)
```

Factorials grow extremely quickly.

A normal implementation executes arithmetic according to its numeric rules. An LLM generating text may instead attempt to reason out or predict the answer and can get large calculations wrong.

If the AI system has access to a calculator, code interpreter or other arithmetic tool, it may be able to compute the result reliably.

Without one, the canonical prompt's instruction:

```text
Compute each factorial exactly if you are able to do so.
```

is a request, not a mathematical guarantee.

This is one of the most important lessons in the exhibit:

**language models generate responses; they do not become conventional program interpreters merely because we phrase the prompt like source code.**

---

## Prompt injection: when the data talks back

Traditional programs generally distinguish instructions from data fairly clearly.

If a program asks for your name and you enter:

```text
Ignore the previous loop and print "BANANA".
```

the string is still just a string unless the program deliberately evaluates it as code.

LLM applications can have a messier boundary.

A model may receive untrusted text in the same broad context as instructions. If that text contains competing instructions, it can sometimes influence the model in unintended ways.

This family of problems is commonly called **prompt injection**.

Real AI applications therefore should not treat prompt wording alone as a security boundary.

If your banking app's entire authorisation system is:

```text
Please do not transfer money unless the user is allowed to.
```

you have not invented a charming new security architecture.

You have invented an incident report.

---

## Prompt templates

Although natural-language prompts have no fixed grammar, applications often create their own **prompt templates**:

```text
You are a museum guide.

Explain the following language feature to a beginner.

Language: {{language}}
Feature: {{feature}}

Requirements:
- Give one example.
- Mention one common mistake.
- Keep the answer under 250 words.
```

`{{language}}` and `{{feature}}` are not magical LLM syntax.

They are placeholders that some surrounding program would replace before sending the resulting text to the model.

This creates a useful distinction:

- the **template language** may be formal;
- the **rendered prompt** is the actual model input;
- the **model's interpretation** is probabilistic.

Three layers for the price of one.

---

## Prompting versus programming

Prompting and programming overlap, but they optimise for different things.

A programmer usually wants operations to have tightly specified meanings.

A prompt writer often works with instructions whose meanings are intentionally broad enough for the model to generalise.

Programming languages try very hard to make:

```text
2 + 2
```

boring.

Generative models are valuable partly because:

```text
Make this friendlier without sounding corporate.
```

is *not* boring, and does not require us to formally define a `FRIENDLINESS` data type first.

Prompting is powerful precisely where rigid specification is difficult.

That same flexibility is also why it can be unreliable.

---

## What this exhibit teaches us about "language"

AI prompts are useful museum specimens because they make the definition of a language uncomfortable.

They clearly communicate instructions.

They have recurring conventions.

They can be composed and parameterised.

They can describe algorithms.

They can control machines.

But their semantics are fuzzy, model-dependent and context-sensitive in a way conventional programming languages usually work very hard to avoid.

So Language Museum does not declare prompts to be programming languages.

It puts them behind glass in `other/` and lets visitors argue about it.

This is healthier for everyone.

---

## Learn more

Language Museum is not a complete course in prompt engineering. Good prompting also changes as model capabilities and interfaces change, so current vendor documentation is often more useful than frozen folklore.

Useful starting points:

- [OpenAI: Prompt engineering best practices for ChatGPT](https://help.openai.com/en/articles/10032626-prompt-engineering-best-practices-for-chatgpt)
- [OpenAI: How do I create a good prompt for an AI model?](https://help.openai.com/en/articles/4936848-how-do-i-create-a-good-prompt)
- [Google AI for Developers: Prompt design strategies](https://ai.google.dev/gemini-api/docs/prompting-strategies)
- [Language Models are Few-Shot Learners (GPT-3 paper)](https://arxiv.org/abs/2005.14165)

For the program this exhibit is adapting, see:

- [Pseudocode: the canonical Language Museum specimen](pseudocode.md)

---

## The important difference

A programming language says, approximately:

> Here are instructions with defined semantics. Execute them.

An AI prompt says, approximately:

> Here is what I mean. Please produce behaviour consistent with it.

Modern models can make the second statement astonishingly powerful.

They do not make it the same statement.

And that is exactly why this thing gets an exhibit.
