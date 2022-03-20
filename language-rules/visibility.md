## Visibility

Restricting visibility of properties, methods, and entire types helps with keeping code decoupled.

-   Limit symbol visibility as much as possible.
-   Consider converting private methods to non-exported functions within the same file but outside of any class, and moving private properties into a separate, non-exported class.
-   TypeScript symbols are public by default. Never use the `public` modifier except when declaring non-readonly public parameter properties (in constructors).

```ts
class Foo {
    bar = new Bar(); // GOOD: public modifier not needed

    constructor(public baz: Baz) {} // public modifier allowed
}
```
