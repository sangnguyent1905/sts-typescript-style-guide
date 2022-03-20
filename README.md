# identifiers

## UpperCamelCase

`class` / `interface` / `type` / `enum` / `decorator` / `type parameters`

```ts
class ClassName {}

interface InterfaceName {}

type TypeName {}

enum  EnumName {}

@DecoratorName()

Array<T>

```

## lowerCamelCase

`variable` / `parameter` / `function` / `method` / `property` / `module alias`

```ts
var variableName

(parameter: TypeName) => {}

function functionName() {}

class ClassName {
    propertyName: number;
    methodName() {};
}

import { ... } from '@modules-alias/...';

```

## CONSTANT_CASE

`global constant values`, `including enum values`

```ts
const GLOBAL_CONSTANT = 'value';

enum LogLevel {
    ERROR,
    WARN,
    INFO,
    DEBUG
}
```

## Abbreviations

use `loadHttpUrl`, not `loadHTTPURL`

## Dollar sign

Identifiers should not generally use `$` unless it is a `Observable` variable.

# Naming style

-   Names should be two things: `clear` (know what it refers to) and `precise` (know what it does not refer to);

-   Do not use `_` for `private` properties or methods.

-   Do not use prefix `I` for interfaces

-   Do not mark interfaces specially (IMyInterface or MyFooInterface).

-   Suffixing Observables with `$`

# Aliases

When creating a local-scope alias of an existing symbol, use the format of the existing identifier. The local alias must match the existing naming and format of the source. For variables use `const` for your local aliases, and for class fields use the `readonly` attribute.

```ts
const CAPACITY = 5;

class Teapot {
    readonly BrewStateEnum = BrewStateEnum;
    readonly CAPACITY = CAPACITY;
}
```

# JSDoc vs comments

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

# Language Rules

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

## Constructors

-   Constructor calls must use parentheses, even when no arguments are passed:

```ts
const x = new Foo(); // BAD
const x = new Foo(); // GOOD
```

-   It is unnecessary to provide an empty constructor

```ts
class UnnecessaryConstructor {
    constructor() {}
}
```

## Parameter properties

-   Rather than plumbing an obvious initializer through to a class member, use a TypeScript parameter property.

```ts
class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {}
}

let dad = new Octopus('Man with the 8 strong legs');
dad.name;
```

## Field initializers

If a class member is not a parameter, initialize it where it's declared, which sometimes lets you drop the constructor entirely.

```ts
class Foo {
    private readonly userList: string[];
    constructor() {
        this.userList = []; // BAD
    }
}

class Foo {
    private readonly userList: string[] = []; // GOOD
}
```

## Getters and Setters (Accessors)

Do not define pass-through accessors only for the purpose of hiding a property. Instead, make the property `public`

```ts
class Bar {
    private barInternal = '';
    // Neither of these accessors have logic, so just make bar public.
    get bar() {
        return this.barInternal;
    }

    set bar(value: string) {
        this.barInternal = value;
    }
}
```

## Array constructor

TypeScript code must not use the `Array()` constructor, with or without `new`. It has confusing and contradictory usage:

```ts
const a = new Array(2); // BAD

const c = []; // GOOD
c.length = 2;
```

## Variables

Always use const or let to declare variables. Use const by default, unless a variable needs to be reassigned. Never use var.

```ts
const foo = otherValue; // GOOD
let bar = someValue; // GOOD

var foo = someValue; // BAD
```

## Exceptions

Always use `new Error()` when instantiating exceptions, instead of just calling `Error()`.

```ts
throw new Error('Foo is not a valid bar.'); // GOOD

throw Error('Foo is not a valid bar.'); // BAD
```
