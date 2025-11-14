## Overview

Both `interface` and `type` allow to define the shape of a object but not identical.
## Interface

Best for object shapes & object-oriented programming pattern.

Interface can extend multiple interfaces

```ts
interface Person {
	name: string;
}

interface Account {
	email: string;
}

interface User extends Person, Account {
	id: number;
}
```

Interface support declaration merging

```ts
interface User {
  age: number;
}

// Merged!
interface User {
  address: string;
}
```

## Type

Can alias **union**, **intersection**, **primitive**, **function**, **tuple**, etc.  

Recommended for complex types

```ts
type User = {
  id: number;
  name: string;
};
```

Type can represent primitive

```ts
type Id = string | number;
```

Type for functions

```ts
type Login = (user: string, pass: string) => boolean;
```

Type for tuple

```ts
type Point = [number, number];
```

Type can extend 

```ts
type Person = { name: string };
type Account = { email: string };

type User = Person & Account & { id: number };
```
