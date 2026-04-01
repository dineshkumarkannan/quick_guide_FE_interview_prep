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