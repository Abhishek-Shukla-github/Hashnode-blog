---
title: "Implementing a Curried Sum Function"
seoTitle: "Knowing JS - Curried Sum Function Implementation"
seoDescription: "Knowing JS - Learn how to implement a curried sum function in JavaScript to enable method chaining and closures"
datePublished: Thu Sep 19 2024 04:58:35 GMT+0000 (Coordinated Universal Time)
cuid: cm18tn8s3001v09l72uk3bbza
slug: implementing-a-curried-sum-function
tags: interview, js, javascript, frontend-development, problem-solving-skills

---

Question Link: [https://bigfrontend.dev/problem/create-a-sum](https://bigfrontend.dev/problem/create-a-sum)

Our goal is to achieve the following functionality:

```javascript
const sum1 = sum(1)
sum1(2) == 3 // true
sum1(3) == 4 // true
sum(1)(2)(3) == 6 // true
sum(5)(-1)(2) == 6 // true
```

As we see, we need to have some sort of method/functional chaining, to do this we can be sure that the `sum` function should return a function which in turn can be called over and over again thereby forming a chain.

And the way we will achieve this is by utilizing the concept of Currying

### What is Currying?

Currying in JavaScript is a technique wherein a function that can take multiple arguments is transformed to a function that takes a single argument and returns a function that in turn takes the further arguments, and so on until all the arguments are received.

### Approach

* Return an inner function from the `sum` function. This inner function takes the next argument of the chain and adds it to the current sum argument.
    
* This happens because the inner function forms a closure with the sum function and remembers the lexical environment (which includes the arguments) even when it's called individually
    
    * Then the recursive call happens with the updated argument passed to sum and inner function again having access to this updated argument. (This will be clear with the breakdown below)
        
* In the end since we are returning the inner function itself but we need to compare the final sum with the given value
    
* To achieve this we alter the .valueOf property through which we’ll return the num argument which will help us compare. The reason for this is when a function is compared to a primitive value , the valueOf property is called and the assigned function is executed.
    
* Let’s understand it with an example
    

```javascript
function returnTwo(){
    console.log("Does Nothing")
}

returnTwo.valueOf = () => 2
returnTwo == 2 // returns true
// Because when returnTwo == 2 is executed, 
// the .valueOf function of the returnTwo object is called 
// and it returns 2 due to type coersion in case of "=="
// and it'll fail in case of "===" because of no type coersion (it strictly checks a functional object to a primitive number)
```

### Code

```javascript

function sum(num) {
  const innerFunction = (nextVal) => sum(num+nextVal)
  innerFunction.valueOf = () => num
  return innerFunction; 
}
```

### Visual Walkthrough

Consider the example `sum(5)(-1)(2)`, below is the visual flow of how the code will execute which helps better grasp and understand the flow

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1726719846213/0098e74b-ccd0-4fd8-ad41-c524ae045bee.jpeg align="center")](https://miro.com/app/board/uXjVLdM7eqk=/?share_link_id=68059689334)

With that we’re able to solve this problem of creating a curried sum.

Ending this blog with a positive note to help fuel your day towards your endeavors:

### “Discipline is choosing between what you want now and what you want most.” - **Abraham Lincoln**.