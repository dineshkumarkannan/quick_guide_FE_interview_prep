# Hoisting 
- It is a JS mechanism where declarations of variables, functions, and classes are conceptually moved to top of the current scope during the "compilation phase" befor the code "execution phase". 
- JS engine process two main phases 
- Compilation/Memory creation phase: The engine scans the code for all declaration and register them in memory 
- Execution phase: the code runs line by line, and the assignments are processed
- The key insight is that only declaration is hoisted, not the assignment
- `var` Hoisted to top of the function or to the global scope. Initialized with `undefined`. Access before the decalaration, and it will return `undefined`
- `let`/`const` Hoisted to the surrounding block scope. Not initialized. Can not be accessed before declaration, throws `ReferenceError` because they are in the TDZ (Temporal Dead Zone)

``` js
console.log(myVar); // Output: undefined
var myVar = 5;
console.log(myVar); // Output: 5

console.log(myLet); // Output: ReferenceError: Cannot access 'myLet' before initialization
let myLet = 10;

myFunction(); // Output: "Hello World"
function myFunction() {
  console.log("Hello World");
}

sayHello(); // Output: TypeError: sayHello is not a function
var sayHello = function() {
  console.log("Hello!");
};
```


# Closure 
- It is a combination of a function that bundled together with references to its surrounding state (its lexical environment), which grants it access to its outer scope even after the outer function has finished executing.

```js
function createCounter() {
  let count = 0; // The private variable
  return {
    increment: function() { // An inner function (closure)
      count++;
      return count;
    },
    getCount: function() { // Another inner function (closure)
      return count;
    }
  };
}

const counter1 = createCounter();
console.log(counter1.increment()); // Output: 1
console.log(counter1.increment()); // Output: 2

const counter2 = createCounter();
console.log(counter2.increment()); // Output: 1 (separate, independent state)
console.log(counter1.getCount());  // Output: 2 (state is preserved)

// console.log(count); // ReferenceError: count is not defined (data privacy achieved)
```

Practical Use Cases :
* Data Privacy/Encapsulation - Module pattern 
* Function Factories 
* Event Handlers and Callbacks 
* Curring and Partial Application
* Memoization


# var, let, const 

|   Feature  |	var |   let |   const   |
|---|---|---|---|
|   Scope   |	Function or Global scope    |	Block scope |	Block scope |
|   Hoisting    |	Hoisted and initialized with `undefined`  |	Hoisted but not initialized (TDZ applies)   |	Hoisted but not initialized (TDZ applies)   |
|   Reassignment    |	**Yes**, can be updated |	**Yes**, can be updated |	**No**, cannot be updated   |
|   Redeclaration   |	**Yes**, can be redeclared in the same scope    |	**No**, cannot be redeclared in the same scope  |	**No**, cannot be redeclared in the same scope  |
|   Initialization  |	Optional at declaration |	Optional at declaration |	Required at declaration |


# Event delegation 

- Event delegation is a technique where single event listener attached to common parent element 
- It will avoid individual listener on multiple child elements
- It leverages event bubbling 

```js
const parent = document.getElementById('parent');
parent.addEventListener('click', (event) => {
    if (event.target.tagName === 'LI') {
        console.log('Clicked', event.target.textContent);
    }
});
```

- Key advantages : Performance, Dynamic Elements, Maintainability


# == vs ===

- **== (Loose Equality)** performs type coercion
- **=== (Strict Equality)** does not perform type coercion


# Promises 
- Handling asynchronous operations in javascript is a fundamental part of creating responsive and interactive web applications
- A Promise is JS represents the eventual complition(or failure) of an asynchronous operation and its resulting value 
- Three states : **Pending, Fulfilled, Rejected**

```js
const myPromise = new Promise((resolve, reject) => {
  // Asynchronous operation here
  if (/* operation successful */) {
    resolve('Success');
  } else {
    reject('Error');
  }
});

myPromise
  .then((value) => {
    // Handle success
    console.log(value);
  })
  .catch((error) => {
    // Handle error
    console.error(error);
  })
  .finally(() => {
    // Execute cleanup or final operations
    console.log('Completed');
  });
```

### Promise.all()
- When you need to run multiple promises in parallel and wait for all of them to complete.

```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("promise1 success!")
    }, 1000)
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("promise2 success!")
    }, 1000)
});

const promiseAll = Promise.all([promise1, promise2]);

promiseAll.then((res) => {
    console.log(res)
}).catch(err => {
    console.log(err)
}).finally(() => {
    console.log("Completed!")
})

// Output: [ 'promise1 success!', 'promise2 success!' ]
// Completed!
```


### Promise.rase()
- `Promise.rase()` is similar to `Promise.all()`, but it resolves or rejects as soon as one of promises in the itterable resolves or rejects, with the value or reasons from that promise  

```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("promise1 success!")
    }, 1000)
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        reject("promise2 reject!")
    }, 2000)
});

const promiseRace = Promise.race([promise1, promise2]);

promiseRace.then((res) => {
    console.log(res)
}).catch(err => {
    console.log(err)
}).finally(() => {
    console.log("Completed!")
})

// Output: promise1 success!
// Completed!

// promise2 reject, however promise1 resolve's before promise2
// hence, promise1 fullfied and retured
```

### Promise.allSettled()

- This method returns a promise that resolves after all of the given promises either resolved or rejected

```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("promise1 success!")
    }, 1000)
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        reject("promise2 reject!")
    }, 2000)
});

const promiseAllSettled = Promise.allSettled([promise1, promise2]);

promiseAllSettled.then((res) => {
    console.log(res)
}).catch(err => {
    console.log(err)
}).finally(() => {
    console.log("Completed!")
});

// Output: [
//  { status: 'fulfilled', value: 'promise1 success!' },
//  { status: 'rejected', reason: 'promise2 reject!' }
// ]
// Completed!
```

### Promise.any()
- This taked iterable promise objects and as soon as one of the promises in the iterable fulfills, return a single promise that resolves with the value from that promise.

```js
const promise1 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        resolve("promise1 success!")
    }, 1000)
});

const promise2 = new Promise((resolve, reject) => {
    setTimeout(()=> {
        reject("promise2 reject!")
    }, 2000)
});

const promiseAny = Promise.any([promise1, promise2]);

promiseAny.then((res) => {
    console.log(res)
}).catch(err => {
    console.log(err)
}).finally(() => {
    console.log("Completed!")
})

// Output: promise1 success!
// Completed!
```
