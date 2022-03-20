
## Array constructor

TypeScript code must not use the `Array()` constructor, with or without `new`. It has confusing and contradictory usage:

```ts
const a = new Array(2); // BAD

const c = []; // GOOD
c.length = 2;
```