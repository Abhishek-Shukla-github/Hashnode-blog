---
title: "Creating a Promise.any() polyfill"
seoTitle: "Knowing JS - Promise.any() Polyfill Guide"
seoDescription: "Learn how to create a Promise.any() polyfill to handle promise aggregation effectively"
datePublished: Thu Sep 12 2024 04:11:09 GMT+0000 (Coordinated Universal Time)
cuid: cm0yrvaf8003y0ajq3ktb5c3u
slug: creating-a-promiseany-polyfill
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Zs5X1KnHUzw/upload/b7508f8f6003f900bbc689b9d50aac2c.jpeg
tags: interview, javascript, frontend-development, problem-solving-skills

---

Problem Link: [https://bigfrontend.dev/problem/implement-Promise-any](https://bigfrontend.dev/problem/implement-Promise-any)

First things first, as usual :)

### What is a Promise?

An object that represents an eventual completion of failure of an asynchronous operation

### What is a polyfill?

A function or a snippet of code that mimics a particular functionality. Created majorly to provide desired functionality in older browsers which don’t support latest, usually higher order operations

### What is Promise.any() ?

A static method that takes an iterable of promises and resolves to the value of the first resolved promise, if not , it rejects when all the promises reject, by combining all the error reasoning using the `AggregateError` constructor

### Approach

* Return a promise
    
* Have a counter variable to keep track of rejected promises and an array to store the error’s for the corresponding promises
    
* Loop over the given promises array:
    
    * For each promise, if it resolves, directly resolve the main promise
        
    * Else, when it fails, store the error at the appropriate index and increment the error counter
        
    * If the error counter equals the length of given promises array, it means none of the promises were resolved and all of them were rejected.
        
    * Hence, combine all the errors by passing the error array to the `AggregateError` constructor to the reject function of the main promise function.
        

## **Code Breakdown and explanation**

```javascript
function any(promises) {
// Edge Case: Empty array
if (promises.length === 0) {
  return Promise.reject(new AggregateError('No promises were passed', []));
}
// rejectCount keeps track of the number of promises reject, 
// which will help us to identify if all the promises are rejected
  let rejectCount=0, rejectedPromisesArr = [];
// return a Promise 
 return new Promise((resolve,reject) => {
    promises.forEach((promise,i) => {
      Promise.resolve(promise)
// If a Promise is resolved ,directly resolve it
      .then(resolve)
      .catch(err => {
// If it fails, store the error at corresponding index of the rejected Promise
        rejectedPromisesArr[i] = err
// Increment the rejected count
        rejectCount++;
// Once the rejection count matches the length of the given promises array 
// i.e all the Promises are rejected, 
// it's time we reject the main promise function by 
// passing rejectPromisesArr to the AggregateError constructor
        if(rejectCount === promises.length) reject(new AggregateError("No promises resolved",rejectedPromisesArr))
      })
    })
  })
}
```

And that’s it, we have successfully implemented the Promise.any() polyfill.

It’s a bit tricky question, so are most of the Promises polyfills. But functionally it’s the opposite of Promise.all(), you can find it’s blog [here](https://abshukla.hashnode.dev/creating-a-promiseall-polyfill)

A positive thought for you incase you made it till here!

### *Those who dare to fail miserably can achieve greatly- JFK*