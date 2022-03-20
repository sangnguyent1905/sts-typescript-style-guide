## Constructors

- Constructor calls must use parentheses, even when no arguments are passed:

```ts
const x = new Foo; // BAD
const x = new Foo(); // GOOD
```

- It is unnecessary to provide an empty constructor

```ts
class UnnecessaryConstructor {
  constructor() {}
}
```