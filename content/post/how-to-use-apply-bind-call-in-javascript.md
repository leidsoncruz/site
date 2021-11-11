---
title: "How to use apply, bind & call in Javascript"
tags: ['JavaScript']
date: 2020-11-08T19:00:04.294000
---

![Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/11520/0*DqWi0UdvXPtCBVS8)*Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

This post has the intention to discuss the difference between **apply** , **bind** , and **call** methods.


<!--more-->

In Javascript, functions are **Function Objects** . The **Function Object** has properties like *name, arguments, etc.* Also, it has methods like *apply, bind, call, toString, etc* .

## Function.prototype.apply()

The **apply()** method invokes the function immediately. When you use it, you need to pass to the function the **this** value. The apply method accepts arguments as an array, but it’s optional. The **this** value indicates the context that the function will execute.

 *“Ok, I understood. How I use it?”* 

```
const animal = function(name, method) {
 console.log(`${name} do ${this.sound}, using ${method}`);
}
```

```
const dog = { sound: “auuuuuuu” };
const cat = { sound: “miauuuu” };
```

```
animal.apply(dog, [“Thor”, “apply”]);
// "Thor do auuuuuuu, using apply"
```

```
animal.apply(dog, [“Scarlett”, “apply”]);
//"Scarlett do auuuuuuu, using apply"
```

It’s like you borrow the function to apply in another context. I’ll give another example:

```
const wizard = {
 name: “Gandalf”,
 health: 100,
 heal(hp){
 this.health += hp
 }
};
```

```
const archer = {
 name: “Legolas”,
 health: 30
}
```

```
wizard.heal.apply(archer, [10]);
```

```
console.log(archer);
//{name: "Legolas", health: 40}
```

## Function.prototype.call()

Like the **apply()** method, the **call** invokes the function immediately and receive the **this** value as a parameter. The only difference between the **Apply** and **call** method is how to pass the arguments to them. The **Apply** method receives an array as the arguments. The **call** method receives the arguments as a simple param. Reusing the previous examples:

```
const animal = function(name, method) {
 console.log(`${name} do ${this.sound}, using ${method}`);
}
```

```
const dog = { sound: “auuuuuuu” };
const cat = { sound: “miauuuu” };
```

```
animal.call(dog, “Thor”, “call”);
// "Thor do auuuuuuu, using call"
```

```
animal.call(dog, “Scarlett”, “call”);
//"Scarlett do auuuuuuu, using call"
```

```
const wizard = {
 name: “Gandalf”,
 health: 100,
 heal(hp){
 this.health += hp
 }
};
```

```
const archer = {
 name: “Legolas”,
 health: 30
}
```

```
wizard.heal.call(archer, 20);
```

```
console.log(archer);
//{name: "Legolas", health: 50}
```

## Function.prototype.bind()

The **bind()** is different from the previous methods, doesn’t invoke the function immediately. It returns a new function with a new context ( **this** value). Taking the wizard heal example, we can apply the **bind()** method like this:

```
const wizard = {
 name: “Gandalf”,
 health: 100,
 heal(hp){
 this.health += hp
 }
};
```

```
const archer = {
 name: “Legolas”,
 health: 30
}
```

```
const healArcher = wizard.heal.bind(archer, 45)
console.log(archer); 
//Does not change the health of the archer because the function wasn't invoked.
//{name: "Legolas", health: 30}
```

```
healArcher();
console.log(archer);
//{name: "Legolas", health: 75}
```

More examples to use the bind method:

```
function multiply(a,b){
 const value = a*b;
 console.log(“multiply”, value);
 return value;
}
```

```
let multiplyByTwo = multiply.bind(this, 2);
multiplyByTwo(4);
//"multiply", 8
```

```
let multiplyByTen = multiply.bind(this, 10);
multiplyByTen(4);
//"multiply", 40
```

Thank you for reading and I hope this post will be helpful for you to understand the differences between apply, bind, and call.

The original post is https://medium.com/@leidson/how-to-use-apply-bind-call-in-javascript-f47970454b6a