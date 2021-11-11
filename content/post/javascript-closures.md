---
title: "Javascript: Closures"
tags: ['JavaScript', 'Closures Functions', 'Javascript Tips']
date: 2020-12-01T20:35:53.474000
---

If you don’t know what **Context** or **Scope** is, I recommend you to read a previous [post](https://leidson.com/post/javascript-context-vs-scope/) talking about [Context VS Scope](https://leidson.com/post/javascript-context-vs-scope/).

### What Closure is?


<!--more-->

It is very simple to understand what Closure is. It is functionality to access variables from other Scope, even if the function is pop off from the call stack.

See the example:

```
const closureExample = () => {
 const name = "Leidson";
  
  return () => {
   return `Hi ${name}`;
  }
};
```

```
const sayHi = closureExample();//this line, removes the closureExample from the call stack
```

```
console.log(sayHi()); //But, we still have the name variable in memory and the result of this execution is: Hi Leidson
```

How we already saw in the post about context, every time that a function is executed, it creates an execution context. After the function returns, it pops off from the call stack, and the **garbage collector** cleans variables and references. But, in this case, the garbage collector sees: that’s a closure, I need to keep it in the memory because it’ll use after.

The place name’s of where the variable is kept in memory is **Closure Box** . So, instead of the function goes to the parent scope, it goes to the Closure Box and gets the values.

 **An important note** : Closure always **returns** **a** **function** . It is a combination of Function and Lexical Environment (I’ll talk in the next posts about it).

>Ok, why closures are interesting or powerful?

Imagine a heavy code, for example, a big returned data from API. You need to get a specified index from this data. So, probably you’ll have a code like this:

```
const fetchData = () => {
 console.log("fetch new data!")
 return Array.from({length: 99999}, () => Math.floor(Math.random() * 99999));
}
```

```
const getItem = (index) => {
 const data = fetchData();
  
 return data[index];
}
```

```
console.log(getItem(1)); //"fetch new data!" & number
console.log(getItem(12));//"fetch new data!" & number
console.log(getItem(3)); //"fetch new data!" & number
```

So, every time that you running the `getItem`, the `fetchData` will be running too. Can you imagine how big is it? Now, can you imagine if, instead of fetching data, you’ll have a very heavy operation, how much performance you’ll lose? So, to avoid this, we can use Closures:

```
const fetchData = () => {
 console.log("fetch new data!")
 return Array.from({length: 99999}, () => Math.floor(Math.random() * 99999));
}
```

```
const getItemUsingClosure = () => {
 const data = fetchData();
```

```
 return (index) => data[index];
}
```

```
const getItemClosure = getItemUsingClosure(); //"fetch new data!"

```

```
console.log(getItemClosure(1)); //number
console.log(getItemClosure(12));//number
console.log(getItemClosure(3));//number
```

What does this code do? When we assign the `getItemClosure` in a variable, the data variable goes to the **Closure box** . So, every time that you call the `getItemClosure`, it goes to the Closure box to get the data value instead of needs to call the fetch data again.

We could have a lot of code after the getItemClosure assigned, and the data value would still be kept in memory, ready to be used when we needed it. For example:

```
const fetchData = () => {
 console.log("fetch new data!")
 return Array.from({length: 99999}, () => Math.floor(Math.random() * 99999));
}
```

```
const getItemUsingClosure = () => {
 const data = fetchData();
```

```
return (index) => data[index];
}
```

```
const getItemClosure = getItemUsingClosure(); //"fetch new data!"
```

```
//DO A LOT OF THINGS, AFTER 1000 LINES:
```

```
console.log(getItemClosure(1)); //number
console.log(getItemClosure(12));//number
console.log(getItemClosure(3));//number
```

```
//Got the same result
```

We can reuse the example from the post: [How to use apply, bind & call](https://leidson.com/post/how-to-use-apply-bind-call-in-javascript/). Transforming the bind example to use Closure, we can have this:

```
const multiply = (a) => {
  return (b) => a * b; 
}
```

```
//another way to write the multiply function:
//const multiply = (a) => (b) => a * b;
```

```
const multiplyByTwo = multiply(2);
const multiplyByThree = multiply(3);
```

```
console.log(multiplyByTwo(4)); //8
console.log(multiplyByThree(4));//12
```

Well, I hope you enjoy the post and understand the power of Closures. If you have doubts, you can send a message or comment. I’ll be glad to talk and help you.

The original post is https://medium.com/@leidson/javascript-closures-629ecd65d7b4