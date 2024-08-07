# Automated testing with Mocha

- Automated testing will be used in further tasks, and it's also widely used in real projects.

## Why do we need tests?

- When we write a function, we can usually imagine what it should do: which parameters give which results. During development, we can check the function by running it and comparing the outcome with the expected one. For instance, we can do it in the console. If something is wrong -- then we fix the code, run again, check the result -- and so on till it works.

**When testing a code by manual re-runs, it's easy to miss something.**

- That's very typical. When we develop something, we keep a lot of possible use cases in mind. But it's hard to expect a programmer to check all of them manually after every change. So it becomes easy to fix one thing and break another one.

**Automated testing means that tests are written separately, in addition to the code. They run our functions in various ways and compare results with the expected.**

## Behavior Driven Development (BDD)

- Let's start with a technique named [Behavior Driven Development] or, in short, BDD.

**BDD is three things in one: tests AND documentation AND examples.**


## Development of "pow": the spec

- Before creating the code of `pow`, we can imagine what the function should do and describe it. Such description is called a *specification* or, in short, a spec, and contains descriptions of use cases together with tests for them, like this:

```js
describe("pow", function() {

  it("raises to n-th power", function() {
    assert.equal(pow(2, 3), 8);
  });

});
```

A spec has three main building blocks that you can see above:

`describe("title", function() { ... })`
: What functionality we're describing? In our case we're describing the function `pow`. Used to group "workers" -- the `it` blocks.

`it("use case description", function() { ... })`
: In the title of `it` we *in a human-readable way* describe the particular use case, and the second argument is a function that tests it.

`assert.equal(value1, value2)`
: The code inside `it` block, if the implementation is correct, should execute without errors.


## The spec in action

The full HTML page with these frameworks and `pow` spec:

```html src="index.html"
```

The page can be divided into five parts:

1. The `<head>` -- add third-party libraries and styles for tests.
2. The `<script>` with the function to test, in our case -- with the code for `pow`.
3. The tests -- in our case an external script `test.js` that has `describe("pow", ...)` from above.
4. The HTML element `<div id="mocha">` will be used by Mocha to output results.
5. The tests are started by the command `mocha.run()`.


## Initial implementation

- Let's make a simple implementation of `pow`, for tests to pass:

```js
function pow(x, n) {
  return 8; // :) we cheat!
}
```

Wow, now it works!

[iframe height=250 src="pow-min" border=1 edit]

## Improving the spec

- The principal difference is that when `assert` triggers an error, the `it` block immediately terminates. So, in the first variant if the first `assert` fails, then we'll never see the result of the second `assert`.

- Making tests separate is useful to get more information about what's going on, so the second variant is better.


1. The first variant -- add one more `assert` into the same `it`:

    ```js
    describe("pow", function() {

      it("raises to n-th power", function() {
        assert.equal(pow(2, 3), 8);
    *!*
        assert.equal(pow(3, 4), 81);
    */!*
      });

    });
    ```
2. The second -- make two tests:

    ```js
    describe("pow", function() {

      it("2 raised to power 3 is 8", function() {
        assert.equal(pow(2, 3), 8);
      });

      it("3 raised to power 4 is 81", function() {
        assert.equal(pow(3, 4), 81);
      });

    });
    ```

## Improving the implementation

- Let's write something more real for tests to pass:

```js
function pow(x, n) {
  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}
```

- To be sure that the function works well, let's test it for more values. Instead of writing `it` blocks manually, we can generate them in `for`:

```js
describe("pow", function() {

  function makeTest(x) {
    let expected = x * x * x;
    it(`${x} in the power 3 is ${expected}`, function() {
      assert.equal(pow(x, 3), expected);
    });
  }

  for (let x = 1; x <= 5; x++) {
    makeTest(x);
  }

});
```

The result:

[iframe height=250 src="pow-3" edit border="1"]

## Nested describe

- We're going to add even more tests. But before that let's note that the helper function `makeTest` and `for` should be grouped together. We won't need `makeTest` in other tests, it's needed only in `for`: their common task is to check how `pow` raises into the given power.

Grouping is done with a nested `describe`:

```js
describe("pow", function() {

*!*
  describe("raises x to power 3", function() {
*/!*

    function makeTest(x) {
      let expected = x * x * x;
      it(`${x} in the power 3 is ${expected}`, function() {
        assert.equal(pow(x, 3), expected);
      });
    }

    for (let x = 1; x <= 5; x++) {
      makeTest(x);
    }

*!*
  });
*/!*

  // ... more tests to follow here, both describe and it can be added
});
```

The nested `describe` defines a new "subgroup" of tests. In the output we can see the titled indentation:

[iframe height=250 src="pow-4" edit border="1"]

In the future we can add more `it` and `describe` on the top level with helper functions of their own, they won't see `makeTest`.

````smart header="`before/after` and `beforeEach/afterEach`"
We can setup `before/after` functions that execute before/after running tests, and also `beforeEach/afterEach` functions that execute before/after *every* `it`.

For instance:

```js no-beautify
describe("test", function() {

  before(() => alert("Testing started – before all tests"));
  after(() => alert("Testing finished – after all tests"));

  beforeEach(() => alert("Before a test – enter a test"));
  afterEach(() => alert("After a test – exit a test"));

  it('test 1', () => alert(1));
  it('test 2', () => alert(2));

});
```

The running sequence will be:

```
Testing started – before all tests (before)
Before a test – enter a test (beforeEach)
1
After a test – exit a test   (afterEach)
Before a test – enter a test (beforeEach)
2
After a test – exit a test   (afterEach)
Testing finished – after all tests (after)
```

[edit src="beforeafter" title="Open the example in the sandbox."]

Usually, `beforeEach/afterEach` and `before/after` are used to perform initialization, zero out counters or do something else between the tests (or test groups).
````

## Extending the spec

The basic functionality of `pow` is complete. The first iteration of the development is done. When we're done celebrating and drinking champagne -- let's go on and improve it.

As it was said, the function `pow(x, n)` is meant to work with positive integer values `n`.

To indicate a mathematical error, JavaScript functions usually return `NaN`. Let's do the same for invalid values of `n`.

Let's first add the behavior to the spec(!):

```js
describe("pow", function() {

  // ...

  it("for negative n the result is NaN", function() {
*!*
    assert.isNaN(pow(2, -1));
*/!*
  });

  it("for non-integer n the result is NaN", function() {
*!*
    assert.isNaN(pow(2, 1.5));    
*/!*
  });

});
```

The result with new tests:

[iframe height=530 src="pow-nan" edit border="1"]

The newly added tests fail, because our implementation does not support them. That's how BDD is done: first we write failing tests, and then make an implementation for them.

```smart header="Other assertions"
Please note the assertion `assert.isNaN`: it checks for `NaN`.

There are other assertions in [Chai](https://www.chaijs.com/) as well, for instance:

- `assert.equal(value1, value2)` -- checks the equality  `value1 == value2`.
- `assert.strictEqual(value1, value2)` -- checks the strict equality `value1 === value2`.
- `assert.notEqual`, `assert.notStrictEqual` -- inverse checks to the ones above.
- `assert.isTrue(value)` -- checks that `value === true`
- `assert.isFalse(value)` -- checks that `value === false`
- ...the full list is in the [docs](https://www.chaijs.com/api/assert/)
```

So we should add a couple of lines to `pow`:

```js
function pow(x, n) {
*!*
  if (n < 0) return NaN;
  if (Math.round(n) != n) return NaN;
*/!*

  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}
```

Now it works, all tests pass:

[iframe height=300 src="pow-full" edit border="1"]

[edit src="pow-full" title="Open the full final example in the sandbox."]
