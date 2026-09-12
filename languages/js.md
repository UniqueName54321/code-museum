# JavaScript

> **Collection:** `languages/`  
> **Kind:** General-purpose programming language, most famously the language of the Web  
> **Paradigms:** Multi-paradigm: imperative, functional, prototype-based object-oriented  
> **First appeared:** 1995  
> **Created by:** Brendan Eich at Netscape  
> **Standardized as:** ECMAScript / ECMA-262  
> **File extensions:** `.js`, `.mjs`, `.cjs` depending on environment and module system  
> **Museum target:** ECMAScript 2026  
> **Reference exhibit:** [`../other/pseudocode.md`](../other/pseudocode.md)

JavaScript is the programming language that escaped from a web browser and discovered it could run basically everywhere.

It began as a scripting language for web pages. It now runs in browsers, servers, command-line tools, desktop applications, mobile applications, embedded systems, edge platforms, databases, build systems, game engines, and probably at least one toaster somebody deeply regrets connecting to npm.

But there is an important rule for this exhibit:

> **JavaScript is not the browser. JavaScript is not Node.js. JavaScript is not Deno.**

The language is standardized as **ECMAScript**.

Browsers, Node.js, Deno and other environments provide *hosts* around that language, each with additional APIs.

That distinction matters immediately because the Language Museum program needs **input and output**, and ECMAScript itself does not define a universal "ask the human a question in a terminal" function.

So this exhibit has three canonical implementations:

1. **browser JavaScript**
2. **Node.js**
3. **Deno**

Same language.

Different habitats.

---

# JavaScript versus ECMAScript

These terms are related but not identical.

**ECMAScript** is the standardized language defined by **ECMA-262** and maintained through Ecma International's TC39 process.

**JavaScript** is the name everybody actually uses for the language in practice.

As of this exhibit, the current published standard is **ECMAScript 2026**, the 17th edition of ECMA-262.

A host environment then adds facilities around ECMAScript.

For example:

| Feature | ECMAScript language? | Browser | Node.js | Deno |
| --- | --- | --- | --- | --- |
| `let`, `const`, functions | Yes | Yes | Yes | Yes |
| `Array` | Yes | Yes | Yes | Yes |
| `BigInt` | Yes | Yes | Yes | Yes |
| `console.log()` | Host API, widely provided | Yes | Yes | Yes |
| DOM / `document` | No | Yes | No by default | No by default |
| `window.prompt()` | No | Yes | No | No `window`, but Deno supplies global `prompt()` |
| `process.stdin` | No | No | Yes | Node compatibility available |
| `Deno.*` | No | No | No | Yes |

This is why saying:

> JavaScript has `document.querySelector()`

is slightly wrong.

Browsers have a DOM API exposed to JavaScript.

JavaScript itself does not know what a `<div>` is.

---

# A tiny bit of history

JavaScript was created by Brendan Eich at Netscape in 1995 during the early browser wars.

Its famously short initial development period has become part of programming folklore.

The language went through names including **Mocha** and **LiveScript** before becoming JavaScript, a name that has caused several geological eras of beginners to wonder whether it is related to Java.

It is not Java.

Java and JavaScript are related in roughly the sense that "car" and "carpet" are related.

JavaScript was standardized through Ecma as **ECMAScript** beginning in the 1990s.

A particularly important modern milestone was **ECMAScript 2015**, commonly called **ES6**, which introduced major features such as:

- `let` and `const`;
- classes;
- arrow functions;
- promises;
- modules;
- template literals;
- destructuring;
- and much more.

Modern JavaScript is substantially different from the tiny browser scripting language of 1995.

---

# First impressions

```javascript
const name = "Mika";
let age = 22;

if (age >= 18) {
    console.log(`${name} passes the age check.`);
} else {
    console.log("No :(");
}
```

Several characteristic pieces appear immediately.

### Braces define blocks

```javascript
if (condition) {
    // block
}
```

Unlike Python, indentation is for humans rather than the parser.

### `const` and `let`

Modern JavaScript normally uses:

```javascript
const fixedBinding = 10;
let changingBinding = 10;
```

`const` means the *binding* cannot later be reassigned.

It does not make an object deeply immutable:

```javascript
const person = { name: "Mika" };
person.name = "Ren"; // allowed
```

The variable still refers to the same object. The object's contents changed.

JavaScript also has `var`, the older function-scoped declaration mechanism.

Modern code generally prefers `const` and `let`.

### Semicolons are often optional

JavaScript has **Automatic Semicolon Insertion**.

This means:

```javascript
const x = 1
const y = 2
```

works.

It does **not** mean semicolons never matter.

Welcome to a language where the phrase "usually optional punctuation with edge cases" has been allowed to survive for three decades.

This exhibit uses semicolons consistently.

---

# The canonical program: browser version

This version is intended to be pasted into a browser's developer console.

The browser provides `prompt()` for input and the console provides `console.log()` for output.

```javascript
const LANGUAGE = "JavaScript (browser)";
const MAX_PASSENGERS = 100;

function greet(name) {
    console.log(`Hi ${name}!`);
    console.log(`I hope you are well, ${name}`);
    console.log(`Nice to meet you, ${name}`);
}

function ageCheck(age) {
    if (age >= 18) {
        return "You may legally drink alcohol here.";
    } else {
        return "No :(";
    }
}

function factorial(num) {
    const n = BigInt(num);

    if (n <= 1n) {
        return 1n;
    }

    return n * factorial(n - 1n);
}

const people = [];

const numPassengers = Number.parseInt(
    prompt(
        `How many people are you going into the bar with? ` +
        `(maximum ${MAX_PASSENGERS})`
    ),
    10
);

for (let index = 0; index < numPassengers; index++) {
    const name = prompt("What is your name?");

    greet(name);

    const age = Number.parseInt(
        prompt("How old are you (in human years)?"),
        10
    );

    console.log(ageCheck(age));

    people.push({ name, age });
}

console.log(
    "Fun fact! For everyone here, their ages in factorial would be:"
);

for (const person of people) {
    console.log(`- ${person.name} is ${factorial(person.age)}`);
}

console.log(
    `Interactive museum entry for ${LANGUAGE} completed.`
);
```

### A browser-specific footnote

`prompt()` is **not part of ECMAScript**.

It is a browser API traditionally exposed as:

```javascript
window.prompt(...)
```

Browsers also expose it as the global:

```javascript
prompt(...)
```

The function opens a modal input dialog.

Modern production websites usually build their own interface with HTML and DOM APIs instead of building an application out of `prompt()` boxes like it is 1998 and the dancing baby has just loaded.

For a museum specimen, however, `prompt()` is wonderfully compact.

---

# The canonical program: Node.js version

Node.js does **not** provide browser `prompt()`.

For terminal input, this exhibit uses Node's stable promise-based `node:readline/promises` API.

Save this as `museum.mjs`:

```javascript
import { createInterface } from "node:readline/promises";
import { stdin, stdout } from "node:process";

const LANGUAGE = "JavaScript (Node.js)";
const MAX_PASSENGERS = 100;

const rl = createInterface({
    input: stdin,
    output: stdout,
});

function greet(name) {
    console.log(`Hi ${name}!`);
    console.log(`I hope you are well, ${name}`);
    console.log(`Nice to meet you, ${name}`);
}

function ageCheck(age) {
    if (age >= 18) {
        return "You may legally drink alcohol here.";
    } else {
        return "No :(";
    }
}

function factorial(num) {
    const n = BigInt(num);

    if (n <= 1n) {
        return 1n;
    }

    return n * factorial(n - 1n);
}

const people = [];

const numPassengers = Number.parseInt(
    await rl.question(
        `How many people are you going into the bar with? ` +
        `(maximum ${MAX_PASSENGERS}) `
    ),
    10
);

for (let index = 0; index < numPassengers; index++) {
    const name = await rl.question("What is your name? ");

    greet(name);

    const age = Number.parseInt(
        await rl.question("How old are you (in human years)? "),
        10
    );

    console.log(ageCheck(age));

    people.push({ name, age });
}

console.log(
    "Fun fact! For everyone here, their ages in factorial would be:"
);

for (const person of people) {
    console.log(`- ${person.name} is ${factorial(person.age)}`);
}

console.log(
    `Interactive museum entry for ${LANGUAGE} completed.`
);

rl.close();
```

Run it with:

```sh
node museum.mjs
```

As of **12 September 2026**, Node.js **26** is the Current release line and Node.js **24 "Krypton"** is an LTS line.

The code above uses ES modules and top-level `await`.

Node also supports the older CommonJS module system, where imports traditionally look like:

```javascript
const fs = require("node:fs");
```

The coexistence of ES modules and CommonJS is one of those historical layers that makes a mature ecosystem feel less like a clean blueprint and more like a city where every century refused to demolish the previous century.

---

# The canonical program: Deno version

Deno is another JavaScript and TypeScript runtime.

Unlike Node, Deno provides simple global terminal prompt helpers, including `prompt()`.

That makes the specimen amusingly close to the browser version:

```javascript
const LANGUAGE = "JavaScript (Deno)";
const MAX_PASSENGERS = 100;

function greet(name) {
    console.log(`Hi ${name}!`);
    console.log(`I hope you are well, ${name}`);
    console.log(`Nice to meet you, ${name}`);
}

function ageCheck(age) {
    if (age >= 18) {
        return "You may legally drink alcohol here.";
    } else {
        return "No :(";
    }
}

function factorial(num) {
    const n = BigInt(num);

    if (n <= 1n) {
        return 1n;
    }

    return n * factorial(n - 1n);
}

const people = [];

const numPassengers = Number.parseInt(
    prompt(
        `How many people are you going into the bar with? ` +
        `(maximum ${MAX_PASSENGERS})`
    ),
    10
);

for (let index = 0; index < numPassengers; index++) {
    const name = prompt("What is your name?");

    greet(name);

    const age = Number.parseInt(
        prompt("How old are you (in human years)?"),
        10
    );

    console.log(ageCheck(age));

    people.push({ name, age });
}

console.log(
    "Fun fact! For everyone here, their ages in factorial would be:"
);

for (const person of people) {
    console.log(`- ${person.name} is ${factorial(person.age)}`);
}

console.log(
    `Interactive museum entry for ${LANGUAGE} completed.`
);
```

Run it with:

```sh
deno run museum.js
```

Deno's `prompt()` uses terminal input when run from the CLI and requires no import.

As of this exhibit, the current Deno release family is **Deno 2.9**.

Deno also supports TypeScript as a first-class input language, but this exhibit deliberately uses JavaScript because TypeScript deserves its own museum page rather than being smuggled into this one wearing a fake moustache.

---

# Why three implementations?

Because this:

```javascript
if (age >= 18) {
    return "yes";
}
```

is JavaScript.

This:

```javascript
document.querySelector("#age")
```

is JavaScript **using the browser's DOM API**.

This:

```javascript
process.stdin
```

is JavaScript **using Node's process API**.

This:

```javascript
Deno.readTextFile(...)
```

is JavaScript **using a Deno API**.

If we collapse all of those into "JavaScript features", we lose one of the most important concepts for understanding the language.

The ECMAScript specification defines the core language and built-in objects.

The host defines the surrounding world.

---

# What changed from the pseudocode?

## Records became ordinary objects

The pseudocode defines a `Person` record.

JavaScript can represent one with an object literal:

```javascript
const person = {
    name: "Mika",
    age: 22,
};
```

The exhibit therefore stores:

```javascript
people.push({ name, age });
```

JavaScript objects are much more general than simple records. They are a central part of the language's prototype-based object system.

---

## The fixed array became a dynamic `Array`

```javascript
const people = [];
```

creates an empty array.

```javascript
people.push(person);
```

adds another element.

Like Python lists, JavaScript arrays grow dynamically.

They also have a large collection of higher-order methods:

```javascript
people.map(...)
people.filter(...)
people.find(...)
people.reduce(...)
```

The museum implementation sticks to ordinary loops so the translation remains easy to compare.

---

## Functions are first-class values

```javascript
function greet(name) {
    ...
}
```

defines a function.

JavaScript also has function expressions:

```javascript
const greet = function (name) {
    ...
};
```

and arrow functions:

```javascript
const greet = (name) => {
    ...
};
```

Functions are values. They can be stored, passed around and returned.

This is fundamental to JavaScript's style, especially for callbacks, event handlers and asynchronous programming.

---

# `Number` is not an integer type

JavaScript's ordinary numeric type is:

```javascript
Number
```

A JavaScript `Number` is an IEEE 754 double-precision floating-point value.

That means:

```javascript
1
1.5
-20
Infinity
NaN
```

are all values of the same numeric type.

This has a famous consequence.

JavaScript can exactly represent integers only up to:

```javascript
Number.MAX_SAFE_INTEGER
```

which is:

```text
9007199254740991
```

Now recall our canonical specimen's factorial.

```text
18! = 6402373705728000
19! = 121645100408832000
```

`18!` is still within JavaScript's safe integer range.

`19!` is not.

So an implementation using ordinary `Number` arithmetic would stop guaranteeing exact factorials for a completely normal human age.

The museum specimen has found JavaScript's pressure point almost immediately.

Beautiful.

---

# `BigInt` saves the factorial

Modern JavaScript has a separate arbitrary-size integer primitive called **BigInt**.

BigInt literals have an `n` suffix:

```javascript
1n
42n
900719925474099100000000000n
```

Our factorial therefore converts the age:

```javascript
const n = BigInt(num);
```

and recursively computes with BigInts:

```javascript
return n * factorial(n - 1n);
```

This preserves exact integer factorials far beyond the safe range of `Number`.

There is one important rule:

```javascript
1n + 1
```

throws a `TypeError`.

You cannot casually mix `BigInt` and `Number` arithmetic. You must convert deliberately.

JavaScript has two numeric worlds and expects you to carry a passport.

---

# `==` versus `===`

JavaScript has two famous equality families.

```javascript
a == b
```

performs **loose equality**, which may convert values before comparing them.

```javascript
a === b
```

performs **strict equality**, without that coercion.

For example:

```javascript
0 == "0"    // true
0 === "0"   // false
```

Modern JavaScript style generally prefers `===` unless coercive equality is specifically intended.

The language's implicit conversion rules are powerful, historical, and the source of enough jokes to power several medium-sized cities.

---

# Type coercion

JavaScript frequently converts values between types.

```javascript
"5" + 1
```

produces:

```text
"51"
```

because string concatenation wins.

But:

```javascript
"5" - 1
```

produces:

```text
4
```

because subtraction expects numbers.

This is not randomness. JavaScript has defined coercion rules.

The problem is that understanding those rules sometimes feels less like programming and more like reconstructing maritime law from shipwreck debris.

---

# `null` and `undefined`

JavaScript has both:

```javascript
null
```

and:

```javascript
undefined
```

Broadly:

- `undefined` often means a value has not been supplied or assigned;
- `null` is commonly used as an explicit "no value" marker.

For historical reasons:

```javascript
typeof null
```

returns:

```text
"object"
```

This is a famous bug preserved for web compatibility.

Changing it now would break old websites.

The web never forgets.

The web does not forgive.

The web has production code from 2003.

---

# Prototype-based objects

JavaScript's object model is based on **prototypes**.

Objects can inherit behaviour from other objects through a prototype chain.

Modern JavaScript has `class` syntax:

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

but JavaScript classes are built on top of the language's prototype system.

This makes JavaScript object-oriented, but not in quite the same underlying way as class-centric languages such as Java or C++.

---

# Asynchronous JavaScript

JavaScript is famous for asynchronous programming.

A promise represents a future result:

```javascript
const response = await fetch("https://example.com/");
```

`async` and `await` make promise-based code easier to read.

Our Node implementation already uses this:

```javascript
const name = await rl.question("What is your name? ");
```

`rl.question()` returns a promise.

`await` pauses that async flow until the answer is available.

The broader event-loop model varies somewhat by host environment, because once again:

**the language is not the host.**

---

# Modules

Modern ECMAScript modules use:

```javascript
export function greet(name) {
    ...
}
```

and:

```javascript
import { greet } from "./greet.js";
```

Browsers support ES modules.

Node supports ES modules and also its older CommonJS system.

Deno strongly embraces ES modules and also provides substantial Node/npm compatibility.

This is another case where "JavaScript modules" has both a language-level answer and a historical ecosystem answer.

---

# Browser JavaScript

In the browser, JavaScript can interact with the Web Platform.

Common APIs include:

- the DOM;
- events;
- `fetch`;
- Web Storage;
- timers;
- WebSockets;
- Canvas;
- Web Audio;
- Workers;
- browser navigation.

Example:

```javascript
document.querySelector("button").addEventListener("click", () => {
    console.log("button acquired");
});
```

None of `document`, the HTML button, or the click event is defined by ECMA-262.

They come from browser standards and APIs.

JavaScript is merely the language currently holding the wrench.

---

# Node.js

Node.js was created by Ryan Dahl and initially released in 2009.

It brought JavaScript to general-purpose server and command-line programming using the V8 JavaScript engine.

Node provides APIs for things browsers traditionally do not expose directly, such as:

- filesystem access;
- processes;
- TCP and other networking;
- operating-system information;
- terminal streams;
- server creation.

It also became inseparable from the rise of **npm**, whose package ecosystem became enormous enough that saying "there's probably a package for that" stopped being comedy and became a statistical statement.

---

# Deno

Deno was also created by Ryan Dahl, years after Node.

It was designed partly in response to lessons learned from Node's early architecture.

Modern Deno provides:

- JavaScript and first-class TypeScript execution;
- Web Platform APIs;
- a built-in toolchain;
- explicit permission controls for many sensitive capabilities;
- npm and Node compatibility;
- the `Deno.*` runtime API.

The philosophical difference between Node and Deno has narrowed over time as both evolved and borrowed good ideas from the wider ecosystem.

They are still distinct runtimes.

A `.js` file does not automatically imply identical host behaviour in both.

---

# The package ecosystem

JavaScript's package ecosystem is gigantic.

The most famous registry is **npm**.

A project often has a `package.json` describing metadata, scripts and dependencies:

```json
{
  "name": "museum-example",
  "type": "module",
  "dependencies": {}
}
```

Node commonly uses npm, pnpm or Yarn.

Deno can consume npm packages while also supporting JSR and URL/module-based workflows.

Browser applications frequently use bundlers and build tools.

The modern JavaScript ecosystem has many tools because the language managed to become everybody's problem at once.

---

# Shitpost cabinet

## `[] + []`

In JavaScript:

```javascript
[] + []
```

produces:

```text
""
```

Arrays convert to strings during the `+` operation.

Naturally this led to an entire genre of JavaScript coercion archaeology.

## `NaN !== NaN`

```javascript
NaN === NaN
```

is:

```text
false
```

This behaviour comes from IEEE 754 floating-point semantics rather than JavaScript inventing a bespoke chaos goblin.

Use:

```javascript
Number.isNaN(value)
```

when you actually want to test for `NaN`.

## `"11" + 1`

```javascript
"11" + 1
// "111"
```

while:

```javascript
"11" - 1
// 10
```

JavaScript is not confused.

It knows exactly what it did.

That somehow makes it worse.

---

# Learn more

Language Museum is not a full JavaScript course.

### The language

- [ECMAScript 2026 / ECMA-262](https://ecma-international.org/publications-and-standards/standards/ecma-262/)
- [Latest ECMAScript draft](https://tc39.es/ecma262/)
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [MDN JavaScript Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [TC39](https://tc39.es/)

### Browsers

- [MDN Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)
- [MDN `window.prompt()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt)

### Node.js

- [Node.js](https://nodejs.org/)
- [Node.js documentation](https://nodejs.org/docs/latest/api/)
- [Node.js `readline`](https://nodejs.org/api/readline.html)

### Deno

- [Deno](https://deno.com/)
- [Deno documentation](https://docs.deno.com/)
- [Deno input prompts](https://docs.deno.com/examples/prompts/)
- [Deno Web Platform APIs](https://docs.deno.com/runtime/reference/web_platform_apis/)

For the source program:

- [Pseudocode: the canonical Language Museum specimen](../other/pseudocode.md)

---

# Museum summary

JavaScript's syntax is only half of this exhibit.

The other half is the environment around it.

The same language can run:

- in a browser with `window`, the DOM and modal `prompt()`;
- in Node with `process`, filesystem APIs and `node:readline`;
- in Deno with Web APIs, `Deno.*`, permissions and a built-in prompt helper.

Meanwhile the factorial test exposes JavaScript's split numeric system:

- `Number` is floating-point and cannot exactly represent every large integer;
- `BigInt` handles arbitrary-size integer arithmetic.

If Python is the museum specimen that looks almost suspiciously like pseudocode, JavaScript is the exhibit where the walls open and reveal that half the room was supplied by the building.
