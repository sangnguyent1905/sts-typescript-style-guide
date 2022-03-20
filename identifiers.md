
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
  DEBUG,
}

```


## Abbreviations

use `loadHttpUrl`, not `loadHTTPURL`

## Dollar sign

Identifiers should not generally use `$` unless it is a `Observable` variable.

