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

