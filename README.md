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

## lower-case

`file-name`

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
const x = new Foo; // BAD
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

## Iterating objects

Iterating objects with `for (... in ...)` is error prone. It will include enumerable properties from the prototype chain.

Do not use unfiltered `for (... in ...)` statements:

```ts
//BAD
for (const x in someObj) {
    // x could come from some parent prototype!
}

//GOOD
for (const x in someObj) {
    if (!someObj.hasOwnProperty(x)) continue;
    // now x was definitely defined on someObj
}
for (const x of Object.keys(someObj)) {
    // note: for _of_!
    // now x was definitely defined on someObj
}
for (const [key, value] of Object.entries(someObj)) {
    // note: for _of_!
    // now key was definitely defined on someObj
}
```

## Control flow statements & blocks

Control flow statements spanning multiple lines always use blocks for the containing code.

```ts
if (x) {
    doSomethingWithALongMethodName(x); // GOOD
}

if (x) doSomethingWithALongMethodName(i); // BAD
```

## Switch Statements

All switch statements must contain a `default` statement group, even if it contains no code.

```ts
switch (x) {
    case Y:
        doSomethingElse();
        break;
    default:
    // nothing to do.
}
```

## Equality Checks

Always use triple equals `(===)` and not equals `(!==)`.

```ts
if (foo == 'bar' || baz != bam) {
    // Hard to understand behaviour due to type coercion.
}
if (foo === 'bar' || baz !== bam) {
    // All good here.
}
```

## Function Declarations

Use `function foo() { ... }` to declare named functions instead of assigning a function expression into a local variable.

```ts
// GOOD
function foo() { ... }

//BAD

// Given the above declaration, this won't compile:
foo = () => 3;  // ERROR: Invalid left-hand side of assignment expression.

// So declarations like this are unnecessary.
const foo = function() { ... }
```

## Use arrow functions in expressions

Always use arrow functions instead of pre-ES6 function expressions defined with the `function` keyword.

```ts
bar(() => { this.doSomething(); }) // GOOD

bar(function() { ... }) // BAD
```

## Arrow functions as properties

    Code should always use arrow functions to call instance methods (`const handler = (x) => { this.listener(x); };`), and should not obtain or pass references to instance methods  (`const handler = this.listener; handler(x);`).

```ts
// BAD
class DelayHandler {
    constructor() {
        // Problem: `this` is not preserved in the callback. `this` in the callback
        // will not be an instance of DelayHandler.
        setTimeout(this.patienceTracker, 5000);
    }
    private patienceTracker() {
        this.waitedPatiently = true;
    }
}

// GOOD
class DelayHandler {
    constructor() {
        setTimeout(() => {
            this.patienceTracker();
        }, 5000);
    }
    private patienceTracker() {
        this.waitedPatiently = true;
    }
}
```

## Type Assertions

Type assertion (`x as SomeType`) is unsafe. it only silences the TypeScript compiler, but do not insert any runtime checks to match these assertions, so they can cause your program to crash at runtime.

```ts
(x as Foo).foo(); // BAD

// GOOD
if (x instanceof Foo) {
    x.foo();
}
```

## Type Assertions and Object Literals

Use type annotations (: Foo) instead of type assertions (as Foo) to specify the type of an object literal. This allows detecting refactoring bugs when the fields of an interface change over time.

```ts
// BAD
const foo = {
    bar: 123,
    bam: 'abc'
} as Foo;

// GOOD
const foo: Foo = {
    bar: 123,
    bam: 'abc'
};
```

## Member property declarations

Interface and class declarations must use the ; character to separate individual member declarations:

```ts
//GOOD
interface Foo {
    memberA: string;
    memberB: number;
}

// BAD
interface Foo {
    memberA: string,
    memberB: number,
}
```

## Optimization compatibility for module object imports

When importing a module object, directly access properties on the module object rather than passing it around. This ensures that modules can be analyzed and optimized. Treating module imports as namespaces is fine.

```ts
// GOOD
import { method1, method2 } from 'utils';
class A {
    readonly utils = { method1, method2 };
}

// BAD
import * as utils from 'utils';
class A {
    readonly utils = utils;
}
```

## Debugger statements

Debugger statements must not be included in `production` code.

```ts
function debugMe() {
    debugger;
}
```

# Type System

## Type Inference

Code may rely on type inference as implemented by the TypeScript compiler for all type expressions

```ts
// BAD: 'Set' is trivially inferred from the initialization
const x: Set<string> = new Set();

// GOD
const x = new Set<string>();
```

## Nullable/undefined type aliases

```ts
// BAD
type CoffeeResponse = Latte | Americano | undefined | null;

// GOOD
type CoffeeResponse = Latte | Americano;
```

## Null vs. Undefined

Prefer not to use either for explicit unavailability

```ts
// BAD
return null;

// GOOD
return undefined;
```

Use == null / != null (not === / !==) to check for null / undefined

```ts
// BAD
if (error !== null)

// GOOD
if (error != null)

```

## Optionals vs |undefined type

In addition, TypeScript supports a special construct for optional parameters and fields, using `?`:

```ts
interface CoffeeOrder {
  sugarCubes: number;
  milk?: Whole|LowFat|HalfHalf;
}

function pourCoffee(volume?: Milliliter) { ... }

```

## Interfaces vs Type Aliases

TypeScript supports `type aliases `for naming a type expression. This can be used to name primitives, unions, tuples, and any other types.

However, when declaring types for objects, use `interfaces` instead of a `type alias` for the object literal expression.

```ts
// GOOD
interface User {
    firstName: string;
    lastName: string;
}

//BAD
type User = {
    firstName: string;
    lastName: string;
};
```

## Array<T> Type

For simple types (containing just alphanumeric characters and dot), use the syntax sugar for arrays, `T[]`, rather than the longer form `Array<T>`.

```ts
const a: string[]; // GOOD

const f: Array<string>; // BAD
```

## any Type

Consider not to use `any`, In circumstances where you want to use `any`, consider one of:

-   Provide a more specific type
-   Use `unknown`
-   Suppress the lint warning and document why

## Tuple Types

If you are tempted to create a Pair type, instead use a `tuple type`:

```ts
// BAD
interface Pair {
  first: string;
  second: string;
}
function splitInHalf(input: string): Pair {
  ...
  return {first: x, second: y};
}


// GOOD
function splitInHalf(input: string): [string, string] {
  ...
  return [x, y];
}

// Use it like:
const [leftHalf, rightHalf] = splitInHalf('my string');
```

## Source Organization

-   Limit the length of code to `500–600 lines` in a file and the methods to `20–30 lines.`

-   Avoid using repetitive` if else` (Use` switch case` instead)

-   Use `try catch` to handle the error and exceptions

```ts
try {
    throw new Error('Error');
} catch (error) {
    console.error(error);
}
```

-   Add `TODO` for pending code blocks.

-   `Reuse` the code as much as possible and `remove unnecessary code` blocks.

## Quotes
Prefer single quotes `(')` unless escaping.


## Space
Use `2` spaces. Not `tabs`.

