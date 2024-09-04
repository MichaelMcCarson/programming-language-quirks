# TypeScript Quirks

### 1. **Type Inference**

```typescript
let message = "Hello, TypeScript!";
// TypeScript infers that `message` is of type `string`
message = 42; // Error: Type '42' is not assignable to type 'string'
```

### 2. **Union and Intersection Types**

```typescript
// Union Type Example
let value: string | number;
value = "Hello"; // OK
value = 42; // OK
value = true; // Error: Type 'boolean' is not assignable to type 'string | number'

// Intersection Type Example
type Person = { name: string };
type Employee = { employeeId: number };

type Worker = Person & Employee;
const worker: Worker = { name: "Alice", employeeId: 123 };
```

### 3. **Literal Types**

```typescript
type Direction = "up" | "down" | "left" | "right";
let move: Direction;
move = "up"; // OK
move = "down"; // OK
move = "north"; // Error: Type '"north"' is not assignable to type 'Direction'
```

### 4. **Mapped Types**

```typescript
type Point = { x: number; y: number };
type ReadOnlyPoint = { readonly [K in keyof Point]: Point[K] };
// Equivalent to:
// type ReadOnlyPoint = { readonly x: number; readonly y: number };

const point: ReadOnlyPoint = { x: 10, y: 20 };
point.x = 30; // Error: Cannot assign to 'x' because it is a read-only property
```

### 5. **Conditional Types**

```typescript
type IsNumber<T> = T extends number ? "Yes" : "No";

type A = IsNumber<number>; // "Yes"
type B = IsNumber<string>; // "No"
```

### 6. **Type Guards**

```typescript
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function printValue(value: string | number) {
  if (isString(value)) {
    console.log(`String: ${value.toUpperCase()}`);
  } else {
    console.log(`Number: ${value.toFixed(2)}`);
  }
}
```

### 7. **Discriminated Unions**

```typescript
interface Square {
  kind: "square";
  size: number;
}

interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Circle;

function getArea(shape: Shape): number {
  switch (shape.kind) {
    case "square":
      return shape.size * shape.size;
    case "circle":
      return Math.PI * shape.radius * shape.radius;
  }
}
```

### 8. **Generics**

```typescript
function identity<T>(value: T): T {
  return value;
}

let stringIdentity = identity("Hello"); // type of stringIdentity is string
let numberIdentity = identity(42); // type of numberIdentity is number
```

### 9. **Generic Constraints**

```typescript
function getLength<T extends { length: number }>(arg: T): number {
  return arg.length;
}

getLength("Hello"); // OK
getLength([1, 2, 3]); // OK
getLength(42); // Error: Argument of type '42' is not assignable to parameter of type '{ length: number; }'
```

### 10. **Decorators**

```typescript
function readonly(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  descriptor.writable = false;
}

class MyClass {
  @readonly
  myMethod() {
    console.log("Hello");
  }
}

const myClass = new MyClass();
myClass.myMethod = () => console.log("Modified"); // Error: Cannot assign to 'myMethod' because it is a read-only property
```

### 11. **Type Assertions**

```typescript
let someValue: unknown = "Hello, TypeScript!";
let strLength: number = (someValue as string).length;
```

### 12. **Index Signatures**

```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Alice"];
let myStr: string = myArray[0];
```

### 13. **Readonly and Immutable Types**

```typescript
interface Point {
  x: number;
  y: number;
}

let point: Readonly<Point> = { x: 10, y: 20 };
point.x = 30; // Error: Cannot assign to 'x' because it is a read-only property
```

### 14. **Utility Types**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type PartialUser = Partial<User>; // All properties of User are optional

const partialUser: PartialUser = { name: "Alice" }; // OK

type ReadonlyUser = Readonly<User>; // All properties of User are readonly

const readonlyUser: ReadonlyUser = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
readonlyUser.name = "Bob"; // Error: Cannot assign to 'name' because it is a read-only property
```

### 15. **Type Aliases vs. Interfaces**

```typescript
// Type Alias
type AliasUser = {
  id: number;
  name: string;
};

// Interface
interface InterfaceUser {
  id: number;
  name: string;
}

let userAlias: AliasUser = { id: 1, name: "Alice" };
let userInterface: InterfaceUser = { id: 1, name: "Alice" };
```

### 16. **Module Augmentation**

```typescript
// In some third-party library (e.g., lodash)
declare module "lodash" {
  interface LoDashStatic {
    customMethod(): void;
  }
}

// In your code
import _ from "lodash";

_.customMethod = () => {
  console.log("Custom method");
};

_.customMethod(); // Works with your custom method
```

### 17. **Declaration Merging**

```typescript
interface MyInterface {
  prop1: string;
}

interface MyInterface {
  prop2: number;
}

const obj: MyInterface = { prop1: "Hello", prop2: 42 };
```

### 18. **Ambient Declarations (`declare`)**

```typescript
// Declaring a global variable from a JavaScript library
declare var jQuery: (selector: string) => any;

jQuery("#myElement").hide();
```

### 19. **Enums**

```typescript
enum Direction {
  Up = 1,
  Down,
  Left,
  Right,
}

let dir: Direction = Direction.Up;
```

### 20. **Tuple Types**

```typescript
let tuple: [string, number];
tuple = ["Hello", 42]; // OK
tuple = [42, "Hello"]; // Error: Type 'number' is not assignable to type 'string'
```

### 21. **Optional Chaining (`?.`) and Nullish Coalescing (`??`)**

```typescript
interface User {
  name?: string;
  address?: {
    city?: string;
  };
}

let user: User = {};

console.log(user?.address?.city); // undefined

let username = user.name ?? "Guest"; // "Guest" if user.name is null or undefined
```

### 22. **Template Literal Types**

```typescript
type EventName = `on${Capitalize<string>}`;
type ClickEvent = `onClick`; // Valid EventName
type MoveEvent = `onmove`; // Error: 'onmove' does not match EventName pattern
```

### 23. **Branded Types**

```typescript
type Brand<T, U> = T & { __brand: U };

type USD = Brand<number, "USD">;
type EUR = Brand<number, "EUR">;

let usdAmount: USD = 10 as USD;
let eurAmount: EUR = 10 as EUR;

usdAmount = eurAmount; // Error: Type 'EUR' is not assignable to type 'USD'
```

### 24. **Type Narrowing**

```typescript
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase()); // TypeScript knows id is a string here
  } else {
    console.log(id.toFixed(2)); // TypeScript knows id is a number here
  }
}
```

### 25. **Type Predicates**

```typescript
function isNumber(value: any): value is number {
  return typeof value === "number";
}

function printValue(value: string | number) {
  if (isNumber(value)) {
    console.log(`Number: ${value.toFixed(2)}`);
  } else {
    console.log(`String: ${value.toUpperCase()}`);
  }
}
```
