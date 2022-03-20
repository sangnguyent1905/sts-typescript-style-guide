## Variables

Always use const or let to declare variables. Use const by default, unless a variable needs to be reassigned. Never use var.

```ts
const foo = otherValue;  // GOOD
let bar = someValue;     // GOOD


var foo = someValue;     // BAD
```