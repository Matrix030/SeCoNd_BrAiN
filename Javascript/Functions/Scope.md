[Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) in JavaScript defines where variables and functions are accessible in your code, and it can behave differently depending on the environment (such as a browser or Node.js). There are four levels, from highest to lowest:

1. **Global Scope**:
    - Variables declared globally have the highest level of scope and can be accessed from anywhere in your code.
    - In browsers, global variables are properties of the `window` object. For example, `window.myGlobalVar = 'hello world'` defines a global variable.
    - In Node.js, global variables are properties of the `global` object: `global.myGlobalVar = 'hello world'`.
2. **Module Scope**:
    - In ES modules (both in Node.js and modern browsers), variables declared at the top level of a module are scoped to that module. They are not added to the global scope.
    - In the browser, using `<script type="module">` creates a module scope for that script.
3. **Function Scope**:
    - Variables declared with `var` (we try to avoid this) are limited to the function scope. They are accessible only within that function and any nested functions.
4. **Block Scope**:
    - ES6 introduced block scope with the `let` and `const` keywords. A block is typically defined by curly braces `{}`, like in `if` statements, loops, and other blocks of code.
    - Variables declared with `let` and `const` are confined to their block, making them more predictable and reducing the chances of accidental variable hoisting.