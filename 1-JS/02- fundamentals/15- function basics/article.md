# Functions

- Quite often we need to perform a similar action in many places of the script. For example, we need to show a nice-looking message when a visitor logs in, logs out and maybe somewhere else.

- Functions are the main "building blocks" of the program. They allow the code to be called many times without repetition. We've already seen examples of built-in functions, like `alert(message)`, `prompt(message, default)` and `confirm(question)`. But we can create functions of our own as well.

## Function Declaration

- To create a function we can use a *function declaration*. It looks like this:

```js
function showMessage() {
  alert( 'Hello everyone!' );
}
```

- The `function` keyword goes first, then goes the *name of the function*, then a list of *parameters* between the parentheses (comma-separated, empty in the example above, we'll see examples later) and finally the code of the function, also named "the function body", between curly braces.

```js
function name(parameter1, parameter2, ... parameterN) {
 // body
}
```

- Our new function can be called by its name: `showMessage()`. For instance:

```js run
function showMessage() {
  alert( 'Hello everyone!' );
}

*!*
showMessage();
showMessage();
*/!*
```

- This example clearly demonstrates one of the main purposes of functions: to avoid code duplication. If we ever need to change the message or the way it is shown, it's enough to modify the code in one place: the function which outputs it.

## Local variables

- A variable declared inside a function is only visible inside that function. For example:

```js run
function showMessage() {
*!*
  let message = "Hello, I'm JavaScript!"; // local variable
*/!*

  alert( message );
}

showMessage(); // Hello, I'm JavaScript!

alert( message ); // <-- Error! The variable is local to the function
```

## Outer variables

- A function can access an outer variable as well, for example:

```js run no-beautify
let *!*userName*/!* = 'John';

function showMessage() {
  let message = 'Hello, ' + *!*userName*/!*;
  alert(message);
}

showMessage(); // Hello, John
```

- The function has full access to the outer variable. It can modify it as well. For instance:

```js run
let *!*userName*/!* = 'John';

function showMessage() {
  *!*userName*/!* = "Bob"; // (1) changed the outer variable

  let message = 'Hello, ' + *!*userName*/!*;
  alert(message);
}

alert( userName ); // *!*John*/!* before the function call

showMessage();

alert( userName ); // *!*Bob*/!*, the value was modified by the function
```

- If a same-named variable is declared inside the function then it *shadows* the outer one. For instance, in the code below the function uses the local `userName`. The outer one is ignored:

```js run
let userName = 'John';

function showMessage() {
*!*
  let userName = "Bob"; // declare a local variable
*/!*

  let message = 'Hello, ' + userName; // *!*Bob*/!*
  alert(message);
}

// the function will create and use its own userName
showMessage();

alert( userName ); // *!*John*/!*, unchanged, the function did not access the outer variable
```

```
It's a good practice to minimize the use of global variables. Modern code has few or no globals. Most variables reside in their functions. Sometimes though, they can be useful to store project-level data.
```

## Parameters

- We can pass arbitrary data to functions using parameters. In the example below, the function has two parameters: `from` and `text`.

```js run
function showMessage(*!*from, text*/!*) { // parameters: from, text
  alert(from + ': ' + text);
}

*!*showMessage('Ann', 'Hello!');*/!* // Ann: Hello! (*)
*!*showMessage('Ann', "What's up?");*/!* // Ann: What's up? (**)
```

- When the function is called in lines `(*)` and `(**)`, the given values are copied to local variables `from` and `text`. Then the function uses them.

- Here's one more example: we have a variable `from` and pass it to the function. Please note: the function changes `from`, but the change is not seen outside, because a function always gets a copy of the value:

```js run
function showMessage(from, text) {

*!*
  from = '*' + from + '*'; // make "from" look nicer
*/!*

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// the value of "from" is the same, the function modified a local copy
alert( from ); // Ann
```

In other words, to put these terms straight:

- A parameter is the variable listed inside the parentheses in the function declaration (it's a declaration time term).
- An argument is the value that is passed to the function when it is called (it's a call time term).


## Default values

- If a function is called, but an argument is not provided, then the corresponding value becomes `undefined`. We can specify the so-called "default" (to use if omitted) value for a parameter in the function declaration, using `=`:

```js run
function showMessage(from, *!*text = "no text given"*/!*) {
  alert( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given
```

- Now if the `text` parameter is not passed, it will get the value `"no text given"`. The default value also jumps in if the parameter exists, but strictly equals `undefined`, like this:

```js
showMessage("Ann", undefined); // Ann: no text given
```

- Here `"no text given"` is a string, but it can be a more complex expression, which is only evaluated and assigned if the parameter is missing. So, this is also possible:

```js run
function showMessage(from, text = anotherFunction()) {
  // anotherFunction() only executed if no text given
  // its result becomes the value of text
}
```

```
In the example above, `anotherFunction()` isn't called at all, if the `text` parameter is provided. On the other hand, it's independently called every time when `text` is missing.
```


### Alternative default parameters

- Sometimes it makes sense to assign default values for parameters at a later stage after the function declaration. We can check if the parameter is passed during the function execution, by comparing it with `undefined`:

```js run
function showMessage(text) {
  // ...

*!*
  if (text === undefined) { // if the parameter is missing
    text = 'empty message';
  }
*/!*

  alert(text);
}

showMessage(); // empty message
```

...Or we could use the `||` operator:

```js
function showMessage(text) {
  // if text is undefined or otherwise falsy, set it to 'empty'
  text = text || 'empty';
  ...
}
```

- Modern JavaScript engines support the [nullish coalescing operator](info:nullish-coalescing-operator) `??`, it's better when most falsy values, such as `0`, should be considered "normal":

```js run
function showCount(count) {
  // if count is undefined or null, show "unknown"
  alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
```

## Returning a value

- A function can return a value back into the calling code as the result. The simplest example would be a function that sums two values:

```js run no-beautify
function sum(a, b) {
  *!*return*/!* a + b;
}

let result = sum(1, 2);
alert( result ); // 3
```

- The directive `return` can be in any place of the function. When the execution reaches it, the function stops, and the value is returned to the calling code (assigned to `result` above). There may be many occurrences of `return` in a single function. For instance:

```js run
function checkAge(age) {
  if (age >= 18) {
*!*
    return true;
*/!*
  } else {
*!*
    return confirm('Do you have permission from your parents?');
*/!*
  }
}

let age = prompt('How old are you?', 18);

if ( checkAge(age) ) {
  alert( 'Access granted' );
} else {
  alert( 'Access denied' );
}
```

- It is possible to use `return` without a value. That causes the function to exit immediately. For example:

```js
function showMovie(age) {
  if ( !checkAge(age) ) {
*!*
    return;
*/!*
  }

  alert( "Showing you the movie" ); // (*)
  // ...
}
```


## Naming a function [#function-naming]

- Functions are actions. So their name is usually a verb. It should be brief, as accurate as possible and describe what the function does, so that someone reading the code gets an indication of what the function does. For instance, functions that start with `"show"` usually show something.

Examples of such names:

```js no-beautify
showMessage(..)     // shows a message
getAge(..)          // returns the age (gets it somehow)
calcSum(..)         // calculates a sum and returns the result
createForm(..)      // creates a form (and usually returns it)
checkPermission(..) // checks a permission, returns true/false
```


## Functions == Comments

- Functions should be short and do exactly one thing. If that thing is big, maybe it's worth it to split the function into a few smaller functions. Sometimes following this rule may not be that easy, but it's definitely a good thing. For instance, compare the two functions `showPrimes(n)` below. Each one outputs [prime numbers](https://en.wikipedia.org/wiki/Prime_number) up to `n`.

The first variant uses a label:

```js
function showPrimes(n) {
  nextPrime: for (let i = 2; i < n; i++) {

    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }

    alert( i ); // a prime
  }
}
```

The second variant uses an additional function `isPrime(n)` to test for primality:

```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    *!*if (!isPrime(i)) continue;*/!*

    alert(i);  // a prime
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if ( n % i == 0) return false;
  }
  return true;
}
```

- The second variant is easier to understand, isn't it? Instead of the code piece we see a name of the action (`isPrime`). Sometimes people refer to such code as *self-describing*. So, functions can be created even if we don't intend to reuse them. They structure the code and make it readable.

