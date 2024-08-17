
```js run demo
function readNumber() {
  let num;

  do {
    num = prompt("Enter a number please?", 0);
  } while ( !isFinite(num) );

  if (num === null || num === '') return null;
  
  return +num;
}

alert(`Read: ${readNumber()}`);
```

- So we actually accept the input until it is a "regular number". Both `null` (cancel) and empty line also fit that condition, because in numeric form they are `0`.

- After we stopped, we need to treat `null` and empty line specially (return `null`), because converting them to a number would return `0`.