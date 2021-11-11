---
title: "Javascript: Context vs Scope"
tags: ['JavaScript']
date: 2020-11-13T18:19:09.222000
---

![Photo by [Irvan Smith](https://unsplash.com/@mr_vero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/8192/0*hcBr_-BUKv8qjPA1)*Photo by [Irvan Smith](https://unsplash.com/@mr_vero?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

In the next posts, I’ll talk about the **closures** and **lexical environment** . To be easier to understand both. I’ll give more knowledge about context/scope and the difference between them.


<!--more-->

## Context

It’s an object reference in the current code running, or it’s the environment in which the JavaScript code is executed. Says, what is the value of the **this** keyword.

When we run a code in Javascript, it is always going to be part of an execution context. This execution context can be part of the global execution context or function execution context.

JavaScript engine creates the execution context in the following two stages: **creation** and **execution** .

### Creation Stage

It is the phase that the engine compiles the code and scan the function code to compile it. It doesn’t execute any code.

### Execution Stage

It is the phase that the engines will again scan the functions to update the variable object with the values and will execute the code.

## Execution Context

Represents the *environment* in which JavaScript runs. It has two types: **Global** **Execution** **Context** and **Function** **Execution** **Context.**

### Global execution context

The first step that the Javascript engine does for us it creates a **global execution context** and gives it to us. We can access the global context regardless is on the browser or NodeJs.

* Browser: you can access the **window** variable.

* NodeJs: you can access the **global** variable.

### Function execution context

When we run a function, it creates a new execution context. You can access the context information inside the function.

If you execute three functions, you’ll have one execution context for each function.

Let’s supposed that we have two functions: *FunctionA* and *FunctionB* . We execute the *FunctionA* and after we execute the *FunctionB* . We’ll have this in the call stack:

 **Global** **execution** **context** → **FunctionA** **execution** **context** → **FunctionB** **execution** **context**

## Scope

Scope means which variables and expressions are enabled to use when the function is called. It is the current context of the execution.

There are three types of scope in JavaScript: **Global** **Scope** , **Function** **Scope,** and **Block** **Scope** .

### Global Scope

Any variable declared outside a function or block is inside the global scope. These variables can be accessed from anywhere in the program.

```
var name = "Leidson";
```

```
function hello() {
  console.log(`Hello ${name}`);
}
```

```
hello(); // Prints 'Hello Leidson';
```

### Function Scope / Local Scope

Any variable declared inside a function. That means is it only accessible inside its function(scope).

```
function hello() {
  var lastName = "Cruz";
  console.log(`Hello ${lastName}`);
}
```

```
hello(); // Prints 'Hello Cruz';
```

```
console.log(lastName); //Uncaught ReferenceError: lastName is not defined
```

### Block Scope

Until ES6, JavaScript only had function scope. ES6 introduced **let** and **const** keywords. You can use these new keywords between a pair of curly braces, such if, switch, for, while, and it’ll be block scoped.

```
function hello() {
 if(true){
   let fullName = "Leidson Cruz";
   var age = 31;
    
   console.log(fullName); // Prints 'Leidson Cruz'
  }
  
  console.log(age); // Prints '31'
  
  console.log(fullname); // Uncaught ReferenceError: fullname is not defined
}
```

```
hello();
```

 *“Ok, so scope has the own variable and expressions. But I can access a variable from the parent. How is it possible?”.* 

It’s possible because of the **Scope chain** .

### Scope Chain

Every time that a variable is used, the Javascript engine will try to find the variable in the current scope. If it could not find the variable, it will look into the parent scope and will continue to do, until it finds the variable or reaches the global scope.

For this reason, you can use variables from the parent scope.

```
function sayHello(){
 var name = "Leidson";
 let lastName = "Cruz"; 
```

```
 return function createFullName(){
   const fullName = `${name} ${lastName}`;
    
   return function printHello(){
     console.log(fullName);
   }
 }
}
```

```
sayHello()()(); //Prints 'Leidson Cruz';
```

 *Can you understand why we can use variables blocked scopes in the children’s functions? Answer in the comments.* 

## Conclusion

A short explanation of the difference between context and scope is:

*  **Context** is the value of **this** keyword, the property of **execution** **context.**

*  **Scope** is the visibility of variables and expressions in the current execution context.

The original post is https://medium.com/@leidson/javascript-context-vs-scope-1a4b5fa0a513