# Ninja code

- Programmer ninjas of the past used these tricks to sharpen the mind of code maintainers.
- Code review gurus look for them in test tasks.
- Novice developers sometimes use them even better than programmer ninjas.

Read them carefully and find out who you are -- a ninja, a novice, or maybe a code reviewer?

## Brevity is the soul of wit

- Make the code as short as possible. Show how smart you are. Let subtle language features guide you. For instance, take a look at this ternary operator `'?'`:

```js
// taken from a well-known javascript library
i = i ? i < 0 ? Math.max(0, len + i) : i : 0;
```

- Cool, right? If you write like that, a developer who comes across this line and tries to understand what is the value of `i` is going to have a merry time. Then come to you, seeking for an answer.

## One-letter variables

- Another way to code shorter is to use single-letter variable names everywhere. Like `a`, `b` or `c`.

- A short variable disappears in the code like a real ninja in the forest. No one will be able to find it using "search" of the editor. And even if someone does, they won't be able to "decipher" what the name `a` or `b` means.


## Use abbreviations

- If the team rules forbid the use of one-letter and vague names -- shorten them, make abbreviations.

Like this:

- `list` -> `lst`.
- `userAgent` -> `ua`.
- `browser` -> `brsr`.
- ...etc

## Soar high. Be abstract.

- While choosing a name try to use the most abstract word. Like `obj`, `data`, `value`, `item`, `elem` and so on.

- **The ideal name for a variable is `data`.** Use it everywhere you can. Indeed, every variable holds *data*, right?

    ...But what to do if `data` is already taken? Try `value`, it's also universal. After all, a variable eventually gets a *value*.

- **Name a variable by its type: `str`, `num`...**

    Give them a try. A young initiate may wonder -- are such names really useful for a ninja? Indeed, they are!


- **...But what if there are no more such names?** Just add a number: `data1, item2, elem5`...

## Attention test

Only a truly attentive programmer should be able to understand your code. But how to check that?

**One of the ways -- use similar variable names, like `date` and `data`.**

Mix them where you can.

A quick read of such code becomes impossible. And when there's a typo... Ummm... We're stuck for long, time to drink tea.


## Smart synonyms

- Using *similar* names for *same* things makes life more interesting and shows your creativity to the public. For instance, consider function prefixes. If a function shows a message on the screen -- start it with `display…`, like `displayMessage`. And then if another function shows on the screen something else, like a user name, start it with `show…` (like `showName`).

## Reuse names

- Add a new variable only when absolutely necessary. Instead, reuse existing names. Just write new values into them.

**An advanced variant of the approach is to covertly (!) replace the value with something alike in the middle of a loop or a function.**

For instance:

```js
function ninjaFunction(elem) {
  // 20 lines of code working with elem

  elem = clone(elem);

  // 20 more lines, now working with the clone of the elem!
}
```

## Underscores for fun

- Put underscores `_` and `__` before variable names. Like `_name` or `__value`. It would be great if only you knew their meaning. Or, better, add them just for fun, without particular meaning at all. Or different meanings in different places.

- You kill two rabbits with one shot. First, the code becomes longer and less readable, and the second, a fellow developer may spend a long time trying to figure out what the underscores mean.


## Show your love

- Let everyone see how magnificent your entities are! Names like `superElement`, `megaFrame` and `niceItem` will definitely enlighten a reader.

- Indeed, from one hand, something is written: `super..`, `mega..`, `nice..` But from the other hand -- that brings no details. A reader may decide to look for a hidden meaning and meditate for an hour or two of their paid working time.


## Overlap outer variables

- Use same names for variables inside and outside a function. As simple. No efforts to invent new names.

```js
let *!*user*/!* = authenticateUser();

function render() {
  let *!*user*/!* = anotherValue();
  ...
  ...many lines...
  ...
  ... // <-- a programmer wants to work with user here and...
  ...
}
```

- A programmer who jumps inside the `render` will probably fail to notice that there's a local `user` shadowing the outer one.

## Side-effects everywhere!

- There are functions that look like they don't change anything. Like `isReady()`, `checkPermission()`, `findTags()`... They are assumed to carry out calculations, find and return the data, without changing anything outside of them. In other words, without "side-effects".

**A really beautiful trick is to add a "useful" action to them, besides the main task.**

An expression of dazed surprise on the face of your colleague when they see a function named `is..`, `check..` or `find...` changing something -- will definitely broaden your boundaries of reason.

**Another way to surprise is to return a non-standard result.**

Show your original thinking! Let the call of `checkPermission` return not `true/false`, but a complex object with the results of the check.

## Powerful functions!

- Don't limit the function by what's written in its name. Be broader. For instance, a function `validateEmail(email)` could (besides checking the email for correctness) show an error message and ask to re-enter the email.

**Joining several actions into one protects your code from reuse.**

Imagine, another developer wants only to check the email, and not output any message. Your function  `validateEmail(email)` that does both will not suit them. So they won't break your meditation by asking anything about it.
