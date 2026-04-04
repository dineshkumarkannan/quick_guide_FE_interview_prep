# memoization 
- optimization techniques that catches the result of expensive funciton 
- works best with pure functions


```js
function memoize(fn) {
  const cache = new Map(); // Closure-based storage [2, 12]
  return function(...args) {
    const key = JSON.stringify(args); // Generate unique key [13]
    console.log(`cached`, cache);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
}

const add = (a, b) => {
    return a + b;
}

const memofun = memoize(add);

console.log(memofun(2, 3));
console.log(memofun(2, 3));
```

- drawbacks : increases memory usage, can lead to performance issue 

# Currying 

- technique where function with multiple aruguments is transformed into a sequence of functions, each taking single argument
- currying relies two fundamental concepts : Closures and First-Class functions

- Partial application and reusability 
- Cleaner code
- Avoiding repetitive arguments

```js
const curriedMultiply = a => b => c => a * b * c;
// 1. Call with 'a'
const multiplyByTwo = curriedMultiply(2); 
// 'multiplyByTwo' is now: b => c => 2 * b * c
// It remembers 'a' is 2.

// 2. Call with 'b'
const multiplyByTwoAndThree = multiplyByTwo(3);
// 'multiplyByTwoAndThree' is now: c => 2 * 3 * c
// It remembers 'a' is 2 and 'b' is 3.

// 3. Final call
const result = multiplyByTwoAndThree(4); // 2 * 3 * 4 = 24
```

```js
// A curried logger function
const logger = level => component => message => 
  `[${level}] ${component}: ${message}`;

// Create a specialized error logger
const errorLogger = logger("ERROR");
const apiErrorLogger = errorLogger("API_SERVICE");

// Use it later
console.log(apiErrorLogger("Connection Failed"));
// "[ERROR] API_SERVICE: Connection Failed"

console.log(apiErrorLogger("Timeout"));
// "[ERROR] API_SERVICE: Timeout"
```

# for...of for...in

- for...in - best used for objects
- includes inherited enumerable properties 

```js
const user = { name: 'Alice', age: 25 };
for (let key in user) {
  console.log(key); // Output: "name", "age"
}
```

- for...of - best used for arrays, sting, maps, sets
- ignores inherited properties

```js
const colors = ['red', 'green', 'blue'];
for (let color of colors) {
  console.log(color); // Output: "red", "green", "blue"
}
```

# Garbage collection 
- reclaims memory from object that are no longer accessible 
- Mark-and-sweep algorithm - 1) The collector scans from root to every object graph marking "alive". 2) then collector scans heap and identies what are all not marked as "alive". 3) Finally, memory reclaimed 
- This algorithm successfully handles circular references as well, where two objects reference each other but are diconnected from root

- Preventing memory leaks 
  - Accidental Globals 
  - Forgotten timers/listeners
  - Closures
  - Detached DOM Nodes

# Callback function 
- a function passed as a argument to another function, which is then invoked inside the outer function to complete a specific task
- Synchronous, Asynchronous callbacks 
- It's a Non-blocking execution

# Dynamic import 
- load modules asynchronously and on-demand at runtime 
- Unlike "static" import, dynamic imports use the `import` anywhere in the code such as inside functions or conditional blocks 

```js
// Using then()
import('./math.js')
  .then(module => {
    console.log(module.add(2, 3));
  });

// Using async/await
async function loadModule() {
  const { add } = await import('./math.js');
  console.log(add(5, 10));
}
```

- Handling Exports : Named Export, default export 

# Fetch api
- promise based interface for making HTTP requestes
- replacing the older `XMLHttpRequest`

```js
async function fetchData(url) {
  try {
    // 1. Initiate the fetch request
    const response = await fetch(url);

    // 2. Check if the response was successful (status 200-299)
    // Fetch only rejects on network failure, not on 404 or 500 errors.
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }

    // 3. Parse the JSON body
    const data = await response.json();
    
    console.log("Success:", data);
    return data;
  } catch (error) {
    // 4. Catch and handle network errors or thrown status errors
    console.error("Fetch operation failed:", error.message);
  }
}

// Example usage with a public API
fetchData('https://jsonplaceholder.typicode.com/posts/1');
```

### standard api error codes
- **4XX** : Client Error Codes 
  - 400 Bad Request 
  - 401 Unauthorized
  - 403 Forbidden error
  - 404 Not Found
  - 405 Method Not Allowed 
  - 409 Conflic
  - 422 Unprocessable Entity
  - 429 Too Many Requests

- **5xx** : Server Error Codes
  - 500 Internal Server Error
  - 502 Bad Gateway
  - 503 Service Unavailable
  - 504 Gateway Timeout

- **2xx**, **3xx** : Success Error Codes
  - 200 Ok. Standard Success
  - 201 Created
  - 204 No Content 
  - 301 Moved Permanently : The requested resourse has permanently moved to a new URL
  - 304 Not Modified : Indicates the catched version of the resource is still valid
  
# array.slice(), array.splice()

### array.slice()
- creates a shallow copy without modifying original data 

```js
const arr1 = [ 1, 2, 3];
const copiedArr = arr1.slice();
copiedArr.push(4);
arr1.push(10);
console.log(arr1);
console.log(copiedArr);
```

### array.splice()
- modifies the original array. adding, removing, or replacing elements

```js
const arr1 = [ 1, 2, 3, 4, 5];

arr1.splice(1, 1); // removed arr1[1] position one element from original array

console.log(arr1); // [ 1, 3, 4, 5 ]

arr1.splice(2, 0, 555); // added new element to 2nd position 

console.log(arr1); // [ 1, 3, 555, 4, 5 ]

arr1.splice(2, 1, 666); // replaced position 1 element value with new value 

console.log(arr1); // [ 1, 3, 666, 4, 5 ]
```

# tagged templates 
- an advanced form of js template literals, that allow you to parse or manipulate string literals using custom functions known as tag.

```js
function highlight(strings, ...values) {
  return strings.reduce((prev, curr, i) => {
    return `${prev}${curr}${values[i] ? `<span class="hl">${values[i]}</span>` : ''}`;
  }, '');
}

const name = 'Alice';
const role = 'Developer';
const sentence = highlight`${name} is a ${role}.`;
// Result: "<span class="hl">Alice</span> is a <span class="hl">Developer</span>."

```

# event bubbling, event capturing 

- two phases of javascript event propagation 
- **bubbling**(default) : propagate events from the target element upward through parents
- **capturing** : moves from the root down to the target
- stop an event from propagation in ether phase using the `event.stopPropagation()`

# ES5, ES6

| Feature | 	ES5 (Old) |	ES6 (Modern/ES2015) |
|-------|------|-----|
**Variable Declaration**  |	var (function scope)  |	let, const (block scope)
**Functions** |	function()  | keyword	Arrow functions () => {}
**String Handling** |	Concatenation (+) |	Template Literals (`)
**Modules** |	require / exports |	import / export
**Object Oriented** |	Prototypes |	Classes (class, constructor)
**Asynchronous** |	Callbacks	| Promises, Async/Await
**Parameters** |	arguments object |	Default, Rest, Spread operators

# Function.prototype
- used to implement prototypical inheritance, allowing all the instances create with the constructor function to share the same methods and properties without duplicating them in memory 

```js
// 1. Define a constructor function
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

// 2. Add a method to the prototype
// This method is now shared by ALL Person instances
Person.prototype.getFullName = function() {
    return `${this.firstName} ${this.lastName}`;
};

// 3. Create instances
const user1 = new Person("Jane", "Doe");
const user2 = new Person("John", "Smith");

// 4. Access the shared method
console.log(user1.getFullName()); // Output: Jane Doe
console.log(user2.getFullName()); // Output: John Smith

// 5. Verification
console.log(user1.getFullName === user2.getFullName); // true (they share the same function reference)
```

```js
Array.prototype.last = function() {
    return this[this.length - 1];
};
const nums = [1, 2, 3];
console.log(nums.last()); // 3
```

