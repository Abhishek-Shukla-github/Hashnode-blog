---
title: "Creating a Promise.all() polyfill"
seoTitle: "Knowing JS - Promise.all() Polyfill Creation Guide"
seoDescription: "Create a polyfill for Promise.all() to support older browsers. Learn about Promises and the approach to implement it effectively"
datePublished: Fri Aug 30 2024 06:35:46 GMT+0000 (Coordinated Universal Time)
cuid: cm0gcb6n1000j0alb4fi172eb
slug: creating-a-promiseall-polyfill
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/w7ZyuGYNpRQ/upload/d6c8aca57564962f9bcff1eb9fd790ad.jpeg
tags: js, javascript, frontend-development, dsa

---

Problem Link: [https://bigfrontend.dev/problem/implement-Promise-all](https://bigfrontend.dev/problem/implement-Promise-all)

First things first, what is a polyfill?

**Polyfill** is a function/snippet of code which mimics a specific functionality.

The major usage of polyfills is to provide the desired functionality in the older browsers which do not natively support that particular functionality.

### What is a Promise?

Getting the core basics right is essential to understand the higher order concepts

A Promise in JavaScript is an **object** representing a future **completion** or **failure** of an **asynchronous** operation

### What is Promise.all()

*Promise.all()* is a static method (a method that is available on the class itself rather than it's instance) which accepts an array/iterable of promises and returns a promise which either resolves when **all** of the passed promises resolve (while maintaining the **order** in which the promises were passed) or **rejects** when the **very first** of the passed promise rejects

### Approach:

* Return a promise
    
* Maintain an array for collecting the resolved promises and a count to keep track of all the resolved promises
    
* Iterate on the given *promises* array:
    
    * For each promise try to resolve it , if it does, insert to array on it's corresponding index and increase the resolved promises count
        
    * If the count is equal to that of the length of the given ***promises*** array, this means that all the promises are resolved, so we finally resolve the promise that is to be returned, with results array passed in as it's value
        
    * If the promise fails, then reject it by using a catch block.
        

## Code Breakdown and explanation

```javascript
function all(promises) {
  
  //Initialization
  let res = [];
  let resolvedPromiseCount = 0

//Returning a Promise because Promise.all() returns one too
  return new Promise((resolve,reject) => {
    //An Edge case, in case there's no Promise present
    if(promises.length === 0) return resolve([]);
    promises.forEach((promise,i) => {
    // Using Promise.resolve(promise) ensure even if the given value 
    // is not a promise, it is wrapped in a Promise

    // promise.then() can fail in cases where the given 
    // value can be a non-promise (eg: string, number, boolean,etc) 
    // because .then won't exist on those
      Promise.resolve(promise).then((val) => {
        // increment the count, to keep track of all resolved promises
        resolvedPromiseCount++;
        // insert the resolved promise to the result array, at corresponding index 
        // because Promise.all() returns the promises in which they were passed
        res[i] = val
        // This check indicates that all the promises are resolved , 
        // so we can now resolve the main Promise that is to be 
        // returned with the value as result
        if(resolvedPromiseCount === promises.length) resolve(res)
      })
        // If a Promises fails, immediately reject it
      .catch((e) => reject(e))
    })
  })
}
```

So there you go, you've just created the polyfill for Promise.all()

I always like ending my blogs with something positive:

### ***"Things may come to those who wait, but only the things left by those who hustle."*** ***\- Honest Abe***