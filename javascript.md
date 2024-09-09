# Javascript Quirks

### 1. **Type Coercion**

JavaScript automatically converts types when performing operations, which can lead to unexpected results.

```javascript
console.log("5" - 1); // 4 (string "5" is converted to number 5)
console.log("5" + 1); // "51" (number 1 is converted to string "1")
console.log(false == 0); // true (false is converted to 0)
```

### 2. **Floating-Point Precision**

JavaScript uses floating-point arithmetic, which can lead to precision issues.

```javascript
console.log(0.1 + 0.2); // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3); // false
```

### 3. **`==` vs. `===`**

The `==` operator performs type coercion, while `===` checks for strict equality without type conversion.

```javascript
console.log(1 == "1"); // true (type coercion)
console.log(1 === "1"); // false (no type coercion)
```

### 4. **`NaN` (Not-a-Number)**

`NaN` is a special value in JavaScript that represents an invalid number, and it is not equal to itself.

```javascript
console.log(NaN === NaN); // false
console.log(isNaN(NaN)); // true
```

### 5. **Falsy Values**

JavaScript has several values that are considered "falsy" in conditional statements.

```javascript
if (!0) console.log("0 is falsy"); // "0 is falsy"
if (!"") console.log("Empty string is falsy"); // "Empty string is falsy"
if (!null) console.log("null is falsy"); // "null is falsy"
if (!undefined) console.log("undefined is falsy"); // "undefined is falsy"
```

### 6. **Hoisting**

Function and variable declarations are "hoisted" to the top of their scope.

```javascript
console.log(x); // undefined (hoisted, but not initialized)
var x = 5;

hoistedFunction(); // "Hoisted!"
function hoistedFunction() {
  console.log("Hoisted!");
}
```

### 7. **`this` Keyword**

The value of `this` depends on how a function is called.

```javascript
const obj = {
  name: "Alice",
  getName() {
    console.log(this.name);
  },
};

obj.getName(); // "Alice" (this refers to obj)

const getName = obj.getName;
getName(); // undefined (this refers to global object or undefined in strict mode)
```

### 8. **Global Variables in Browsers**

In the browser, variables declared without `var`, `let`, or `const` become global.

```javascript
function test() {
  globalVar = "I am global!";
}

test();
console.log(globalVar); // "I am global!"
```

### 9. **Closures**

Functions retain access to the variables from their outer scope even after the outer function has returned.

```javascript
function outer() {
  const outerVar = "I'm outside!";
  return function inner() {
    console.log(outerVar); // "I'm outside!"
  };
}

const innerFunction = outer();
innerFunction();
```

### 10. **Immediately Invoked Function Expressions (IIFE)**

IIFEs are functions that are executed immediately after they are defined.

```javascript
(function () {
  console.log("IIFE executed!");
})(); // "IIFE executed!"
```

### 11. **Prototypal Inheritance**

JavaScript uses prototypes for inheritance, rather than classical inheritance found in other languages.

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(`${this.name} makes a sound.`);
};

const dog = new Animal("Dog");
dog.speak(); // "Dog makes a sound."
```

### 12. **Callback Functions**

Functions can be passed as arguments to other functions.

```javascript
function greet(name, callback) {
  console.log(`Hello, ${name}!`);
  callback();
}

greet("Alice", function () {
  console.log("This is a callback function.");
});
```

### 13. **Event Loop and Asynchronous Execution**

JavaScript is single-threaded but uses an event loop to handle asynchronous operations.

```javascript
console.log("Start");

setTimeout(function () {
  console.log("Timeout");
}, 0);

console.log("End");

// Output: "Start", "End", "Timeout"
```

### 14. **Undefined vs. Null**

`undefined` is the default value of uninitialized variables, while `null` is an assignment value representing "no value."

```javascript
let a;
console.log(a); // undefined

let b = null;
console.log(b); // null
```

### 15. **`arguments` Object**

Functions have an `arguments` object containing all passed arguments.

```javascript
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sum(1, 2, 3)); // 6
```

### 16. **Function Scopes vs. Block Scopes**

`var` is function-scoped, while `let` and `const` are block-scoped.

```javascript
function testVar() {
  if (true) {
    var x = 5;
  }
  console.log(x); // 5 (var is function-scoped)
}

function testLet() {
  if (true) {
    let y = 10;
  }
  console.log(y); // Error: y is not defined (let is block-scoped)
}

testVar();
testLet();
```

### 17. **Array-Like Objects**

Objects like `arguments` and `NodeList` are array-like but not true arrays.

```javascript
function example() {
  console.log(arguments); // Array-like object
  console.log(Array.isArray(arguments)); // false
}

example(1, 2, 3);
```

### 18. **Sparse Arrays**

JavaScript allows arrays with missing elements.

```javascript
const sparseArray = [1, , 3];
console.log(sparseArray.length); // 3
console.log(sparseArray[1]); // undefined
```

### 19. **Typeof Quirks**

The `typeof` operator has some quirks, particularly with `null` and functions.

```javascript
console.log(typeof null); // "object" (a known bug in JavaScript)
console.log(typeof function () {}); // "function"
```

### 20. **Adding Properties to Functions**

Functions are objects in JavaScript, so you can add properties to them.

```javascript
function myFunction() {
  console.log("Hello!");
}

myFunction.description = "This is a function";
console.log(myFunction.description); // "This is a function"
```

### 21. **`for...in` vs. `for...of`**

`for...in` loops through object properties, while `for...of` loops through iterable elements.

```javascript
const obj = { a: 1, b: 2, c: 3 };

for (let key in obj) {
  console.log(key); // "a", "b", "c"
}

const arr = [1, 2, 3];

for (let value of arr) {
  console.log(value); // 1, 2, 3
}
```

### 22. **Primitive vs. Reference Types**

Primitive values are copied by value, while objects (including arrays) are copied by reference.

```javascript
let a = 10;
let b = a;
a = 20;
console.log(b); // 10 (primitive values are copied)

let obj1 = { name: "Alice" };
let obj2 = obj1;
obj1.name = "Bob";
console.log(obj2.name); // "Bob" (objects are copied by reference)
```

### 23. **Automatic Semicolon Insertion (ASI)**

JavaScript automatically inserts semicolons in some cases, which can lead to unexpected behavior.

```javascript
function returnObject() {
  return;
  {
    name: "Alice";
  }
}

console.log(returnObject()); // undefined (ASI inserts a semicolon after `return`)
```

### 24. **Mutable vs. Immutable Strings**

Strings are immutable, meaning their characters cannot be changed.

```javascript
let str = "Hello";
str[0] = "J";
console.log(str); // "Hello" (strings are immutable)
```

### 25. **Function Overloading**

JavaScript doesn’t support function overloading in the traditional sense, but you can achieve similar functionality with conditional logic.

```javascript
function add(a, b) {
  if (typeof a === "string" || typeof b === "string") {
    return String(a) + String(b);
  }
  return a + b;
}

console.log(add(1, 2)); // 3
console.log(add("1", 2)); // "12"
```

### 26. **Adding Arrays**

When you add two arrays, JavaScript converts them to strings and concatenates them.

```javascript
console.log([1, 2] + [3, 4]); // "1,23,4" (arrays are converted to strings and then concatenated)
```

### 27. **Array `sort()` Method**

The `sort()` method sorts elements as strings by default, which can lead to unexpected results with numbers.

```javascript
const numbers = [10, 1, 3, 20];
numbers.sort();
console.log(numbers); // [1, 10, 20, 3] (numbers are sorted as strings)
```

### 28. **`0` vs `-0`**

JavaScript has both `0` and `-0`, and while they are almost indistinguishable, they can behave differently in some contexts.

```javascript
console.log(0 === -0); // true
console.log(1 / 0 === 1 / -0); // false (Infinity vs. -Infinity)
```

### 29. **Subtracting Arrays**

Subtracting arrays leads to `NaN`, as JavaScript attempts to convert arrays to numbers but fails.

```javascript
console.log([1, 2] - [3, 4]); // NaN (arrays can't be subtracted)
```

### 30. **`[]` and `null` Coercion**

An empty array `[]` is coerced to `0`, while `null` is coerced to `0` in mathematical operations.

```javascript
console.log([] + 1); // "1" (array is converted to an empty string, then concatenated)
console.log([] - 1); // -1 (array is converted to 0, then subtracted)
console.log(null + 1); // 1 (null is coerced to 0)
```

### 31. **`true` and `false` as Numbers**

`true` is coerced to `1` and `false` to `0` in arithmetic operations.

```javascript
console.log(true + true); // 2 (true is coerced to 1)
console.log(true - false); // 1 (true is 1, false is 0)
console.log(true + "false"); // "truefalse" (true is converted to "true" and concatenated)
```

### 32. **Empty Array in Boolean Context**

An empty array `[]` is truthy, but can behave oddly in comparisons.

```javascript
console.log([] == false); // true (empty array is coerced to an empty string, which is falsy)
console.log([] === false); // false (strict comparison, no type coercion)
console.log(![]); // false (empty array is truthy, so negation is false)
```

### 33. **`null` and `undefined` in Comparisons**

`null` and `undefined` can behave inconsistently in comparisons.

```javascript
console.log(null == undefined); // true (they are considered equal with `==`)
console.log(null === undefined); // false (strict comparison, no type coercion)
console.log(null > 0); // false (null is not coerced)
console.log(null >= 0); // true (null is coerced to 0 in relational comparisons)
```

### 34. **Adding Objects**

When you add objects, JavaScript converts them to strings (`[object Object]`).

```javascript
console.log({} + {}); // "[object Object][object Object]"
```

### 35. **`typeof` for Arrays and `null`**

The `typeof` operator can return unexpected results for arrays and `null`.

```javascript
console.log(typeof []); // "object" (arrays are objects in JavaScript)
console.log(typeof null); // "object" (null is also an object, a known bug)
```

### 36. **`NaN` Is Not Equal to Anything**

`NaN` is not equal to itself, which can lead to confusing checks.

```javascript
console.log(NaN === NaN); // false (NaN is not equal to itself)
console.log(isNaN(NaN)); // true (correct way to check for NaN)
```

### 37. **`parseInt()` with Leading Zeros**

The `parseInt()` function can behave unpredictably with numbers that have leading zeros, particularly in older versions of JavaScript.

```javascript
console.log(parseInt("08")); // 8 (correct in modern JavaScript)
console.log(parseInt("08", 8)); // 0 (interpreted as octal, 8 is not a valid octal digit)
```

### 38. **String and Number Addition**

When you mix strings and numbers with the `+` operator, JavaScript coerces the number to a string.
(As seen in previous examples)

```javascript
console.log(5 + "5"); // "55" (number is coerced to string and concatenated)
console.log("5" - 1); // 4 (string is coerced to number and subtracted)
```

### 39. **Automatic Semicolon Insertion**

See #23 but expanded on here.
JavaScript sometimes inserts semicolons automatically, which can change the behavior of code.

```javascript
const a = {
  value: 1,
};

const b = function () {
  return;
  {
    value: 2;
  }
};

console.log(a); // { value: 1 }
console.log(b()); // undefined (ASI adds a semicolon after return)
```

### 40. **Accessing Non-Existent Properties**

Accessing a non-existent property on an object returns `undefined`, but this can sometimes be unexpected.

```javascript
const obj = { a: 1 };
console.log(obj.b); // undefined (no error, just undefined)
```

### 41. **Equality of Objects**

Objects are compared by reference, not by value, so even identical objects are not equal.

```javascript
const obj1 = { a: 1 };
const obj2 = { a: 1 };

console.log(obj1 == obj2); // false (different references)
console.log(obj1 === obj2); // false (strict comparison, different references)
```

### 42. **`for...in` with Arrays**

Using `for...in` with arrays can lead to unexpected behavior, as it iterates over all enumerable properties.

```javascript
const arr = [1, 2, 3];
arr.customProp = "Hello";

for (let key in arr) {
  console.log(key); // "0", "1", "2", "customProp"
}
```

### 43. **Deleting Array Elements**

Using `delete` on an array element removes the element but doesn't re-index the array, leading to "holes."

```javascript
const arr = [1, 2, 3];
delete arr[1];
console.log(arr); // [1, empty, 3]
console.log(arr.length); // 3 (length remains the same)
```

### 44. **`+` Operator with Objects**

Adding objects to numbers can produce unexpected results due to type coercion.

```javascript
const obj = { value: 42 };
console.log(obj + 10); // "[object Object]10" (object is coerced to a string)
```

### 45. **String Comparison**

Strings are compared lexicographically, which can lead to surprising results.

```javascript
console.log("2" > "10"); // true ("2" is greater than "1" in lexicographic order)
```

### 46. **Setting Object Properties on Primitives**

Primitive values in JavaScript cannot hold properties, but attempting to set a property on a primitive doesn't throw an error.

```javascript
const str = "hello";
str.foo = "bar";
console.log(str.foo); // undefined (setting properties on primitives is ignored)
```

### 47. **Array Length Property**

Setting the `length` property of an array directly can truncate the array.

```javascript
const arr = [1, 2, 3, 4, 5];
arr.length = 3;
console.log(arr); // [1, 2, 3] (array is truncated)
```

### 48. **`undefined` vs `undeclared` Variables**

`undefined` is a value, while accessing an undeclared variable throws an error.

```javascript
let a;
console.log(a); // undefined (variable declared but not initialized)

console.log(b); // ReferenceError: b is not defined (variable is not declared)
```

### 49. **`isNaN()` vs. `Number.isNaN()`**

`isNaN()` coerces the argument to a number before checking, while `Number.isNaN()` doesn't.

```javascript
console.log(isNaN("hello")); // true (coerced to NaN)
console.log(Number.isNaN("hello")); // false (no coercion, "hello" is not NaN)
```

### 50. **Function Declarations vs. Function Expressions**

Function declarations are hoisted, while function expressions are not.
Worth circling back to #16.

```javascript
console.log(declaredFunction()); // "Declared!"

function declaredFunction() {
  return "Declared!";
}

console.log(expressionFunction()); // TypeError: expressionFunction is not a function

var expressionFunction = function () {
  return "Expression!";
};
```
