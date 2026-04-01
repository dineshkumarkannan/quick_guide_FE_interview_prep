# Deep copy and shallow copy 
### shallow copy 
- Only the top level is copied 
- spread operator (...)
- Object.assign()
- Array.from(), Array.prototype.concat() 
```js
let arr = [1, 2];
let arr1 = Array.from(arr);
arr1.push(3);
console.log(arr1); // [1, 2, 3]
console.log(arr); // [1, 2]
```

### deep copy
- Copy the entire object structure, including all nested structure 
- JSON.parse(JSON.stringify()) (has limitations with functions, Dates, circular references)
- structuredClone() (modern, built-in method for robust deep cloning, however still cannot clone functions or DOM nodes)
- `_.cloneDeep()` Third-party library function

# typeof() and instanceof

- typeof() used to check for primitive data type. return value - 'string', 'number', 'bigint', 'boolean', 'undefined', 'object', 'symbol'
- instanceof() used to check if object is as instance of which constructor / class. return value - true / false

# IIFE

- Immediately Invoked Function Expression 
- function runs as soon as it is defined  
- design pattern primarily to create a private scope for variables, thus avoiding the pollution of the global namespace

```js
(function(){
 // code to be executed immediately
})()
```

# spread operator, rest parameter
 
### spread operator
- expands an iterable(like an array or a string) or object into an individual element or property

```js

// 1. Merging arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2]; 
// combined is [1, 2, 3, 4, 5, 6]

// 2. Copying arrays or objects
const original = [1, 2, 3];
const copy = [...original]; // Creates a new array with same elements

// 3. Passing arguments to function 
function sum(a, b, c) { return a + b + c; }
const nums = [1, 2, 3];
console.log(sum(...nums)); // Output: 6

// 4. Merging objects
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const merged = { ...obj1, ...obj2 }; // merged is { a: 1, b: 2, c: 3, d: 4 }

```

### rest parameter
- gathers an indefinite number of arguments into a single array or the remaining into a new object

```js

// 1. Handling variable-length function arguments
function sum(...numbers) { 
  return numbers.reduce((total, num) => total + num, 0); 
}
console.log(sum(1, 2, 3, 4, 5)); // Output: 15

// 2. Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// 3. Object destructuring 
const { firstName, lastName, ...otherInfo } = { firstName: 'John', lastName: 'Doe', age: 30, city: 'NYC' };
console.log(firstName); // 'John'
console.log(otherInfo); // { age: 30, city: 'NYC' }

```

# synchronous vs asynchronous 

#### synchronous 
- executes tasks sequentially, blocks main thread 
- slower for long task, may cause delay or freeze the application 

#### asynchronous
- allows task run in the background without blocking main thread 
- essentials for I/O operations like fetching data, handles users input, events, timers

# Local Storage, Session Storage, Cookies

#### Local Storage 
- Persists until js erase or user delete 
- ~5MB to ~10MB per domain 
- Client side only 
- accessible for all tabs and windows of the same origin 

```js
localStorage.setItem("myLocal", "test")
console.log(localStorage.getItem("myLocal")); // "test"
```

#### Session Storage 
- data cleared when page/tab closed
- ~5MB to ~10MB per domain 
- Client side only 
- isolated to specific tab/widnow where it was created 

```js
sessionStorage.setItem("myLocal", "test")
console.log(sessionStorage.getItem("myLocal")); // "test"
```

#### Cookies
- expiration is set manually (session based or persistent with an `Expires` attribute)
- ~4MB per cookie/domain
- Both client side / server side 
- sent automatically with every HTTP request 
- when using `HttpOnly` flag can be more secure for storing auth tokens as they prevent client side js access
```js
document.cookie = "myCookie=test; max-age=604800"; // 7 days in seconds
```

# Symbol
- primitive data type used to create completly unique and immutable identifiers
- Non-enumerable - properties keys defined as Symbols do not show up in standard loops like `for...in` or methods like Object.keys()

```js
// Creating two symbols with the same description
const sym1 = Symbol("id");
const sym2 = Symbol("id");

console.log(sym1 === sym2); // false (they are unique)
```

```js
const myID = Symbol("id");

const user = {
  name: "test",
  [myID]: 12345 // Using a symbol as a key
};

console.log(user[myID]); // 12345
console.log(Object.keys(user)); // ["name"] (the symbol key is hidden)
```

# Generator
- special type of function that can be paused and resumed at any time during execution 
- unlike regular functions that run from start to finish in one go, a generator can "yield" a value, wait for you to ask for the next, then pick up where it left off

```js
// 1. Define the generator function with '*'
function* simpleGenerator() {
  console.log("Step 1 started");
  yield "First Value"; // Pauses here

  console.log("Step 2 started");
  yield "Second Value"; // Pauses here

  console.log("Step 3 started");
  yield "Third Value"; // Pauses here
}

// 2. Initialize the generator (returns an object, doesn't run code yet)
const gen = simpleGenerator();

// 3. Use .next() to move to each 'yield'
console.log(gen.next()); // { value: "First Value", done: false }
console.log(gen.next()); // { value: "Second Value", done: false }
console.log(gen.next()); // { value: "Third Value", done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

