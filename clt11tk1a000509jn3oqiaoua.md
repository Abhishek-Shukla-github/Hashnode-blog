---
title: "Knowing JS: Execution Context and Code Execution"
seoTitle: "Knowing JS: Execution Context and Code Execution"
seoDescription: "Knowing JS is a series on JS fundamentals.
In this blog we understand the Execution Context and how it's crucial to JavaScript"
datePublished: Sun Feb 25 2024 05:07:38 GMT+0000 (Coordinated Universal Time)
cuid: clt11tk1a000509jn3oqiaoua
slug: knowing-js-execution-context-and-code-execution
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708837194109/1b15c568-e932-4afd-8cca-11dacbc63dbc.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1708837538978/e4ec3c8b-8b8e-4603-882a-5876f05bc0ca.jpeg
tags: javascript, fundamentals

---

> There's a famous phrase in JavaScript Ecosystem: "*All code execution in JS happens inside an Execution Context* "

### Now what is an Execution Context?

As the name suggests, it’s a context, an abstract environment where our code resides and executes

Now how does our code reside and execute you may ask :) 

Let’s take a granular view of the Execution Context

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708826351758/0c1eaa1d-7a22-4b0f-9580-aa5fd9ceea73.jpeg align="center")

Imagine an Execution Context is like a box which contains 2 components:

1. **Memory Component**: All the variables and functions in our program are stored inside this Memory component in form of key value pair. Hence this Memory component is also called as **Variable Environment.**
    
2. **Code Component**: Code Component is where the code is executed, we’ll take a look of why there’s a need for Code Component further in this blogpost. But for now keep in mind that since this component is responsible for Code Execution , it is also called **Thread of Execution.**
    

Now that we have a basic understanding of what an Execution Context is , let's take a look at how it plays a role in code execution.

Consider the following code:

```javascript
console.log("Welcome!")

function calculateSum(array) {
    let sum = 0;
    for (let i = 0; i < array.length; i++) {
        sum += array[i];
    }
    return sum;
}

console.log(calculateSum(arr));
```

Before we start looking into the execution , here’s another golden nugget for you to remember:

> **JS is a Single-Threaded Synchronous programming language**

So we know that JS can only execute one line at a time sequentially

But then what does AJAX mean ? Well, we’ll look at it after the callbacks which give JS the async powers, but that's a topic for another day.

Coming to the code execution, it happens in 2 pass (phases):

1. **Memory Allocation Phase:**  
    In this phase, JS skims through the entire program and stores variables and functions in the **Memory** component in **key-value pair format**, most importantly , this happens even before a single line of code is executed.
    
    The variables are stored as “*undefined* ” and the functions are stored as their entire *function definition* (keep in mind there are few exception with regards to **closures**)
    
    Let's understand phase 1 with our above code example, if a breakpoint is added to line 1 , it denotes that no code is executed. Let's see what happens:
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708832138529/9b4b40e7-2b5b-4947-802a-f281deb07982.png align="center")
    
    We observe that the console is still empty , which means the very first line is not executed
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708832157502/cc7d8e46-cd39-422c-a511-1996127b7c98.png align="center")
    
    Now if we switch to the Sources tab and take a look at the global object, we find that the variable **arr** is assigned ***undefined*** and further below we find that the **calculateSum** function also exists and points to its actual and entire ***function definition***.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708832493130/cccc5135-2f19-4f07-ad2f-50ce7fee54c8.png align="center")
    
    All this before a single line is executed , therefore we see that memory is allocated to all the variables and functions prior to code execution , this constitutes the end of Phase 1: The Memory Creation phase.
    
2. **Code Execution Phase:**
    

In this phase the actual code execution starts line-by-line in a sequential manner

If we add a breakpoint at line 2, so first line will be completed executing and the console would print "Welcome!":

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708833166126/2f9c2901-e3cf-42fb-a554-883df5e0cba6.png align="center")

We observe that Welcome is printed on the console and in the Global scope the values will still be the same as they were in Phase 1.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708834775018/2a5cd28f-0c6e-4009-9a3f-32bfd4789a8b.png align="center")

However now if we shift the breakpoint to line 3, which would mean the execution of line 2.

Now we observe that variable ***arr*** *is assigned the array* ***\[3,5,7,9,10\]*** in the Global Object

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708833643462/2285caac-4198-42bd-9d72-175331e65edd.png align="center")

If we add a breakpoint on line 12:

```javascript
console.log(calculateSum());
```

we’ll observe that console has still not printed and the actual sum i.e the **calculateSum** is still not invoked

Let’s make that happen, now we see that **calculateSum** is invoked.

At this step , something interesting happens, as we discussed that every code in JS is executed inside an Execution Context .

Let me give you another golden Nugget:

> **Every function invocation leads to the creation of a new Execution Context for that function.**

Okay , but now what happens to the Global Execution Context , how will JS manage the 2 execution contexts and what if more are on the way?

Enters the **Call Stack** for the rescue!  
Call stack is, as the name suggests , a stack that **maintains the order of execution** of Execution Contexts  
  
Basically on invocation , an Execution Context of that function is **pushed** **to**,  and on completion of execution , that Execution Context is **popped from** the Call Stack.

Whenever a program is executed in JS, the Global Execution Context is pushed inside the Call Stack and when a new function is invoked , it’s Execution Context is pushed inside the Call Stack too and popped once execution is completed

Let’s have a look at what we just discussed and modify our code a bit to include more functions:

```javascript
console.log("Welcome!")
var arr = [3,5,7,9,10];

function calculateSum(array) {
    let sum = 0;
    for (let i = 0; i < array.length; i++) {
        sum += array[i];
    }
    return sum;
}

function calculateProduct(array) {
    let prod = 1;
    for (let i = 0; i < array.length; i++) {
        prod *= array[i];
    }
    return prod;
}

function calculateSquare(array) {
    return array.map(num => num*num);
}

console.log(calculateSum(arr));
console.log(calculateProduct(arr));
console.log(calculateSquare(arr));
```

If we add a breakpoint on line 1 , we observe only the Global Execution Context in the Call stack

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708835256421/ee00cdc7-ebc6-483e-9a2b-063970000396.png align="center")

Now let's add the breakpoint on a line inside the **calculateSum** function:

```javascript
 let sum = 0;
```

We observe only Welcome is printed on console and on looking at the Call Stack from Sources tab

We observe that call stack now contains **Global Execution Context** as well as the **calculateSum’s Execution Context** pushed on top

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708835402096/19d13ca5-0e45-416a-9331-4b4c92b3e77f.png align="center")

Let's further shift the breakpoint inside the **calculateProduct** function on line:

```javascript
 let prod = 1;
```

We observe calculateSum is now **popped off** the Call Stack and **calculateProd** is not **pushed** on top

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708835513655/7bc9660b-56be-4358-ad11-d0da30301e11.png align="center")

Let's try the same for **calculateSquare** function and we observe that calculateProd is not popped off and **calculateSquare** is pushed on top

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708835671926/f338f18b-ca17-4e2e-ba45-b363b407441c.png align="center")

Lastly once we remove all the breakpoint, we observe console prints the results and Global Execution context is also popped off the Call Stack leaving it empty, marking the end of execution

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708835746308/038629fc-4bb5-4e50-8884-5bf3fb43311e.png align="center")

Therefore we saw how Execution Context helps in code execution and why they're important and how JS efficiently and splendidly manages all the Execution Contexts with the help of Call Stack

That's all I wanted to share, my understandings, leaving you with a positive note:

> If you're not good at something, it's not because of your innate inability to be so but only because you're inexperienced in it.