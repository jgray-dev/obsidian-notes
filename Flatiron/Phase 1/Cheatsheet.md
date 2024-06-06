// VARIABLES
```
const x = 10; // Declaring a constant variable (immutable)
let y = 20; // Declaring a variable (mutable, can be changed)
console.log(x); // Output: 10
console.log(y); // Output: 20
```

// DICTIONARIES
```
const exampleDictionary = { 
			a: 1,
			b: 2, 
			c: 3 }; // Creating a dictionary with key-value pairs (object)
console.log(exampleDictionary.a); // Accessing a dictionary value using dot notation
console.log(exampleDictionary['b']); // Accessing a dictionary value using bracket notation
```

// FUNCTIONS
// Normal Function - Reusable code
```
function yippee(var1, var2) {
    // A function that adds two variables passed as arguments
    return var1 + var2;
}
```

// Arrow Function - Compact syntax
```
let arrowFunctionReturn = () => "This is a simple arrow function"; // Example of an arrow function
console.log(arrowFunctionReturn()); // Output: This is a simple arrow function
```

// Callback Function - Pass function as an argument
```
function callback(newFunction) {
    console.log(newFunction); // Outputs the passed function
}
callback(yippee); // Passing the 'yippee' function as an argument
```

// ARRAY METHODS
```
let arr = [1, 2, 3, 4];
arr.push(5); // Adding a value to the end of the array
arr.pop(); // Removing the last value from the array
arr.shift(); // Removing the first value from the array
arr.unshift(0); // Adding a value to the beginning of the array
```

// OBJECT METHODS
```
const obj = { firstIndex: "first-value", secondIndex: "second-value" }; // Creating an object with key-value pairs
console.log(obj.firstIndex); // Accessing an object value using dot notation
```

// LOOPS
// For Loop and Index Loop
```
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // Looping through an array using an index
}
for (const value of arr) {
    console.log(value); // Looping through an array using the 'for...of' loop
}
```

// TERNARY OPERATOR
// Conditional (Ternary)
```
let condition = true;
let result = condition ? "exprIfTrue" : "exprIfFalse"; // Using the ternary operator for a simple condition
```

// DOM MANIPULATION
// Selecting Elements
```
const header = document.querySelector('h1'); // Selecting the first 'h1' element in the document
header.textContent = "Changing the H1 from the DOM"; // Changing the text content of the selected element
```

// EVENT LISTENERS
```
const button = document.querySelector('button'); // Selecting a button element
button.addEventListener('click', normalFunction); // Adding a click event listener with a normal function
```

```
function normalFunction(heehee) {
    console.log("heehee was called " + heehee); // Output: heehee was called [MouseEvent]
}
```

```
button.addEventListener('click', () => {}); // Adding a click event listener with an arrow function
```

// FORM HANDLING
// Form Event Listener
```
const formButton = document.querySelector('button'); // Selecting a button element within a form
formButton.addEventListener('click', (e) => {
    console.log(e.target.FORMNAME.value); // Accessing form data on button click
});
```

