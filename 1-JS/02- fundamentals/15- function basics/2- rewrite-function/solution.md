Using a question mark operator `'?'`:

```js
function checkAge(age) {
  return (age > 18) ? true : confirm('Did parents allow you?');
}
```

Using OR `||` (the shortest variant):

```js
function checkAge(age) {
  return (age > 18) || confirm('Did parents allow you?');
}
```
