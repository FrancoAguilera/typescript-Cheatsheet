# TS Cheatsheet

_there are many like this, but this is mine_

### Advantages

- Optional static typing (the key here is optional)
- creates a type check layer above js
- Great tooling support with IntelliSense
- The ability to compile down to a version of JavaScript that runs on
  all browsers

## Install

```
sudo npm install -g typescript
yarn add global typescript
```

## Usage

```javascript
tsc --init // will starts tsconfig.json
yarn add --dev prettier // optional

// .prettierrc minimal example
{
  "semi": true,
  "trailingComma": "none",
  "singleQuote": true,
  "tabWidth": 2,
  "printWidth": 80
}
```

```typescript
// tsconfig.json example
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "skipLibCheck": true
  }
}

```

**tsconfig.json** doc on [TS Config reference](https://www.typescriptlang.org/tsconfig)

### Basic data types

- string

```typescript
let color: string = "blue";
```

- number

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

- boolean

```typescript
let isDone: boolean = false;
```

- void

```typescript
// void: When we don'0t return anything
const doSomething = (): void => {
  console.log("Do something");
};
```

- any

```typescript
// "any" type turns off TS type check (avoid the use of any), it's not a
// solution, it's just the start for bigger problems
let foo: any = "a string";
foo = 12345;
console.log(foo);
```

- never

```typescript
// a function with "never" can't be executed to the end
const doSomethingTwo = (): never => {
  throw "never";
};
```

- null

```typescript
let n: null = null;
```

- undefined

```typescript
let u: undefined = undefined;
```

- array

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

- tuple

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

- enum

```typescript
enum Color {
  Red = 1,
  Green = 2,
  Blue = 4,
}
let c: Color = Color.Green;
```

- unknown

```typescript
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

## Examples

### functions params and returns

```typescript
const getFullName = (name: string, surName: string): string => {
  return `${name} ${surName}`;
};

console.log(getFullName("Franco", "Aguilera"));
```

### Objects and interfaces

```typescript
// object with ts params
const user: { name: string; surname: string; age: number } = {
  name: "Franco",
  surname: "Aguilera",
  age: 30,
};

// will throw an error about the missing params
const userTwo: { name: string; surname: string; age: number } = {
  name: "Luciano",
};

// With and interface
interface IUser {
  name: string;
  age?: number; // ? optional operator
}

const user: IUser = {
  name: "Franco",
  age: 30,
};

const userTwo: IUser = {
  name: "Franco",
};

console.log(user.age);

// functions type in interfaces
interface IUser {
  name: string;
  age?: number;
  getMessage(): string;
}

const user: IUser = {
  name: "Franco",
  age: 30,
  hi: () => `Hello world!`,
};

console.log(user.hi());
```

### Generics <T, S, G>

```typescript
// Generics allow creating 'type variables' which can be used to create
// classes, functions & type aliases that don't need to explicitly define the
// types that they use.

const createPair = <S, T>(v1: S, v2: T): [S, T] => {
  return [v1, v2];
};

console.log(createPair(42, "foo")); // [42, "foo"]
console.log(createPair(true, { bar: "baz" })); // [ true, { bar: 'baz' } ]
```

### Union operator

```typescript
// Union operator "|" (variable with several data types)
let fullName: string | boolean = "Franco Aguilera";
let errorMessage: string | null = null;
```

### Type aliases

```typescript
type ID = string;
type PopularTag = string;
type MaybePopularTag = PopularTag | null;

interface IUser {
  id: ID;
  name: string;
  surname: string;
}

const popularTags: PopularTag[] = ["dragon", "coffee"];
const dragonsTag: MaybePopularTag = "dragon";
```

### Type assertion

```typescript
// Type assertion allows you to set the type of a value and tell the
// compiler not to infer it
let code: any = 123;
let employeeCode = code as number;
```

### working on the DOM

```typescript
// TS does not have any access to our markup, but can make static analisys
// of our code

// can use intelisence for DOM inspection
const someElement = document.querySelector(".foo") as HTMLInputElement;
console.log("SomeElement:", someElement.value);

// on adding a listener
const someElement = document.querySelector(".foo");
someElement?.addEventListener("blur", (event: Event) => {
  const target = event.target as HTMLInputElement;
  console.log("event: ", target.value);
});
```

### Classes on TS

_Public, Private, Readonly, and Protected (only exists on TS)_

- public: everything is public by default
- private: it can only be used inside out class
- static properties: can only be used inside class but not on instances
- readonly: a value that cannot be changed

```typescript
interface IUserx {
  getFullName(): string;
}

// classes are sugar around prototypes
class User implements IUserx {
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.unchangeableString = firstName;
  }

  private firstName: string;
  private lastName: string;
  readonly unchangeableString: string;

  getFullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  changeUnchangableName(): void {
    //this.unchangeableString = 'foo' //
  }
}

const user = new User("Franco", "Aguilera");
console.log(user.getFullName());

class Admin extends User {}

const admin = new Admin("Luciano", "Gonzalez");
console.log(admin.getFullName());
```
