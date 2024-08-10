# Object methods, "this"

- Objects are usually created to represent entities of the real world, like users, orders and so on:

```js
let user = {
  name: "John",
  age: 30
};
```

- Actions are represented in JavaScript by functions in properties.

## Method examples

- For a start, let's teach the `user` to say hello:

```js run
let user = {
  name: "John",
  age: 30
};

*!*
user.sayHi = function() {
  alert("Hello!");
};
*/!*

user.sayHi(); // Hello!
```

Of course, we could use a pre-declared function as a method, like this:

```js run
let user = {
  // ...
};

*!*
// first, declare
function sayHi() {
  alert("Hello!");
}

// then add as a method
user.sayHi = sayHi;
*/!*

user.sayHi(); // Hello!
```

### Method shorthand

- There exists a shorter syntax for methods in an object literal:

```js
// these objects do the same

user = {
  sayHi: function() {
    alert("Hello");
  }
};

// method shorthand looks better, right?
user = {
*!*
  sayHi() { // same as "sayHi: function(){...}"
*/!*
    alert("Hello");
  }
};
```

## "this" in methods

- It's common that an object method needs to access the information stored in the object to do its job. For instance, the code inside `user.sayHi()` may need the name of the `user`.

**To access the object, a method can use the `this` keyword.**

For instance:

```js run
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    // "this" is the "current object"
    alert(this.name);
*/!*
  }

};

user.sayHi(); // John
```

- Technically, it's also possible to access the object without `this`, by referencing it via the outer variable:

```js
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    alert(user.name); // "user" instead of "this"
*/!*
  }

};
```

That's demonstrated below:

```js run
let user = {
  name: "John",
  age: 30,

  sayHi() {
*!*
    alert( user.name ); // leads to an error
*/!*
  }

};


let admin = user;
user = null; // overwrite to make things obvious

*!*
admin.sayHi(); // TypeError: Cannot read property 'name' of null
*/!*
```

## "this" is not bound

- In JavaScript, keyword `this` behaves unlike most other programming languages. It can be used in any function, even if it's not a method of an object.

There's no syntax error in the following example:

```js
function sayHi() {
  alert( *!*this*/!*.name );
}
```


```js run
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

*!*
// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;
*/!*

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

The rule is simple: if `obj.f()` is called, then `this` is `obj` during the call of `f`. So it's either `user` or `admin` in the example above.


```js run
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

## Arrow functions have no "this"

- Arrow functions are special: they don't have their "own" `this`. If we reference `this` from such a function, it's taken from the outer "normal" function.

For instance, here `arrow()` uses `this` from the outer `user.sayHi()` method:

```js run
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```
