## Hoisting 

- It is a JS mechanism where declarations of variables, functions, and classes are conceptually moved to top of the current scope during the "compilation phase" befor the code "execution phase". 
- JS engine process two main phases 
- Compilation/Memory creation phase: The engine scans the code for all declaration and register them in memory 
- Execution phase: the code runs line by line, and the assignments are processed
- The key insight is that only declaration is hoisted, not the assignment
- `var` Hoisted to top of the function or to the global scope. Initialized with `undefined`. Access before the decalaration, and it will return `undefined`
- `let`/`const` Hoisted to the surrounding block scope. Not initialized. Can not be accessed before declaration, throws Reference error because they are in the TDZ (Temporal Dead Zone)

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