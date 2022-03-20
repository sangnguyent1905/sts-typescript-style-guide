## JSDoc vs comments

There are two types of comments, JSDoc (`/** ... */`) and non-JSDoc ordinary comments (`// ...` or `/* ... */`).

-   Use `/** JSDoc */` comments for documentation, i.e. comments a user of the code should read.

-   Use `//` line comments for implementation comments, i.e. comments that only concern the implementation of the code itself.

```ts
/** This class demonstrates how parameter properties are documented. */
class ParamProps {
    /**
     * @param percolator The percolator used for brewing.
     * @param beans The beans to brew.
     */
    constructor(private readonly percolator: Percolator, private readonly beans: CoffeeBean[]) {}

    /**
     * Brews coffee.
     * @param amountLitres The amount to brew. Must fit the pot size!
     */
    brew(amountLitres: number) {
        // This implementation creates terrible coffee, but whatever.
        // TODO(b/12345): Improve percolator brewing.
    }
}

// Inline block comments for parameters that'd be hard to understand:
new Percolator().brew(/* amountLitres= */ 5);
```
