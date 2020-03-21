---
layout: post
title:      "JavaScript Project Challenge: Scope/Scope Chain & Hoisting"
date:       2020-03-11 21:20:38 -0400
permalink:  rails_back-end_and_and_javascript_front-end_mybudget
---


Javascript month has been the most difficult month of the program so far. I needed more than a few reviews of the curriculum to get a little bit of a handle on it. Knowing Ruby made it helpful as well to learn this language. I knew what things were going to achieve in the lessons using Javascript instead. 

But there were a few sections I had a bit of a tough time remembering: scope and hoisting.

***What is Scope?***

**Scope is defined where declared variables and methods are available within our code.

The Google definition of scope is: 
*"In computer programming, the scope of a name binding—an association of a name to an entity, such as a variable—is the region of a computer program where the binding is valid: where the name can be used to refer to the entity."*


## There are three types of scope: Global Scope, Function Scope and Block Scope.

***Global Scope:***

Variables and functions declared in the global scope are accessible everywhere. If a variable or function is not declared inside a function or block,*it's in the global scope.* In a newly made JavaScript file, you are in the global scope where nothing is defined yet. Variable or function declarations in this scope are accessible everywhere.

***Function Scope:***

When we write a new function, that function creates a new scope. You are no longer in the global scope but inside the function scope. The Global scope cannot access the variables or functions declared inside of that function. If you try to access a variable defined inside of a function from the global scope, you will reciever an error telling you that such variable is not defined.

```
function blogTest(){
let blogContent = "The Global Scope cannot access this variable.";
return blogContent;
}

blogContent;
//=>Uncaught ReferenceError: blogContent is not defined
```

***Block Scope:***

In the example above, we used curly brackets '{}'. If statements and loops use them as well. A block statement creates its own scope. But ES2015 only has partial support for block scoping. What does this mean? Variables declared with var are ***not*** block scoped. Variables declared with let and const ***are*** block scoped. That means even though the block created its own scope, if you declare a var inside of that scope, the var is still accessible outside of that block. ES2015's partial support for block scope includes let and const. Variables declared with let and const inside of a block statement are not accessible outside of that block.

```
if (true){
var name = "Ethan";
}

name;
//=> "Ethan"

if (true){
let name = "Ethan";

const lastName = "Gustafson";
}

name;
//=> Uncaught ReferenceError: name is not defined

lastName;
//=> Uncaught ReferenceError: lastName is not defined
```

***Scope Chain:***

Although the outer(global) scope cannot access function or block scope, function scope has access to the global scope. Every function in JavaScript has access to a scope chain, which contains references to that function's outer scope (the scope in which the function was declared), the outer scope's outer scope, and so on.

```
let myVariable = "Value";

function returnMyVariable(){
return myVariable;
}

returnMyVariable();
//=> "Value"

```

If we put a block statement inside a function, that block statement has its own scope and the function does not have access to that blocks data.

```
function blockStatement(){

if (true){
let secondValue = "Second value";
}
return secondValue;
}

blockStatement();
//=> Uncaught ReferenceError: secondValue is not defined
```

But the block has access to the functions data.

```
function blockStatement(){
let value = "First Value";

if (true){
return value
}

}

blockStatement();
//=>"First Value"
```

Function scope only goes one way. It would be like walking up a staircase. If Javascript cannot find a variable or function reference located in the current scope, it moves upwards through the outer scope to find that variable or function. In the example below, the inner function second has access to the variable `value`. If we invoke first(), what will the result be?

```
function first(){
    const value = "First";
    function second(){
        const secondValue = "second";
        return value + " and " + secondValue;
    }

    const firstSecondResult = second();
    return firstSecondResult;
}
first();
//=>"First and second"
```

We also have to invoke that inner function and/or save it to a variable. Otherwise the function first will return to us `undefined` if nothing is returned. If we say `return second` at the end of the `first()` function, it will return us that function, but not its values. To show a better example of a scope chain, we can write:

```
const myValue = "is connected."

function first(){
    const value = "First";
		
    function second(){
        const secondValue = "second";
				
        if (true){
           return value + " and " + secondValue + myValue;
        }
    }

    return second()
}

first()
//=>"First and second"
```

The block statement has access to the `second()` functions scope, the `first()` functions scope and the global scope. This is what is called a scope chain because JavaScript chains scopes together in this way.

***var, let and const:***

A variable declared with `var` allows you to redeclare a variable with the same name more than once and it allows you to reassign a value to to that variable more than once.

```
function varVariables(){
var value = "First Value";
var value = "Second value";
return value;
}

varVariables()
//=> "Second value"
```

A variable declared with `let` will not let you declare another `let` variable with the same name again. But like `var`, you can reassign a value to a `let` variable.

```
function letVariables(){
let value = "First Value";
let value = "Second Value";
return value
}

//=> Uncaught SyntaxError: Identifier 'value' has already been declared

function letVariables(){
let value;

value = "First Value";
value = "Second Value";
return value;
}

letVariables()
//=> "Second Value"
```

A variable declared with `const` cannot be redeclared with the same name nor can it be reassigned. A `const` variable will always give you the same piece of data every time.

```
function constVariables(){
const value = "Value";
const value = "Second Value";
return value;
}

//=> Uncaught SyntaxError: Identifier 'value' has already been declared
```

However, if the `const` variable holds an object, that objects values can change.

```
const myObject = {}
myObject.value = "I was added to a const variable."

myObject;
//=>{value: "I was added to a const variable."}
```

***Hoisting:***

Hoisting is where variable and function declarations get raised to the top of the current scope. The JavaScript Engine *reads a JavaScript file* line-by-line from top-to-bottom. Would it make sense if we called a function before it has been declared?

```
iDeclare();

function iDeclare(){
return "I have been declared";
}

//=>"I have been declared"
```

What is happening? Even though the function was called before it was declared, we were still able to call the function and it returned to us: `"I have been declared"`

The JavaScript Engine has a two phrase process: Compilation and Execution. When your code is run, the engine makes two passes over your code. The first time is the "Compilation phase" where your code is read line-by-line. When it comes across a variable or function declaration, the engine allocates memory and sets up a reference to the variable's/function's identifier(name of variable/function). The variable/function isn't assigned a value as of yet, and the invokations are skipped over because they are not declarations.

Now your code will be executed line-by-line during the second process called "The Execution Phase."
During the execution phase, values are assigned to variables and functions. When the engine reaches the declarations, it sees the identifier name and assigns it a value through a process called "identifier resolution." The engine checks if that declaration has been declared in the current scope and if it doesn't find it it moves upward throughout the outer scopes until it finds a match. If it doesn't find a match it will throw you a ReferenceError and it will let you know that the identifier hasn't been declared anywhere in the scope chain.

***Does var get hoisted?***
Variables declared with `var` are hoisted, but only their declaration will be hoisted. You can declare a `var` variable but hoisting does not apply if you declare and assign `var` a value at the same time. Variables declared with `var` are initialized with a value of `undefined`.

```
function varVariable(){
console.log(myVar);

var myVar = "Value";
}

varVariable();
//=> undefined
```

***Does let and const really get hoisted?***

Technically, yes. `let` and `const` do get hoisted, but JavaScript won't let you reference them before they have been initialized. Initialization is the means of assigning an initial value to a variable. The variable `var` is initialized with `undefined`, while `let` and `const` are not initialized.

Will `let` and `const` be hoisted?

```
function variableTester(){
console.log(variableConst);
console.log(variableLet);

const variableConst = "Have I been hoisted?";
let variableLet = "Have I also been hoisted?";
}
```

The result when `variableTester()` is invoked is this:

```
variableTester();
//=>Uncaught ReferenceError: Cannot access 'variableConst' before initialization
```

Although `let` and `const` *will* get hoisted, they ***cannot*** be referenced because they were not initialized. 

***Lexical Scoping***

Lexical Scoping is scope based on where variables and blocks of scope are defined by the time it was written. To show an example:

```
const correctScopeVariable = 'First Value';
 
function first () {
  console.log('correctScopeVariable is currently equal to:', correctScopeVariable);
}
 
function second () {
  const correctScopeVariable = 'Second Value';
 
  first();
}

first();
//=>correctScopeVariable is currently equal to: First Value
```

`first()` was defined in the global scope. Its parent scope(outer scope) is the global scope. Even though we invoked the `first()` function inside of `second()`, that doesn't make the `first()` functions parent scope the `second()` functions scope.

Since JavaScript reads a file from top-to-bottom and line-by-line, it created the scopes for the declarations it came across first. Then it adds a reference to those declarations parent scopes. Since the JS Engine came across `correctScopeVariable` and `first()` first in the compilation phase, `first()`'s parent scope is set to the global scope.


***Example of hoisting a function and a function expression***

```
function firstFunction(){
console.log(secondFunction());

function secondFunction(){
    return "I am hoisted.";
}

}
//=>I am hoisted.
```

Functions are hoisted but function expressions are not hoisted.

```
fn();

let fn = function(){
return "I am not hoisted.";
}

//=>Uncaught ReferenceError: Cannot access 'fn' before initialization
```

This is because `fn` is a variable. Remember, variables declared with `let` or `const` cannot be referenced before they have been initialized.


