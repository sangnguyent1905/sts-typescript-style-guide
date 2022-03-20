## Exceptions

Always use `new Error()` when instantiating exceptions, instead of just calling `Error()`.

```ts
throw new Error('Foo is not a valid bar.'); // GOOD

throw Error('Foo is not a valid bar.'); // BAD
```
