# Function expressions

- In JavaScript, a function is not a "magical language structure", but a special kind of value. The syntax that we used before is called a *Function Declaration*:

```js
function sayHi() {
  alert( "Hello" );
}
```

- There is another syntax for creating a function that is called a *Function Expression*. It allows us to create a new function in the middle of any expression.

For example:

```js
let sayHi = function() {
  alert( "Hello" );
};
```

- Here we can see a variable `sayHi` getting a value, the new function, created as `function() { alert("Hello"); }`. As the function creation happens in the context of the assignment expression (to the right side of `=`), this is a *Function Expression*.


## Function is a value

- Let's reiterate: no matter how the function is created, a function is a value. Both examples above store a function in the `sayHi` variable. We can even print out that value using `alert`:

```js run
function sayHi() {
  alert( "Hello" );
}

*!*
alert( sayHi ); // shows the function code
*/!*
```

- In JavaScript, a function is a value, so we can deal with it as a value. The code above shows its string representation, which is the source code. Surely, a function is a special value, in the sense that we can call it like `sayHi()`. But it's still a value. So we can work with it like with other kinds of values.

We can copy a function to another variable:

```js run no-beautify
function sayHi() {   // (1) create
  alert( "Hello" );
}

let func = sayHi;    // (2) copy

func(); // Hello     // (3) run the copy (it works)!
sayHi(); // Hello    //     this still works too (why wouldn't it)
```

We could also have used a Function Expression to declare `sayHi`, in the first line:

```js
let sayHi = function() { // (1) create
  alert( "Hello" );
};

let func = sayHi;
// ...
```

Everything would work the same.


````
You might wonder, why do Function Expressions have a semicolon `;` at the end, but Function Declarations do not:

```js
function sayHi() {
  // ...
}

let sayHi = function() {
  // ...
}*!*;*/!*
```

The semicolon would be there for a simpler assignment, such as `let sayHi = 5;`, and it's also there for a function assignment.
````

## Callback functions

- We'll write a function `ask(question, yes, no)` with three parameters:

`question`
: Text of the question

`yes`
: Function to run if the answer is "Yes"

`no`
: Function to run if the answer is "No"

The function should ask the `question` and, depending on the user's answer, call `yes()` or `no()`:

```js run
*!*
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}
*/!*

function showOk() {
  alert( "You agreed." );
}

function showCancel() {
  alert( "You canceled the execution." );
}

// usage: functions showOk, showCancel are passed as arguments to ask
ask("Do you agree?", showOk, showCancel);
```

**The arguments `showOk` and `showCancel` of `ask` are called *callback functions* or just *callbacks*.**



```smart header="A function is a value representing an \"action\""
Regular values like strings or numbers represent the *data*.
A function can be perceived as an *action*.
We can pass it between variables and run when we want.
```


## Function Expression vs Function Declaration

- *Function Declaration:* a function, declared as a separate statement, in the main code flow:

    ```js
    // Function Declaration
    function sum(a, b) {
      return a + b;
    }
    ```
- *Function Expression:* a function, created inside an expression or inside another syntax construct. Here, the function is created on the right side of the "assignment expression" `=`:

    ```js
    // Function Expression
    let sum = function(a, b) {
      return a + b;
    };
    ```


**A Function Expression is created when the execution reaches it and is usable only from that moment.**

- Function Declarations are different.

**A Function Declaration can be called earlier than it is defined.**

- That's due to internal algorithms. When JavaScript prepares to run the script, it first looks for global Function Declarations in it and creates the functions. We can think of it as an "initialization stage".

For example, this works:

```js run refresh untrusted
*!*
sayHi("John"); // Hello, John
*/!*

function sayHi(name) {
  alert( `Hello, ${name}` );
}
```

...If it were a Function Expression, then it wouldn't work:

```js run refresh untrusted
*!*
sayHi("John"); // error!
*/!*

let sayHi = function(name) {  // (*) no magic any more
  alert( `Hello, ${name}` );
};
```

**In strict mode, when a Function Declaration is within a code block, it's visible everywhere inside that block. But not outside of it.**

- For instance, let's imagine that we need to declare a function `welcome()` depending on the `age` variable that we get during runtime. And then we plan to use it some time later.
If we use Function Declaration, it won't work as intended:

```js run
let age = prompt("What is your age?", 18);

// conditionally declare a function
if (age < 18) {

  function welcome() {
    alert("Hello!");
  }

} else {

  function welcome() {
    alert("Greetings!");
  }

}

// ...use it later
*!*
welcome(); // Error: welcome is not defined
*/!*
```

- That's because a Function Declaration is only visible inside the code block in which it resides.

Here's another example:

```js run
let age = 16; // take 16 as an example

if (age < 18) {
*!*
  welcome();               // \   (runs)
*/!*
                           //  |
  function welcome() {     //  |
    alert("Hello!");       //  |  Function Declaration is available
  }                        //  |  everywhere in the block where it's declared
                           //  |
*!*
  welcome();               // /   (runs)
*/!*

} else {

  function welcome() {
    alert("Greetings!");
  }
}

// Here we're out of curly braces,
// so we can not see Function Declarations made inside of them.

*!*
welcome(); // Error: welcome is not defined
*/!*
```

- What can we do to make `welcome` visible outside of `if`?
The correct approach would be to use a Function Expression and assign `welcome` to the variable that is declared outside of `if` and has the proper visibility.

This code works as intended:

```js run
let age = prompt("What is your age?", 18);

let welcome;

if (age < 18) {

  welcome = function() {
    alert("Hello!");
  };

} else {

  welcome = function() {
    alert("Greetings!");
  };

}

*!*
welcome(); // ok now
*/!*
```
