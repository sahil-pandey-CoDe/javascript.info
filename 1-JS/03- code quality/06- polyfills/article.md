
# Polyfills and transpilers

- As programmers, we'd like to use most recent features. The more good stuff - the better! On the other hand, how to make our modern code work on older engines that don't understand recent features yet?

There are two tools for that:

1. Transpilers.
2. Polyfills.

## Transpilers

A [transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler) is a special piece of software that translates source code to another source code. It can parse ("read and understand") modern code and rewrite it using older syntax constructs, so that it'll also work in outdated engines.

E.g. JavaScript before year 2020 didn't have the "nullish coalescing operator" `??`. So, if a visitor uses an outdated browser, it may fail to understand the code like `height = height ?? 100`.

A transpiler would analyze our code and rewrite `height ?? 100` into `(height !== undefined && height !== null) ? height : 100`.

```js
// before running the transpiler
height = height ?? 100;

// after running the transpiler
height = (height !== undefined && height !== null) ? height : 100;
```


- Usually, a developer runs the transpiler on their own computer, and then deploys the transpiled code to the server. 
- Speaking of names, [Babel](https://babeljs.io) is one of the most prominent transpilers out there.

## Polyfills

- New language features may include not only syntax constructs and operators, but also built-in functions. A script that updates/adds new functions is called "polyfill". It "fills in" the gap and adds missing implementations.

For example, `Math.trunc(n)` is a function that "cuts off" the decimal part of a number, e.g `Math.trunc(1.23)` returns `1`.

- In some (very outdated) JavaScript engines, there's no `Math.trunc`, so such code will fail. As we're talking about new functions, not syntax changes, there's no need to transpile anything here. We just need to declare the missing function.

For this particular case, the polyfill for `Math.trunc` is a script that implements it, like this:

```js
if (!Math.trunc) { // if no such function
  // implement it
  Math.trunc = function(number) {
    // Math.ceil and Math.floor exist even in ancient JavaScript engines
    // they are covered later in the tutorial
    return number < 0 ? Math.ceil(number) : Math.floor(number);
  };
}
```

JavaScript is a highly dynamic language. Scripts may add/modify any function, even built-in ones.

An interesting polyfill libraries is:
- [core js](https://github.com/zloirock/core-js) that supports a lot, allows to include only needed features.


