# Event loop 
- To perform non-blocking operations and asynchronous operations efficiently
- The event loop operates within runtime environment using several key components
- Callstack - Executes synchronous code in LIFO manner 
- Web Api/libuv - Handles asynchrous tasks like timers and networking
- Micro Task queue - Processes high-priority tasks(Promises) completely before next step
- Macro Task queue - Manages I/O operations, timers. Processing one per cycle
- Event Loop - Continiously checks if the call stack is empty to process queued task

```js
console.log('Start');
setTimeout(() => console.log('Timeout'), 0);
Promise.resolve().then(() => console.log('Promise'));
console.log('End');

// Start
// End
// Promise
// Timeout

```

# map(), filter(), reduce(), forEach()

map()

- Transform each element in an array
- retrun new array (same length)
- chainable 

```js
const arr = [1, 2, 3];
const newArr = arr.map(val => val * 2);
console.log(newArr);

// [ 2, 4, 6 ]
```

filter()

- Select specific elements 
- return new array (only passed element from the test)
- chainable 

```js
const arr = [1, 2, 3];
const newArr = arr.filter(val => val % 2 === 0);
console.log(newArr);

// [ 2 ]
```

reduce()

- Accumulate an array into a single value
- Not chainable 

```js
array.forEach((element, index, originalArray) => {
  // code to execute for each element
});
```

# 'this'

- `this` keyword in javascript is a dynamic reference to the object that is currently executing the code, and its value changes depending on the context in which a function is called. 
- **Global Context** - used in global scope(outside of any function), `this` refers to the global object.
- **Function Context** - Simple invocation function, non-strict mode `this` refer to global object. In strict mode "undefined". As an object method `this` refers to own object itself.
- **Contructor Context** - `this` refers to newly created instance of the object
- **Event handlers** - `this` refers to the element on which the element listeners was attached 
- **Arrow Function** - do not bind their own `this`, instead it will refer to surrounding lexical context  

# call(), apply(), bind()

- Can control the value of `this` using specific methods 

- call()

```js
let obj = {
    name: "obj",
    add: (a, b) => {
        return a + b
    }
}

function attachFun(a, b){
    return this.add(a, b);
}

console.log(attachFun.call(obj, 10, 20));

// 30

```

- apply()

```js
let obj = {
    name: "obj",
    add: (a, b) => {
        return a + b
    }
}

function attachFun(a, b){
    return this.add(a, b);
}

console.log(attachFun.apply(obj, [10, 20]));

// 30

```

- bind()

```js
let obj = {
    name: "obj",
    add: (a, b) => {
        return a + b
    }
}

function attachFun(a, b){
    return this.add(a, b);
}

const bindRes = attachFun.bind(obj);

console.log(bindRes(10, 20));

// 30

```

# modules 

- allow you to break code into separate files for better organization
- reusability and to avoid global namespace pollution 
- use `export` and `import` keywords to share functionality 

# arrow `() => {}` function 

- concise way of writing anonymous function expression 
- implicit return 
- lexical `this` binding

```js
const add = (a, b) => a + b;
```

# debounce and throttle

- debounce : only triggers a function after the last event with specific time 

```js
// html
// <input type="text" id="search" />
//

const searchElm = document.getElementById('search');

function debounce(fn, ms) {   
    let timerId;    
    return (...args) => {
        if (timerId) {       
            clearTimeout(timerId);     
        }    

        timerId = setTimeout(() => {       
            fn(...args);     
        }, ms);   
    } 
}

function handleChange(e) {
  console.log(e.target.value)
}

searchElm.addEventListener("input", debounce(handleChange, 1000))

```

- throttle : ensures no matter how frequently the event is triggered, the actual event handling function will be executed once within a specified time 

```js
function throttle(fn, ms) { 
    let flag = false;   
    return (...args) => {   
        if (flag) return;    
        flag = true;     
        setTimeout(() => {  
            fn(...args)      
            flag = false;  
        }, ms);   
    } 
}

const handleScrollTop = throttle(() => {  
    console.log('scrollTop: ', document.body.scrollTop);  
        // todo: 
}, 500); 
window.addEventListener('scroll', handleScrollTop);

```

```js
var throttle = function(func, delay) {            
　　var prev = Date.now();            
　　return function() {                
　　　　var context = this;                
　　　　var args = arguments;                
　　　　var now = Date.now();                
　　　　if (now - prev >= delay) {                    
　　　　　　func.apply(context, args);                    
　　　　　　prev = Date.now();                
　　　　}            
　　}        
}        
function handle() {            
　　console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));         
```

# higher-order function (HoF)
- takes one or more functions as arguments 
- returns a new function 

```js
// A simple callback function
function sayHello() {
  console.log("Hello!");
}

// A higher-order function that takes a callback as an argument
function greet(callback) {
  console.log("I'm about to call the callback...");
  callback(); // Invokes the function passed in
}

// Pass `sayHello` to `greet`
greet(sayHello); 
// Output: 
// I'm about to call the callback...
// Hello!

```

# immutability in javascript

- once a value or object created it can't be changed
- primitive value cannot be changed after creation 
- Objects and Arrays mutable by default 


#### Achieve immutability with Objects and Arrays
- spread operator

```js
const originalArray = [1, 2, 3];
const newArray = [...originalArray, 4]; 
// originalArray is [1, 2, 3]; newArray is [1, 2, 3, 4].

const originalObject = { a: 1, b: 2 };
const newObject = { ...originalObject, b: 3 }; 
// originalObject is { a: 1, b: 2 }; newObject is { a: 1, b: 3 }.

```
- immutable array methods -- .map(), .filter(), .slice() and .concat()

- Object.freeze()  : cannot add, delete, modify the properties, but can manupulate the nested object 

```js
let obj = {
    test: "test"
}

Object.freeze(obj);

obj.add = "test";
obj,test = "updated";

delete obj.test;

console.log(obj)

// Output: { test: 'test' }

```

- for complex/deep structures - library immer or immutable   

# handling errors

- **SyntaxError** occurs when code violates the language grammar rules.
```js
function test ()) {
    console.log("test")
}

test()

// SyntaxError: Unexpected token ')'
```

- **ReferenceError** throws when variable or function is accessed but has not been declared in the current scope

```js
console.log(test)

let test;

// ReferenceError: Cannot access 'test' before initialization
```

- **TypeError** occurs when operations performed on inappropirate or unexpected type
```js
let val = 10;

console.log(val.toLowerCase());

// TypeError: val.toLowerCase is not a function
```

- **RangeError** Arises when a numeric value is outside the range of allowed values for a function's arguments

- **URIError** thrown when using global URL handling functions with malformed parameters

- **AggregateError** handles multiple errors at once, often related to promises 