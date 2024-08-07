# Comments

- Comments can be single-line: starting with `//` and multiline: `/* ... */`. We normally use them to describe how and why the code works. At first sight, commenting might be obvious, but novices in programming often use them wrongly.

## Bad comments

- But in good code, the amount of such "explanatory" comments should be minimal. Seriously, the code should be easy to understand without them.

-There's a great rule about that: "if the code is so unclear that it requires a comment, then maybe it should be rewritten instead".

### Recipe: factor out functions

- Sometimes it's beneficial to replace a code piece with a function, like here:

```js
function showPrimes(n) {
  nextPrime:
  for (let i = 2; i < n; i++) {

*!*
    // check if i is a prime number
    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }
*/!*

    alert(i);
  }
}
```

- The better variant, with a factored out function `isPrime`:


```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    *!*if (!isPrime(i)) continue;*/!*

    alert(i);  
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if (n % i == 0) return false;
  }

  return true;
}
```

- Now we can understand the code easily. The function itself becomes the comment. Such code is called *self-descriptive*.

### Recipe: create functions

- And if we have a long "code sheet" like this:

```js
// here we add whiskey
for(let i = 0; i < 10; i++) {
  let drop = getWhiskey();
  smell(drop);
  add(drop, glass);
}

// here we add juice
for(let t = 0; t < 3; t++) {
  let tomato = getTomato();
  examine(tomato);
  let juice = press(tomato);
  add(juice, glass);
}

// ...
```

- Then it might be a better variant to refactor it into functions like:

```js
addWhiskey(glass);
addJuice(glass);

function addWhiskey(container) {
  for(let i = 0; i < 10; i++) {
    let drop = getWhiskey();
    //...
  }
}

function addJuice(container) {
  for(let t = 0; t < 3; t++) {
    let tomato = getTomato();
    //...
  }
}
```

- Once again, functions themselves tell what's going on. There's nothing to comment. And also the code structure is better when split. It's clear what every function does, what it takes and what it returns.

## Good comments

- Describe the architecture
: Provide a high-level overview of components, how they interact, what's the control flow in various situations... In short -- the bird's eye view of the code. There's a special language [UML](http://wikipedia.org/wiki/Unified_Modeling_Language) to build high-level architecture diagrams explaining the code. Definitely worth studying.

- Document function parameters and usage
: There's a special syntax [JSDoc](http://en.wikipedia.org/wiki/JSDoc) to document a function: usage, parameters, returned value.

For instance:
```js
/**
 * Returns x raised to the n-th power.
 *
 * @param {number} x The number to raise.
 * @param {number} n The power, must be a natural number.
 * @return {number} x raised to the n-th power.
 */
function pow(x, n) {
  ...
}
```

- Why is the task solved this way?
: What's written is important. But what's *not* written may be even more important to understand what's going on. Why is the task solved exactly this way? The code gives no answer.

