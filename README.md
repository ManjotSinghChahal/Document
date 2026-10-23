## Class lifecycle methods

componentDidMount, componentDidUpdate, componentWillUnmount

Component Mount → componentDidMount

Component Update → componentDidUpdate

Component Unmount → componentWillUnmount

useEffect(() => {
console.log("Mounted");

return () => {
console.log("Unmounted");
};
}, []);

Class Component Hooks

constructor() → useState / useMemo
componentDidMount() → useEffect(..., [])
componentDidUpdate() → useEffect(..., [dependencies])
componentWillUnmount() → useEffect cleanup
shouldComponentUpdate() → React.memo / useMemo
componentDidCatch() → Error Boundary

Method Importance Purpose

constructor() ⭐⭐⭐ Initialize state/bind methods
render() ⭐⭐⭐⭐⭐ Render UI
componentDidMount() ⭐⭐⭐⭐⭐ Initial API/setup
shouldComponentUpdate() ⭐⭐⭐⭐ Optimize rendering
componentDidUpdate() ⭐⭐⭐⭐⭐ Handle updates
componentWillUnmount() ⭐⭐⭐⭐⭐ Cleanup
getDerivedStateFromProps() ⭐⭐ Derive state from props
getSnapshotBeforeUpdate() ⭐⭐ Capture info before update
getDerivedStateFromError() ⭐⭐⭐ Error fallback state
componentDidCatch() ⭐⭐⭐ Error logging

JavaScript is a synchronous, single-threaded programming language.This means the JavaScript engine has only one call stack and executes code sequentially, line by line, waiting for one task to finish before moving to the next.
However, JavaScript can behave asynchronously to handle time-consuming tasks (like API requests or timers) without freezing the user interface

## Closures

A closure is when a function has access to variables (can read and change them) defined in its outer scope, even when the function is executed outside of the scope where it was defined. A closure is a function enclosing a reference (variable) to its outer scope. Functions can access variables outside of their scope.

function outerFunction() {
let outerFuncVar = "outside";
function innerFunction() {
console.log(`The value is: ${outerFuncVar}`);
}
return innerFunction();
}

outerFunction();

or

function counter() {
let count = 0;

return function () {
count++;
return count;
};
}

const increment = counter();

increment(); // 1
increment(); // 2
increment(); // 3

Even though counter() has finished, the returned function still has access to count because of the closure

---

# Rest & Spread operator

Spread operator(...): want to duplicate the content of an array or combine or combine multiple arrays/objects together.

Rest operator(...): The rest operator(...) is used in function parameters to collect all remaining arguments into an array.

# Nullish coalescing

const name = user.name ?? "Guest";

?? falls back when the value is null or undefined, unlike ||, which also treats values such as 0, false, and "" as falsy.

# Map / Filter / Reduce

map, filter, and flatMap return arrays; find returns one item; some/every return booleans; reduce returns an accumulated result. 

forEach()  iterates over array elements, returns undefined

map() transforms each item and returns a new array of the same length.

filter() keeps only the items that match a condition and returns a new array that may be shorter.

map() and filter() do not modify the original array; both return a new array.
Methods such as sort(), reverse(), and splice() modify the original array.

find	
used to get the first item that matches a condition
[1, 2, 3].find(n => n > 1) => 2

findIndex
used to get the first item position that matches a condition
[1, 2, 3].findIndex(n => n > 1) => 1

some	
Check whether at least one item matches then return true else false
[1, 2, 3].some(n => n > 2) => true

every	
Check whether all items match	then return true else false
[1, 2, 3].every(n => n > 0) => true

reduce	
Combine items into one value	
[1, 2, 3].reduce((sum, n) => sum + n, 0) => 6

includes	
Check whether an exact value exists	['a', 'b'].includes('b') => true

replace
TBC

flatMap	
Map items, then flatten one level	['a b', 'c'].flatMap(s => s.split(' ')) => ['a', 'b', 'c']

sort	
Sort items in place	[3, 1, 2].sort((a, b) => a - b) => [1, 2, 3]

slice	
used to extract subarray from an array, original array remains unchanged
extracts elements starting at the start index up to, but not including, the end index.

const numbers = [10, 20, 30, 40, 50];
const subset = numbers.slice(1, 4); 
console.log(subset);  // Output: [20, 30, 40]
console.log(numbers); // Output: [10, 20, 30, 40, 50] (Original remains unchanged)


splice	
used to add, delete and replace existing items in an array, original array is changes
array.splice(index, how_many_item_to_remove, item1, ..., itemX)

```js
Examples
At position 2, add 2 elements:
const fruits = [Banana, Orange, Apple, Mango];
fruits.splice(2, 0, Lemon, Kiwi); // Banana,Orange,Lemon,Kiwi,Apple,Mango

At position 2, remove 2 items:
const fruits = [Banana, Orange, Apple, Mango, Kiwi];
fruits.splice(2, 2); // Banana,Orange,Kiwi

At position 2, replace element
const fruits = [Banana, Orange, Apple, Mango, Kiwi];
fruits.splice(2, 1, 'PineApple'); // Banana, Orange, PineApple, Mango, Kiwi
```


# Event loop

console.log("1");

setTimeout(() => {
console.log("2");
}, 0);

Promise.resolve().then(() => {
console.log("3");
});

async function test() {
console.log("4");

await Promise.resolve();

console.log("5");
}

test();

console.log("6");

Expected conceptual ordering:

1
4
6
3
5
2

Understanding this requires you to combine:

Call Stack +
Promises +
async/await +
Microtasks +
Tasks +
Event Loop

Generics vs Any

Generics allow reusable code while preserving the relationship between input and output types. any removes type safety, while generics allow TypeScript to infer and enforce the actual type.

Union Types

A union means a value can be one of several types.

Intersection Types

An intersection combines multiple types.

# Generics

any: Disables type checking entirely — TypeScript stops tracking the value's type, so no autocomplete, no error-catching, and no relationship enforced between related values.

Generics: A placeholder type (T) that gets bound to a concrete type per call, letting TypeScript infer and enforce a consistent relationship between inputs and outputs while keeping full type safety

function identity<T>(arg: T): T {
return arg;
}

function identity(arg: any): any {
return arg;
}

const output = identity("hello");
output.toUpperCase(); // Fine
output.toFixed(2); // For any - crashes at runtime!, For T - give compile time error

A union means a value can be one of several types.
let id: string | number;

id = 10;
id = "abc";

An intersection combines multiple types.

interface User {
id: number;
name: string;
}

interface Admin {
permissions: string[];
}

type AdminUser = User & Admin;

# Utility types

Partial
Required
Readonly
Pick
Omit
Record
ReturnType
Parameters

Partial<T>

Makes all properties optional.

interface User {
name: string;
age: number;
}

type UpdateUser = Partial<User>;

Equivalent to:

{
name?: string;
age?: number;
}

Required<T>

Makes all properties required.

type RequiredUser = Required<User>;

Readonly<T>

Prevents properties from being reassigned through that type.

interface User {
id: number;
name: string;
}

type ReadonlyUser = Readonly<User>;
equivalent to:
{
readonly id: number;
readonly name: string;
}

const u: ReadonlyUser = { id: 1, name: "Alice" };

u.name = "Bob"; // ERROR: Cannot assign to 'name' because it is a read-only property

Pick<T, K>

Pick = "I already have a type. I just want a smaller version of it with only certain fields, and I want that smaller version to stay in sync with the original automatically."

interface User {
id: number;
name: string;
email: string;
passwordHash: string;
address: string;
phone: string;
createdAt: Date;
}

Now you're building a <UserCard> component that just shows a name and email — nothing else.

Without Pick, you'd write a separate interface by hand:

typescript
interface UserCardProps {
name: string;
email: string;
}

Problem: if User.email ever changes type (say, becomes string | null), you have to remember to go update UserCardProps too. They're not actually linked — just two similar-looking interfaces that can silently drift apart.

With Pick, you derive it directly from User:

typescript
type UserCardProps = Pick<User, "name" | "email">;

Omit<T, K>

Remove specific properties.

type UserWithoutAge = Omit<User, "age">;

Record<K, T>

Record<K, T> is a TypeScript utility type that constructs an object type with a fixed set of keys K, where every key maps to a value of type T

type Status = "pending" | "shipped" | "delivered";

const statusLabel: Record<Status, string> = {
pending: "Pending",
shipped: "Shipped",
delivered: "Delivered",
// omit one → compile error
};

ReturnType<T>

Gets the return type of a function.
ReturnType<T> extracts the return type of a function type, without you having to write that type out by hand.
API client functions where the return shape comes from a library you don't control

function getUser() {
return {
id: 1,
name: "John",
};
}

type User = ReturnType<typeof getUser>;
// { id: number; name: string }

keyof

keyof produces a union of the property names of a type.

interface User {
id: number;
name: string;
email: string;
}

type UserKey = keyof User;

Result:

"id" | "name" | "email"

typeof

It can also extract the type of an existing value.

any vs unknown
any disables type checking, while unknown preserves type safety and requires narrowing before the value can be used.

type vs interface

# Security

- Avoid logging secrets keys, access/refresh token, any sensitive data
- Short lived access token
- Adding certificate pinning
- Store data in keystore / Keychain
- Stop reverse engineering

The React Native screen is slow. What would you do?

1. Reproduce the problem
   ↓
2. Measure/profile
   ↓
3. Identify JS vs UI/native bottleneck
   ↓
4. Inspect React rendering
   ↓
5. Inspect expensive computations
   ↓
6. Inspect lists/images/animations
   ↓
7. Optimize the actual bottleneck
   ↓
8. Measure again

First I measure where the bottleneck is, then optimize the relevant layer.
Inspect why taking time
Memory leaks
Check if doing effective work or api is taking time
Use useMemo, useCallback

Memory leak
Growing cache
Subscription leak
Timer leak
Large retained objects
Unbounded state

How do you handle a production crash you cannot reproduce?

Crash reporting
↓
Stack trace + source maps/symbolication
↓
OS/device/version analysis
↓
Breadcrumbs
↓
Identify common path
↓
Inspect recent release changes
↓
Create reproduction
↓
Fix
↓
Test
↓
Staged rollout
↓
Monitor

How would you upgrade a large production React Native app?

My approach:

1. Read upgrade notes
2. Audit dependencies
3. Identify breaking changes
4. Create isolated upgrade branch
5. Upgrade incrementally
6. Resolve Android native changes
7. Resolve iOS native changes
8. Update incompatible libraries
9. Run unit/integration tests
10. Test release builds
11. Test critical user flows
12. Staged rollout
13. Monitor crashes/performance

React Native upgrades can involve:

Gradle
Android SDK/toolchain
CocoaPods
Xcode
native configuration
dependency compatibility

How would you handle API race conditions?

Example:

Request A → "React"
Request B → "React Native"

B finishes first
A finishes later

If A updates the UI after B, the UI becomes stale.

Solutions:

Cancellation

Cancel A when B starts.

Request identity

Only allow the latest request to update state.

How would you handle a crash that only happens in production?

I would:

Crash report
↓
Symbolicate stack
↓
Identify affected versions/devices
↓
Check release-specific configuration
↓
Reproduce release build
↓
Find root cause
↓
Fix
↓
Regression test
↓
Staged rollout
↓
Monitor

I investigate differences such as:

Hermes/release behavior
minification
R8/ProGuard
native dependencies
permissions
environment variables
production API configuration
device-specific behavior

# Call, Bind & Apply

call() and apply() serve the exact same purpose. The only difference between how they work is that call() expects all parameters to be passed in individually, whereas apply() expects an array of all of our parameters.

call, apply, and bind are functions that explicitly control the this value. call and apply invoke the function immediately, while bind creates a new function with the specified this value that can be invoked later. call accepts arguments individually, whereas apply accepts them as an array.

var pokemon = {
firstname: 'Pika',
lastname: 'Chu ',
getPokeName: function() {
var fullname = this.firstname + ' ' + this.lastname;
return fullname;
}
};

var pokemonName = function(snack, hobby) {
console.log(this.getPokeName() + ' loves ' + snack + ' and ' + hobby);
};

pokemonName.call(pokemon,'sushi', 'algorithms'); // Pika Chu loves sushi and algorithms
pokemonName.apply(pokemon,['sushi', 'algorithms']); // Pika Chu loves sushi and algorithms

var logPokemon = pokemonName.bind(pokemon); // creates new object and binds pokemon.

logPokemon('sushi', 'algorithms'); // Pika Chu loves sushi and algorithms

or

function greet(age, city) {
console.log(this.name, age, city);
}

const user = { name: "John" };

// Execute immediately
greet.call(user, 25, "Delhi");

// Execute immediately
greet.apply(user, [25, "Delhi"]);

// Create function, don't execute
const greetUser = greet.bind(user);

// Execute later
greetUser(25, "Delhi");

Prototype
A prototype is an object that provides properties and methods that other objects can access through inheritance.

const userMethods = {
name: 'manjot'
}
const user = Object.create(userMethods);
console.log(user) // {}
console.log(user.name) // manjot

Here, userMethods is the prototype of user. The name is not directly stored on user; user accesses it from its prototype.

Prototype Chain

The prototype chain is the sequence of prototypes JavaScript searches when a property or method isn't found directly on an object.

const animal = {
eat() {
console.log("Eating");
}
};

const dog = Object.create(animal);

dog.bark = function () {
console.log("Barking");
};

dog.bark(); // Barking
dog.eat(); // Eating

Prototype: A mechanism that allows objects to share properties and methods, helping with inheritance and memory efficiency.

Prototype chain: The lookup mechanism JavaScript uses to find a property or method by moving from the object to its prototype and then further up the chain.

Optional Chaining (?.)

Optional chaining lets you safely access a nested property or call a function when a value might be null or undefined. Instead of throwing an error, JavaScript returns undefined.

const user = {
profile: {
name: "John"
}
};
console.log(user.profile?.name); // John
console.log(user.address?.city); // undefined
user.address.city; // TypeError

Nullish Coalescing ??

Nullish coalescing provides a default value when the left side is specifically null or undefined.
prints false, 0 if on left side
?? doesn't replace valid falsy values such as 0, false, or ""

const name = null;

const displayName = name ?? "Guest";

console.log(displayName); // Guest

Modules

A module is a separate JavaScript file that encapsulates related code and allows you to share only what other files need. Modules help organize large applications and avoid putting everything in one file.

Example:

// user.js
export const userName = "John";
// profile.js
import { userName } from "./user";

console.log(userName);

In React Native, almost every file you create can be treated as a module.

Named Exports / Default Exports

Named ExportsCount:

- You can have as many named exports in a single file as you want.
- Import Syntax: You must use curly braces {} when importing
- The names must exactly match the exported identifiers (unless renamed with as).

// Exporting (math.js)
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// Importing (app.js)
import { add, subtract } from './math.js';

Default ExportsCount:

- A file can only have a single default export.
- Import Syntax: You do not use curly braces when importing
- Can name the imported variable whatever you like.

// Exporting (math.js)
const add = (a, b) => a + b;
export default add;

// Importing (app.js) - any name works
import sumFunction from './math.js';

Memory Leaks

A memory leak happens when your application keeps references to objects that are no longer needed, preventing the garbage collector from releasing their memory.

Common causes:

Unremoved event listeners
Timers that continue running
Subscriptions not cleaned up
Global references
Unnecessary closures/references

Debouncing

Debouncing ensures a function executes only after a specified period of inactivity. Every new event resets the timer.

Example use case: Search input

User types: H → He → Hel → Hell → Hello
↓
wait 500ms
↓
API request

Instead of making an API request for every keystroke, you wait until the user stops typing.

Concept: Debouncing → Execute after the activity stops.

Throttling

Throttling limits how frequently a function can execute within a specific time interval.

Example use case: Scroll events

Scroll events:
||||||||||||||||||||||||||
↓ ↓ ↓ ↓
Execute every 200ms

Even if the scroll event fires hundreds of times, the function executes at most once per defined interval.

Concept: Throttling -> Execute at a controlled maximum frequency.

Debounce vs Throttle
Debounce -> "Wait until activity stops."
Throttle -> "Run at most once every X milliseconds."

Currying

Currying is used to break down complex function calls into smaller, more manageable steps. It transforms a function that accepts multiple arguments into a sequence of functions where each function accepts one argument and returns the next function

Normal function:

const add = (a, b, c) => a + b + c;

add(1, 2, 3);

Curried version:

const add = a => b => c => a + b + c;

add(1)(2)(3);

The first function receives a, returns another function that receives b, which returns another function that receives c.

Concept: Currying -> Breaking a multi-argument function into a chain of single-argument functions.

In real applications, currying can be used when a function has some fixed configuration and some dynamic values. We provide the fixed value once, get a reusable function, and provide the changing value when needed. This is commonly useful for API utilities, event handlers, validation, logging, and configuration-based functions.

const request = (token) => (url) => {
return fetch(url, {
headers: {
Authorization: `Bearer ${token}`
}
});
};

const authenticatedRequest = request(token);

authenticatedRequest("/users");
authenticatedRequest("/orders");
authenticatedRequest("/profile");

Strict mode makes JavaScript execution more restrictive and helps detect errors early by enforcing safer language rules.

"use strict";

name = "John"; // ❌ ReferenceError

# Error Handling

React Native errors can come from JavaScript or native iOS/Android code.
Approach of error handling is catch, log, provide message

- 1. Error Boundaries

Error Boundaries are introduced by React to catch JavaScript errors

- are used basically to handle rendering issues (also handles lifecycle methods, Constructors), Get Callback of error, show a fallback UI

```js
class ErrorBoundary extends React.Component {
  state = {
    hasError: false,
  };

  static getDerivedStateFromError(error) {
    return {
      hasError: true,
    };
  }

  componentDidCatch(error, info) {
    console.log(error);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }

    return this.props.children;
  }
}
```

### Error Boundaries DON'T handle

- ❌ Event handlers
- ❌ Asynchronous callbacks such as `setTimeout`
- ❌ Errors from Promise rejections
- ❌ Errors inside the Error Boundary itself
- ❌ Native iOS/Android crashes

These require other mechanisms.

- 2. try/catch

`try/catch` handles errors when our JavaScript code explicitly executes the operation.

Common use cases:

- Event handlers
- Synchronous functions that may throw
- `async/await` operations
- API/network operations that reject

```js
try {
  const response = await fetchUser();
} catch (error) {
  console.log(error);

  // Report to Sentry
}
```

For asynchronous code, remember that `try/catch` only catches a rejected Promise when the operation is properly awaited inside the `try` block.

```js
try {
  await fetchUser();
} catch (error) {
  // Caught
}
```

- 3. Global JavaScript Errors

Some JavaScript errors may not be handled by Error Boundaries or local `try/catch`.

For these cases, applications can use a global JavaScript error handler to capture/report unhandled exceptions.

The purpose is mainly to log and report unexpected errors, rather than trying to recover from every error.

---

- 4. Native Errors / Crashes

Native errors occur in the **iOS or Android layer**, for example:

- Swift / Objective-C
- Kotlin / Java
- Native modules

A native crash is outside the JavaScript Error Boundary mechanism. The app may terminate and the JS runtime may no longer be available.

For production monitoring, use a **native crash-reporting service**, such as:

- Sentry
- Firebase Crashlytics

These can capture native crash information and stack traces.

For specific native exception-handling requirements, a native exception handler can also be used.

In React Native, I handle errors at different levels. Error Boundaries handle JavaScript errors during component rendering and lifecycle execution. I use try/catch for errors in event handlers, synchronous operations, and async operations such as API calls. For unexpected unhandled JavaScript exceptions, I use a global error handler. Native iOS and Android crashes are handled separately using native crash reporting tools such as Sentry or Crashlytics.

Sentry

Sentry is an application monitoring platform that is used to track JavaScript errors, native crashes, and performance issues in production. In React Native, I can integrate the Sentry SDK and capture exceptions from Error Boundaries or try/catch blocks. Sentry provides stack traces, device information, app versions, breadcrumbs, and other context that helps identify and debug production issues.

const handleLogin = async () => {
try {
await loginUser();
} catch (error) {
Sentry.captureException(error);
}
};

Higher-Order Functions (HOF)

A Higher-Order Function is a function that takes another function as an argument, returns a function, or both.
It is commonly used for abstraction, callbacks, reusable logic

const calculate = (a, b, operation) => {
return operation(a, b);
};

const add = (a, b) => a + b;

calculate(10, 20, add); // 30

map, filter, and reduce are HOFs because they accept callback functions.

users.map(user => user.name);
users.filter(user => user.active);
users.reduce((total, item) => total + item.price, 0);

# Hoisting

declarations of variables and functions to the top of their scope during the compilation phase

var is hoisted
declaration is hoisted to the top, but its value is not assigned until the code execution reaches the variable's initialization. This results in the variable being assigned undefined during the hoisting phase.

let & const
let and const are also hoisted, but they remain in a Temporal Dead Zone (TDZ) from the start of the block until their declaration is encountered. Accessing them before their declaration will throw a ReferenceError.

console.log(a); // ReferenceError: Cannot access 'b' before initialization
let a = 10;

console.log(b) // x is not defined

Temporal Dead Zone (TDZ) is period between entering a scope and initializing the variable

TDZ is the period between entering a scope and initializing a let or const variable. During this period, the variable exists but cannot be accessed, otherwise JavaScript throws a ReferenceError.

TDZ is the period where a let or const variable has been created in its scope but not yet initialized, so accessing it before initialization throws a ReferenceError.

Benefits of TDZ
Catch Bugs Early
Prevent Accidental Access

```js
{
  console.log(x); // ❌ ReferenceError
  let x = 10; // TDZ ends here
  console.log(x); // 10
}
```

- Function Declaration Hoisting
  Function declarations are hoisted with both their name and the function body. This means the function can be called before its definition in the code.

greet(); // "Hello, Mahima!"
function greet() {
console.log("Hello, Mahima!");
}

- Function Expression Hoisting
  Function expressions are treated like variable declarations. The variable itself is hoisted, but the function expression is not assigned until the line of execution. This means calling the function before its assignment will result in an error.

```js
hello(); // TypeError: hello is not a function
var hello = function () {
  console.log('Hi!');
};
```

```js
for (let i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i); // 0,1,2
  }, 100);
}
```

```js
for (var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i); // 3,3,3
  }, 100);
}
```

Re-declaring Variables - `var` works fine but not in `let` and `const`

```js
var x = 1;
function test() {
  console.log(x); // undefined
  var x = 2;
}

test();
```

```js
var x = 10;
{
  console.log(x); // 10
  var x = 20;
}
```

var → function scoped
let → block scoped
const → block scoped

```js
let x = 10;
{
  console.log(x); // Cannot access 'x' before initialization
  let x = 20;
}
```

Is hoisting actually moving declarations to the top of the code?

Answer: No. Hoisting is a way to describe JavaScript's behavior during creation/initialization of execution contexts. JavaScript doesn't literally move your source-code declarations to the top.

Type inference
Type inference is TypeScript's ability to automatically determine a value's type from its context or assigned value.
var name = 'Manjot' // name → string

Tuples
A tuple is a special type of array where the number of elements and the type of each position are fixed.

// Regular array - any number of strings
let names: string[] = ["Alice", "Bob", "Charlie"];

// Tuple - exactly 2 elements: string, then number
let person: [string, number] = ["Alice", 25];

The key difference is that tuples have a fixed structure. The first element must always be a string, the second must always be a number, and there can be no third element. We can update value corresponding to defined type

Real-world example
const [count, setCount] = useState(0);

Conceptually, useState returns a tuple:
[number, function]

# Type Annotation, Type literal, Type alias and Enum

Type annotations are an explicit way to tell the compiler what type of data a variable, function parameter, return value, or object property should hold.

let username: string = "Alice";

Type literal restricts a value to exact values.

Type alias gives a reusable name to a type and can represent objects, unions, tuples, etc. An enum defines named constants and unlike type aliases, it has a runtime representation.

```js
// Literal type
type Status = "loading" | "success" | "error";

// Type alias
type User = {
  id: number;
  status: Status;
};

// Enum
enum UserRole {
  Admin = "ADMIN",
  User = "USER"
}
```

# Null vs undefined
In JavaScript, null represents an intentional absence of any object value, while undefined means a variable has been declared but has not yet been assigned a value
Type: null -> object |  undefined -> undefined


# Primitive Types and Non Primitive Types

Primitive Types
immutable - can not change value

JavaScript has 7 primitive types:

- string "John"
- number 25, 10.5
- bigint 123n
- boolean true
- undefined undefined
- null null
- symbol Symbol("id")

Strings are immutable.

let name = "John";
name[0] = "M";
console.log(name); // "John"

name = "Jack";
console.log(name); // "Jack"
The variable is reassigned to another value not modified.

You cannot directly change a character inside the existing string. Operations create a new string

console.log(10 / 0); // Infinity
console.log("hello" \* 2); // NaN

Undefined
Means a variable exists but has not been assigned a value.

Null
null represents an intentional absence of a value.

Symbol

A Symbol creates a unique primitive value.
const id1 = Symbol("id");
const id2 = Symbol("id");

Non-Primitive Types

Non-primitive types are generally objects and can contain collections of values and behavior.

The main examples are:
Object
Array
Function
Date
Map
Set
RegExp
etc.

const user = {};
user.name = "John"; // ✅
const prevents reassignment of the variable, not mutation of the object.

<!--
typeof "hello";      // "string"
typeof 10;           // "number"
typeof true;         // "boolean"
typeof undefined;    // "undefined"
typeof 10n;          // "bigint"
typeof Symbol();     // "symbol"
typeof {};           // "object"
typeof [];           // "object"
typeof function(){}; // "function"
typeof null;         // "object"
-->

Is null primitive or non-primitive?

typeof null === "object"; // true

But null is actually a primitive value.
So: It is a famous JavaScript historical quirk.
Object — shallow copy

```js
const user1 = {
  name: 'John',
};
const user2 = { ...user1 };
user2.name = 'Mike';
console.log(user1.name); // John
console.log(user2.name); // Mike
```

Spread creates a new object.

But nested objects are still shared:

```js
const user1 = {
  name: 'John',
  address: {
    city: 'Delhi',
  },
};

const user2 = { ...user1 };
user2.address.city = 'Mumbai';
console.log(user1.address.city); // Mumbai
```

Because the nested address object is still referenced by both.

The Most Important Difference: Value vs Reference

```js
Primitive → copied by value
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
console.log(b); // 20

Object → reference behavior
const user1 = {
  name: "John"
};

const user2 = user1;

user2.name = "Mike";

console.log(user1.name); // Mike
console.log(user2.name); // Mike
```

```js
const a = {};
const b = a;
console.log(a === b); // true

const user = {
  name: 'John',
};
const copy = user;
copy.name = 'Mike';
console.log(user.name); // Mike
because spread operator creates new_object

const a = [1, 2, 3];
const b = a;
b.push(4);
console.log(a); // [1, 2, 3, 4]
Arrays are objects.Therefore, assignment copies the reference.

let a = {
  count: 1
};
let b = a;
a = {
  count: 2
};
console.log(b.count); // 1 -> b still references the original object.
```

- Mutation vs reassignment
  This distinction is very important.

Mutation
const user = {
name: "John"
};
user.name = "Mike";

Same object, changed internally.

Reassignment
let user = {
name: "John"
};
user = {
name: "Mike"
};

Variable now points to a different object.

console.log("5" + 2); // 52
console.log("5" - 2); // 3

console.log([] == false); // true
console.log([] === false); // false

const a = []; // or {}
const b = a;

console.log(a == b); // true
console.log(a === b); // true

Object.is()
Object.is() is a static method in javascript that is use to compare whether the two values are same or not. It behaves almost identically to the strict equality operator (===), except for how it handles NaN and signed zeros (-0 and +0).

console.log(0 === -0); // true
console.log(Object.is(0, -0)); // false

Object.is() treats two signed zeros (+0 & -0) as unequal, and treats two NaN as equal. While in the case of equality operators the things are quite opposite.

To check whether the passed value is an array or not
Array.isArray([]); // true

# React Query or TanStack Query
library used for server state management in React applications. 
handles fetching, updating, caching and refreshing data.

queryKey -	Identifies a particular piece of cached data.
queryFn -	Fetches the data and returns it, or throws an error.
useQuery -	Reads and fetches data for a query key.
useMutation -	Creates, updates, or deletes data.
invalidateQueries -	Marks matching cached data as stale so it can refresh.

isPending: there’s no successful data yet. The request may be running, paused, or waiting to run.
isFetching: a request is currently in progress, including a background refresh.
isError: the latest query attempt ended in an error.
data: the last successful result, if one exists.

staleTime - controls how long cached data is considered fresh. Fresh data generally won’t be refetched by automatic stale-data triggers.
gcTime - controls how long inactive cached data stays in memory after no component is using it.

# Redux


# SOLID
| Letter | Principle |
|---|---|
| **S** | Single Responsibility |
| **O** | Open/Closed |
| **L** | Liskov Substitution |
| **I** | Interface Segregation |
| **D** | Dependency Inversion |

- Single Responsibility Principle (SRP):• A class should have only one job or reason to change.
 Example: A Student class should only hold student data, not handle database saving or email sending.

- Open/Closed Principle (OCP):• Software parts should be open for extension, but closed for modification.
  Example: Add new features by writing new classes instead of changing working code.

- Liskov Substitution Principle (LSP):• Child classes must be able to replace their parent classes      without breaking the program.
  Example: If a function uses a base bird type, passing a duck or a penguin subclass should not cause unexpected errors or crashes.

- Interface Segregation Principle (ISP):• Clients should not be forced to depend on methods they do not use.
  Example: Split large, bloated interfaces into smaller, specific ones so classes only implement what they need.
 
- Dependency Inversion Principle (DIP):• High-level modules should not depend on low-level modules; both should depend on abstractions.
  Example: Depend on interfaces or abstract classes instead of hardcoding concrete implementations, making it easy to swap components.

  # Old and New React Native Architecture

  Threads
  - JS thread (javascript code resides)
  - UI thread (UI paint on native side)
  - Render thread
  - Background thread (Yoga Engine means layout calculation)

  Metro Bundler - React Native’s Metro Bundler compiles and bundles all your JavaScript files into a single minified file, MainBundle.js.

  Hermes - the new JS Engine
  JSI - Direct communication
  Turbo Modules - Next generation native modules
  Fabric - the new rendering system

  ![alt text](<Screenshot 2026-10-06 at 09.20.09.png>)
  ![Old Architecture of react Native](https://cdn-images-1.medium.com/max/3504/1*K_fgQAbLpCSA2YZFAuswAQ.png)

  Why arch changes

  In Old arch, communication between JavaScript and native code happens through bridge
 - serialized/deserialized - JSON data is serialized and passed through bridge and on native side it is deserialized, this task is time consuming
 - asynchronous communication -  lags UI

Drawbacks
1. Performance Issues
Communication between JavaScript and Native is slow because messages are passed asynchronously through the Bridge.
Scrolling may lag if UI updates arrive too late.

2. No Shared Memory
JavaScript and Native don’t share memory.
Instead of referencing a data object, React Native copies it in JSON format, which slows things down.

3. Heavy Native Module Loading
React Native loads all native modules at startup, even if some aren’t needed, which increases load time.


New Architecture replaces that communication model with JSI,

New arch
- JSI
- Fabric
- Codegen
- Turbo modules - Lazy loading, does not load all content on app launch which reduces app startup time

JSI (JavaScript Interface)
Earlier Arch
JS Thread -> Serialize JSON -> Bridge Queue -> Deserialize JSON -> Native

New
JS Thread -> JSI -> Native

JavaScript holds direct C++ memory references to native object

replaced the bridge from the OLD architecture and provide a direct, native interface to JavaScript objects and functions.
Through the JSI, Native methods will be exposed to JavaScript via C++ objects. These objects can be referenced by JavaScript code, thus can be invoked directly.

No serialization/deserialization
Synchronous execution
Code sharing - Contains reference to native object and does not require to pass copy of data

What is Fabric?
<!-- Fabric removes the Bridge, communication between between JavaScript and Native is faster.
synchronous communication
Allows JavaScript and Native to share memory, reducing the need to copy data
Improves UI responsiveness and smoothness -->

Fabric executes UI operations directly across threads using immutable C++ shadow trees. UI rendering flows through a strict 3-phase pipeline: 

- Render Phase: React creates an Element Tree in JavaScript, which prompts the creation of a parallel C++ Shadow Tree.
- Commit Phase: The layout engine (Yoga) calculates sizes, constraints, and positions directly in the C++ layer.
- Mount Phase: The final C++ tree is mapped directly to physical iOS and Android platform UI components.

What are TurboModules?
TurboModules allow React Native to load only the native modules that are needed, instead of loading everything at startup and reduces app startup time.
Camera, GPS, Bluetooth Api's etc - Earlier it loaded all but loaded when required

Codegen (Native Code Generator)
Codegen specially do these 2 jobs in new architecture of React Native

- Static type checking
- Generate Native code

Let's understand these 2 more clearly below.

Static type checking
JavaScript is a dynamically typed (you don't need to define the type of a variable) language, and JSI is written in C++, which is a statically typed (you must have to define variable type) language. Consequently, there are some communication issues between the two (JS and C++).

That's why the new architecture includes a static type checker called CodeGen. It solves the type related communication issue between JavaScript & C++.

Generate Native code
You write `typed JavaScript code` to define the Native components or Native modules that you want to use in your app.

Codegen creates `native code` for each platform as output from your written `typed JavaScript code`.

These generated `native code` can communicate with `typed JavaScript code` directly, without using the bridge. As a result Codegen makes the communication between JavaScript and native code faster and more reliable, since it avoids using the bridge.

CodeGen generates native code at build time instead of run time.


Hermes Engine

# Event Loop
JavaScript is single-thread, event loop is used to manage the execution of multiple tasks without blocking the main thread.
The event loop ensures that tasks are executed in the correct order.

```js

Example: 1
console.log("Start");

setTimeout(() => {
    console.log("setTimeout Callback");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise Resolved");
});

console.log("End");

Start
End
Promise Resolved
setTimeout Callback

Example: 2

console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
  Promise.resolve().then(() => console.log('Promise 2'));
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 1');
  setTimeout(() => console.log('Timeout 3'), 0);
});

setTimeout(() => console.log('Timeout 2'), 0);

console.log('End');


Start
End
Promise 1
Timeout 1
Promise 2
Timeout 2
Timeout 3
```

console.log("Start") executes first.
setTimeout schedules its callbacks but does not execute it immediately.
Promise.resolve().then() is placed in the microtask queue and executes before the callback queue.
Promise Resolved appears before setTimeout Callback due to microtask priority.

Types of Tasks in JavaScript

- Call Stack
The call stack keeps track of function calls. When a function is invoked, it's added to the stack, and when it completes, it's removed.

Example:
```js
function greet() {
  console.log('Hello');
}

function sayGoodbye() {
  console.log('Goodbye');
}

greet();
sayGoodbye();


greet() is called and added to the stack.
console.log("Hello") is executed.
greet() is removed from the stack.
sayGoodbye() is called and added to the stack.
console.log("Goodbye") is executed.
sayGoodbye() is removed from the stack.
```

- Microtasks
High-priority asynchronous tasks that need to run immediately after the current operation completes like Promise

Macrotasks or Task queue or Callback queue
Lower-priority asynchronous tasks that are added to call stack for execution when the stack is empty like setTimeout, setInterval.

When the synchronous call stack becomes empty, the Event Loop will completely drain the Microtask queue before executing even a single item from the Macrotask

Order of Execution
Execute all synchronous tasks on the call stack.
Process all microtasks in the microtask queue.
Process the first task in the macrotask queue.
Repeat.

![Event Loop](https://media.geeksforgeeks.org/wp-content/uploads/20260414180013593526/event_loop_in_javascript.webp)

![Event Loop Structure](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*0xDGBNrA1WtfSfYY3FJOdw.gif)

# Promises
Promises are used for handling asynchronous operations like API calls, file loading, or time delays easier. It can be in one of three states

Pending: The task is in the initial state.
Fulfilled: The task was completed successfully, and the result is available.
Rejected: The task failed, and an error is provided.

```js
let checkEven = new Promise((resolve, reject) => {
    let number = 4;
    if (number % 2 === 0) resolve("The number is even!");
    else reject("The number is odd!");
});
checkEven
    .then((message) => console.log(message)) // On success
    .catch((error) => console.error(error)); // On failure
```

Promise.all() Method
Waits for all promises to resolve and returns their results as an array. If any promise is rejected, it immediately rejects.
lets independent operations proceed concurrently and fulfills when all fulfill.

```js
Promise.all([
    Promise.resolve("Task 1 completed"),
    Promise.resolve("Task 2 completed"),
    Promise.reject("Task 3 failed")
])
    .then((results) => console.log(results))
    .catch((error) => console.error(error));
```

Promise.allSettled() Method
Waits for all promises to settle (fulfilled or rejected) and returns results.

Promise.race()
resolves or rejects as soon as the first promise settles.

Promise.any() Method
resolves with the first fulfilled promise. If all are rejected, it rejects with an AggregateError.

Promise.resolve() Method
returns a promise that resolves with the given value.

Promise.reject() Method
returns a promise that immediately rejects with a given reason.

Promise.finally() Method
runs a cleanup or final code block regardless of the promise’s result (fulfilled or rejected).

```js
console.log('A');

const promise = new Promise(resolve => {
  console.log('B');
  resolve('done');
});

promise.then(value => console.log(value));
console.log('C');

A
B
C
done
```

The executor logs B right away. The .then() callback runs later as a microtask, after the current synchronous code finishes. If the promise is already settled when .then() is attached, its callback is still scheduled; it does not run immediately.

Promises and async/await use the same underlying promise scheduling model; async/await is syntax that makes promise-based code easier to write and read.

What happens with async/await
await pauses only the surrounding async function. It does not pause the whole program or block the JavaScript thread:

```js
async function work() {
  console.log('inside: before');
  const value = await promise;
  console.log('inside: after', value);
}

work();
console.log('outside');

inside: before
outside
inside: after done
```

The function runs synchronously up to await. Then it suspends, allowing the surrounding code to continue. When the promise settles, the function resumes in a later microtask. If the promise rejects, await throws the rejection reason inside the async function, where try/catch can handle it.

Promises and async/await are two ways to handle asynchronous operations. A Promise represents a value that may be available later, and can handle it with .then() and .catch(). async/await is syntax built on Promises that makes the same flow easier to read: an async function returns a Promise, and await pauses that function until a Promise settles. It doesn't block the JavaScript thread; other code can continue running. I use try/catch with async/await for errors, or .catch() with a Promise chain.

FlatList vs FlashList

- FlatList (Virtualization): As you scroll, FlatList actively mounts new components and unmounts old ones that leave the viewport. This continuous creation and destruction of views causes significant garbage collection overhead, layout recalculation on the JavaScript thread, and the infamous "white blank flashes" during fast scrolling.

- FlashList (Cell Recycling): Instead of destroying off-screen components, FlashList utilizes a recycling pool. It maintains a small, fixed number of native views and simply re-binds them with new data as they scroll into view. This reduces JavaScript thread work, keeps memory flat, and eliminates scroll lag.

FlashList (built by Shopify) is a high-performance alternative to React Native's built-in FlatList, designed to provide smoother scrolling and lower memory usage for large lists. While they share an almost identical API to make switching easy, their underlying architectural approaches to rendering items are fundamentally different

data -> Array passed
keyExtractor -> a unique key for each item so React can track rows when data is inserted, removed, or reordered
extraData -> tell the list about changes outside data (if value changes, it re-render the list)

when you pass a new data reference to FlatList, it detects the change and updates the affected rows.
setItems(current => [...current, newItem]); // new array -> FlatList updates

Avoid mutating the existing array:
items.push(newItem);
setItems(items); // same array reference -> FlatList may not update
Because FlatList uses shallow comparison, it checks whether the data prop changed by reference. If you mutate the existing array, create a new array instead. And if a row depends on state outside data-such as a selected ID-pass that value through extraData

Used for pagination
onRefresh={onRefresh}
onEndReached={loadMore}
onEndReachedThreshold={0.5}
The threshold is measured in visible list lengths: 0.5 means the callback may fire when the end is within half a viewport

Re-render optimization

- Use image wisely

- memoize renderItem: When you define renderItem inline (directly inside the JSX), React creates a new function on every render. This causes FlatList to think the list items need to be re-rendered, even if the data hasn't changed.

- Use removeClippedSubviews for memory optimization
Setting removeClippedSubviews={true} can help reduce memory usage by unmounting components that are outside the viewport.

- Implement Pagination
When working with very large datasets, loading and rendering all items at once, even with optimizations, can still hurt performance and user experience. That's where pagination come in.

- Use getItemLayout for fixed-height rows
If your list items have a fixed height (or at least you know their height in advance), implementing getItemLayout helps FlatList calculate positions without measuring each item dynamically.

- Use a stable keyExtractor

Before:  key 0 -> Apple
         key 1 -> Banana
         key 2 -> Cherry

After:   key 0 -> Banana
         key 1 -> Cherry

// ❌ Avoid this!
keyExtractor={(item, index) => index.toString()} 

// ✅ Better
keyExtractor={item => item.id}

Using the array index as the key is easy. Problems arise when a row has its own state or other retained component state. For example, if the Apple row had an expanded panel or text input value in local state, that state may now appear on Banana's row because React reused the component instance.   


Avoid inline styles, use StyleSheet
Avoid calculations inside JSX
Use debounce to prevent extra renders & API calls
Use Fragments instead of unnecessary View wrappers (Cross check)
Use FastImage
Avoid racing conditions -When calling the same API multiple times, ensure you only use the latest response.


# Passed by Value / Passed by Reference

- Primitives are Passed by Value

Primitive types (Number, String, Boolean, null, undefined, Symbol, BigInt) pass an independent, fresh copy of the actual data. Modifying it inside a function does not change the original variable.

```js
function modifyPrimitive(val) {
    val = 100; // Overwrites the local copy
}

let num = 10;
modifyPrimitive(num);
console.log(num); // Output: 10 (Original value is unchanged)

```

- Objects are Passed by "Copy of the Reference"

For Reference types (Objects, Arrays, Functions), the "value" being copied and passed is the memory address (reference) pointing to where the object sits in memory.
Because both the original variable and the function parameter point to the exact same object, any modification to its properties affects the original data.

```js
function modifyObject(obj) {
    obj.age = 25; // Modifies a property of the shared object
}

let user = { name: "Amit", age: 20 };
modifyObject(user);
console.log(user.age); // Output: 25 (The original object was mutated)
```

# Deep linking
used to navigate to sepcific screen from external link if app is installed


Types of Deep Links
- Custom URL Schemes (myapp://): The simplest option. It opens the app if installed, but fails or does nothing if the app is missing.
- Universal Links (iOS) & App Links (Android) (https://myapp.com): Standard web links that route seamlessly into the app if it is installed. If the app is missing, it falls back to opening the link in stores.

Android
In `AndroidManifest.xml` file under <intent-filter> tag we add `scheme`

<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <!-- Accept myapp:// -->
    <data android:scheme="myapp" /> 
</intent-filter>

iOS
Info.plist
Open your project in Xcode, navigate to your target's Info tab, expand URL Types, and add a new URL Type:

(Note: For Universal Links and App Links, you will also need to configure Entitlements/Associated Domains and host an apple-app-site-association or .well-known/assetlinks.json file on your server).

# Use native driver
native driver offloads animation calculations from the JavaScript thread to the native UI thread, preventing frame drops.

# Interaction Manager
InteractionManager allows long-running work to be scheduled after any interactions/animations have completed. In particular, this allows JavaScript animations to run smoothly.

InteractionManager.runAfterInteractions(() => {
  // ...long-running synchronous task...
});

# Immediately invoke functions expression - IIFE 
An Immediately Invoked Function Expression (IIFE) is a function that runs automatically as soon as it is defined
(function () {
  console.log("Runs immediately!");
})();
- anonymous function
- Preventing Global Scope Pollution

# Arrow Functions / Fat Arrow
 arrow functions are a concise way to define functions in JavaScript
 do not require to define return and parentheses if single statement

const res = (num) => {
 return num * 2
}
const res = (num) => num * 2


- The arrow function does not have its own binding to this, and we cannot directly change this in an arrow function.

```js
const person = {
  name: "Maya",

  regularFunction: function () {
    return this.name;
  },

  arrowFunction: () => this.name,
};

console.log(person.regularFunction()); // "Maya"
console.log(person.arrowFunction());   // Usually undefined
```

Advanced understanding:
Event Listeners & Callbacks:- Dynamic execution context is highly apparent during callbacks (like setTimeout) or DOM event handling.
Regular functions lose their connection to the host object when treated as independent callbacks because their execution context defaults back to the global scope or undefined (in strict mode). Arrow functions fix this by locking onto the surrounding context.

```js
const timer = {
  count: 1,
  start() {
    // Regular function loses context here:
    setTimeout(function() {
      console.log('Normal:', this.count) // undefined
    }, 1000);
    
    // Arrow function captures 'this' safely:
    setTimeout(() => {
       console.log('Arrow:', this.count) // 1
    }, 1000);
  }
};
 timer.start()
```

The arrow function doesn't get this from person; it inherits this from the surrounding scope.
In the arrow function, this doesn't refer to person but to the global context (or undefined in strict mode).
In the regular function, this dynamically binds to person when called as a method.

- Arrow functions do not have their own arguments, uses rest operator to collect arguments

```js
function showArgs(a, b) {
  console.log(arguments[0], arguments[1]);
}
showArgs("A", "B"); // "A" "B"

const showArgs = (...args) => console.log(args);
showArgs("A", "B"); // ["A", "B"]
```

- Arrow functions cannot be used as constructors. Calling them with new throws a TypeError

```js
function fetch1() {
}

const fetch2 = () => {
}

const r1 = new fetch1() // ✅ Works fine
const r2 = new fetch2() // ❌ TypeError: fetch2 is not a constructor
console.log("Result:");
```

- Regular functions support hoisting whereas arrow fn not

 Function Declaration vs Function Expression

Function Declaration
A function declaration is defined as a standalone statement. The JavaScript engine loads it into memory during the compilation phase, making it available anywhere within its scope.

// This works perfectly because of hoisting!
greet(); 
function greet() {
  console.log("Hello!");
}

Function Expression
A function expression is created when a function is assigned inside a variable or used as a value (like a callback). Because it follows variable assignment rules, you cannot call it before it is defined.

// This will throw a ReferenceError or TypeError
sayGoodbye(); 
const sayGoodbye = function() {
  console.log("Goodbye!");
};

# Object.freeze vs Object.seal and Object.assign()

Object.freeze()
Object.freeze() makes an object completely immutable (read-only)
secures only immediate properties of the target object, but if a property points to a nested object or array, that inner reference remains fully mutable.

Object.seal() prevents adding or removing properties but still allows you to change the values of existing properties.

```js
const user = { name: "Alice", role: "Admin", About: { name: 'Andrew' } };

Object.freeze(user);

user.name = "Chahal"

user.About.name = 'John'
user.About.age = '35'

console.log(user) // { name: 'Alice', role: 'Admin', About: { name: 'John', age: '35' } }
```


To check whether obj is sealed/frozen or not:
Object.isSealed(obj)
Object.isFrozen(obj)


Object.assign()
used to copy from one or more source objects into a target object.

```js
const target = { a: 1 };
const source = { b: 2, c: 3 };
Object.assign(target, source); // Merges source into target
console.log(target); // { a: 1, b: 2, c: 3 }
```

# setNativeProps
used to modify view directly without triggering a full component re-render via state or props.

Use when: Frequent re-rendering via setState causes a noticeable performance bottleneck in smooth animations or specific edge cases.

```js
  const handleToggle = () => {
    isToggled = !isToggled;
    // 2. Direct manipulation without triggering a component re-render
    if (boxRef.current) {
      boxRef.current.setNativeProps({
        style: {
          backgroundColor: isToggled ? '#4CD964' : '#FF3B30', // Green vs Red
        },
      });
    }
  };
```

- Direct Manipulation: It acts as the React Native equivalent to modifying a DOM node directly in a web browser.
- Native Layer State: It stores state inside the native layer (UIView on iOS or native views on Android) rather than inside your React component tree.
- Performance Tool: It is designed as an escape hatch to avoid the overhead of re-rendering and reconciling large component hierarchies during high-frequency tasks like continuous animations.
- Common Use Case: It can update a TextInput value directly to bypass character-dropping issues that sometimes happen with controlled components during fast typing.

# Handling Responsiveness in React Native for All Devices

- Flexbox

- Dimensions API - to get the screen's width and height

- Percentage-Based Dimensions

- react-native-responsive-screen or react-native-size-matters library

- Orientation Handling: Using the useWindowDimensions Hook

# Types of Timers in javascript
- setTimeout(): Executes a callback function exactly once after a specified minimum delay in milliseconds.
	 Cancellation: clearTimeout(id).
- setInterval(): Executes a callback function repeatedly at a fixed time interval.
	 Cancellation: clearInterval(id)

# String Polyfills


# Fast Image
Smoother experience as it provides Cache mechanism
Set priority for image to display - FastImage.priority.high
Improved scrolling performance

# ES6 (ECMAScript 2015)
- let and const Declarations
- Arrow Functions
- Destructuring Assignment
- Spread and Rest Operators
- Classes
- Promises
- Default Parameters
- Maps and Sets

# Strict Equality vs Loose Equality
- === (Strict Equality): Compares both the value and the data type. No automatic conversions happen. If the types are different, it immediately returns false.
- == (Loose Equality): Compares only values by performing type coercion. It tries to convert the variables into a matching type before making the

null == undefined  // true
null === undefined // false

# Limitations of Arrow functions
- No Own this Binding: Arrow functions do not have their own this. Instead, they lexically inherit this from the surrounding outer scope.
- Cannot Be Used as Constructors
- No arguments Object: Arrow functions do not have a local arguments object. If you need access to dynamic arguments, you must use rest parameters 

# Event bubbling and Event capturing
Event bubbling and event capturing are the two ways an event propagates (travels) through the DOM tree when an action happens on an element that is nested inside other elements


- Event Bubbling
Direction	Bottom-Up: From the target element up to the window.	
Execution Order	Inner element fires first, then outer parents.	

Imagine you have a button inside a div . When you click the button, the click event doesn’t just stay on the button, it also triggers on the div that contains the button, and then on any other parent elements of that div. This upward movement of events is called Event Bubbling.

- Event Capturing 
Top-Down: From the window down to the target element.
Outer parent fires first, then inner elements.
Reverse of event bubbling

# this keyword
In JavaScript, `this` depends on how a function is called , it may refer to an object, global object, or be undefined (in strict mode), and its value is determined at runtime based on how the function is invoked.

- Global Context (Outside any function)
When used outside of any function, this refers to the global object.
In a browser, this points to window.

- Using this in a Method
this keyword refers to the object itself, allowing the method to access, interact with, and modify the object’s properties and behavior within its scope.

```js
const person = {
    name: 'John',
    age: 30,
    greet() {
        console.log('Hello, my name is ' +
            this.name + ' and I am '
            + this.age +
            ' years old.');
    }
};

person.greet(); // Hello, my name is John and I am 30 years old.
```

# Controlled vs Uncontrolled Components
A controlled component is one where React controls the state of the component. This means that the component's state is managed by React itself, typically through props.

Uncontrolled components manage their state internally. Instead of relying on React to manage state updates, uncontrolled components directly interact with the DOM to retrieve and update their state.
through the useRef hook

# Strict Mode
Enables strict checking of the code quality, deprecation checks and warnings for its child components.

```js
<StrictMode>
  <App />
  </StrictMode>
```

# Shallow Copy vs Deep Copy
A shallow copy copies the top-level properties of an object or array. However, it does not copy nested objects or arrays; instead, it references them. This means changes to nested objects affect both the original and the copy.

Methods to Create a Shallow Copy:
Spread Operator: { ...obj }
Object.assign(): Object.assign({}, obj)
Array.prototype.slice(): For arrays: array.slice()

A deep copy, on the other hand, creates a completely independent copy of the object, including all nested objects or arrays. This ensures that changes made to one object do not affect the other.

Methods to Create a Deep Copy:
JSON.parse(JSON.stringify())
Using Libraries (e.g., Lodash)
Custom Recursive Function
structuredClone (Modern JavaScript)


```js
*Shallow Copy*
const original = {
  name: "Sagar",
  address: { city: "Jaipur" }
};

const shallow = {...original}

shallow.address.city = 'Abohar'
console.log(original) // { name: 'Sagar', address: { city: 'Abohar' } }

**Deep Copy**
const original = {
  name: "Sagar",
  address: { city: "Jaipur" }
};


const deepCopy = structuredClone(original);
deepCopy.name = 'Manjot'
deepCopy.address.city = 'Abohar'

console.log(original) // { name: 'Sagar', address: { city: 'Jaipur' } }

/*
Limitation of JSON Deep Copy:
Cannot copy functions
Loses undefined, Symbol, Date, Map, Set, circular refs

structuredClone is modern way, other way is 
const deepCopy = JSON.parse(JSON.stringify(original)) // object
const deepCopy = original.map(item => ({ ...item })); // array
*/
```

# How do you achieve 60 FPS performance?
Offload Animations from the JS Thread
Optimize Complex Lists (Migrate to FlashList)
Maximize Image Efficiency
Handle Unnecessary Re-renders
Defer Heavy Computations

# How do you detect and fix memory leaks?
When an application does not release the memory it has used, back to the operating system which progressively degrades performance and eventually triggers out-of-memory (OOM) crashes

Common causes
- Do not release resources: not unsubscribing to events, disconnecting sockets, or releasing external resources after they are no longer needed
- Reference error: Objects are not released when there are no longer references to them, leading to a memory leak.
- Un-cancelled Asynchronous Requests
- Unnecessary re-rendering


Detecting and fixing memory leaks in React Native requires tackling two different phases: the JavaScript (JS) runtime and the Native side (iOS/Android).
Use React Native Debugger tool (React DevTools, Chrome DevTools, Flipper etc)
Use Native Debugger tool

# Lazy loading
As an apps grows in functionality and features, they often become burdened with longer load times and increased memory usage, which can negatively impact the user experience.

Lazy loading is a powerful technique that allows developers to optimise their apps by loading components, screens, or resources only when they are needed. By deferring the loading of non-essential parts of the app until they are required, lazy loading can significantly reduce the initial load time, improve memory management, and provide a more efficient use of network resources

- we can lazy load List, Image, Component/Screen

# Approaches for styling component
- Inline Style 
Styles are written directly inside the component using double curly braces.
Creates new object references on every render, degrades performance, and non-reusable code.

- StyleSheet API (StyleSheet.create())
Built-in api, styles separate from component and makes them easy to reuse
Fatest among three

- Styled-Components
Styled-components is a 3rd party library that allows you to write style directly within JavaScript components, leveraging the capabilities of CSS-in-JS. It is widely popular due to its simplicity and flexibility, enabling you to encapsulate style and logic in a single file clearly and concisely

Under Hood: parses CSS strings and runs prop-based functions at render time, then converts the result to style objects. That costs extra JS work
It is the slowest of the three, because styles are processed at render time.

```js
import styled from 'styled-components/native';

const Container = styled.View`
  flex: 1;
  background-color: white;
  justify-content: center;
`;
```

Combine diff stlyes with an array
style={[styles.text, isActive && styles.activeText]}

# Design Patterns
By using design patterns, developers can make their code more organized, maintainable, and reusable. The choice of design pattern depends on the specific requirements of the application and the developer’s preferences.

- Compound Component Pattern:
The Compound Component Pattern is a way to design components that are loosely coupled but still work together as a cohesive whole. Instead of passing a ton of props down the component tree, the parent component manages the shared logic, while the child components focus on their independent concerns.

```js
import Card from './src/componant/card';
import { Header } from './src/componant/card/header';
import { Content } from './src/componant/card/content';
import { Footer } from './src/componant/card/footer';

        {/* Card Component with Header, Content, and Footer */}
        <Card>
          <Card.Header>
            <Header />
          </Card.Header>
          <Card.Content>
            <Content />
          </Card.Content>
          <Card.Footer>
            <Footer />
          </Card.Footer>
        </Card>
```
Here, we’re composing the Card component by passing in the Header, Content, and Footer as its children. Each of these child components is defined separately, but they are now part of the Card structure, thanks to the compound pattern.

Benefits of Using the Compound Pattern:
Readability: The code becomes easier to read because the Card component encapsulates the layout logic, and the child components (Header, Content, Footer) focus only on their content.

Maintainability: If you need to make changes to the layout or shared logic, you can do so in the parent Card component without modifying each individual child.

Separation of Concerns: Each component (parent and child) has a clear responsibility, making the codebase easier to manage and extend.

- Container/Presentational Components: 
The Container/Component pattern is a design pattern that separates the logic from the presentation of the component. The idea behind this pattern is to have two types of components — container components and presentational components. Container components are responsible for handling the logic and data fetching, while presentational components are responsible for rendering the UI.

For example, imagine you have a component that needs to fetch data from an API and render it in a list. Instead of putting all the logic and rendering in the same component, you can create a container component that handles the API call and data fetching, and a presentational component that renders the list.

- Stateful/Stateless Components: 
Stateful/Stateless Components is a design pattern that distinguishes between components that manage their own state and components that do not. Stateful components are responsible for managing the state of the application, while stateless components do not have any state of their own. This pattern helps to keep the code more organized and makes it easier to reason about the application.

For example, imagine you have a component that needs to keep track of whether a button has been clicked or not. Instead of duplicating the state management across all the components, you can create a stateful component that handles the state and passes it to a stateless component.

- Atomic Design: 
Atomic Design is a design pattern that involves breaking down the application into smaller, reusable components. This pattern involves creating components at the atomic level, such as atoms, molecules, and organisms. Atoms are the smallest components, such as buttons and inputs, while molecules are groups of atoms, such as a form. Organisms are groups of molecules, such as a navigation bar. This pattern helps to make the code more modular and reusable.

For example, imagine you have a form that includes a text input and a submit button. Instead of creating the form as a single component, you can break it down into smaller components, such as an input component and a button component. These components can be reused across the application.

- Higher-Order Components (HOC): 
Higher-Order Components (HOC) is a design pattern that allows you to reuse code between components. This pattern involves creating a function that takes a component and returns a new component with additional functionality.

For example, imagine you have a component that needs to fetch data from an API. Instead of duplicating the data fetching code in multiple components, you can use a HOC that adds the data fetching functionality to the component.

- Provider Pattern: 
The Provider Pattern is a design pattern that allows you to pass data or functionality down the component tree without having to pass props manually at each level. This pattern involves creating a provider component that wraps the component tree and provides the data or functionality using Context.

For example, imagine you have an application that needs to access a user’s authentication information. Instead of passing the user’s information as props down the component tree, you can use a provider component that wraps the component tree and provides the user’s information using Context.

- Component Injection: 
Component Injection is a design pattern for injecting components into other components to add functionality. This pattern involves creating a component that accepts other components as props and renders them in a specific way to add functionality.

For example, imagine you have a component that needs to display a tooltip. Instead of duplicating the tooltip code in multiple components, you can use a component that injects the tooltip component.

- Dependency Injection: 
Dependency Injection is a design pattern that allows you to inject dependencies into components to decouple the code. This pattern involves passing the dependencies as props to the component, making it easier to test and reuse the component.

For example, imagine you have a component that needs to use an API client to fetch data. Instead of creating the API client in the component, you can pass it as a prop to the component.

Link: https://medium.com/@mr.kashif.samman/design-patterns-for-react-native-applications-630e5eb9e34f

