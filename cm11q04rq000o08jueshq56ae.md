---
title: "Creating a Promise.race() polyfill"
seoTitle: "Knowing JS - Promise.race() Polyfill Guide"
seoDescription: "Learn to create a Promise.race() polyfill"
datePublished: Sat Sep 14 2024 05:42:14 GMT+0000 (Coordinated Universal Time)
cuid: cm11q04rq000o08jueshq56ae
slug: creating-a-promiserace-polyfill
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/9l_326FISzk/upload/0ad15140741d201d38f541953f682d49.jpeg
tags: interview, js, javascript, reactjs, coding, problem-solving-skills

---

Problem Link: [**https://bigfrontend.dev/problem/implement-Promise-race**](https://bigfrontend.dev/problem/implement-Promise-race)

Continuing the Knowing JS series, with a new question!

First things first, as usual :)

### **What is a Promise?**

An object that represents an eventual completion of failure of an asynchronous operation

### What is a polyfill?

A function or a snippet of code that mimics a particular functionality. Created majorly to provide desired functionality in older browsers which don’t support latest, usually higher order operations

### What is Promise.race() ?

A static method that takes an iterable of Promises and resolves to the first resolved value of the promise or rejects with the first rejection of the promise. As the name suggests, it races to the first resolution or rejection

### Approach

* Return a Promise
    
* Iterate over the given promises array
    
    * Resolve when any of the promise resolves
        
    * Reject when any of the promise rejects
        

## Code Breakdown and explanation

```javascript
function race(promises) {
// Return a Promise
  return new Promise((resolve,reject) => {
// Iterate over the given promises array
    promises.forEach((promise) => {
// Resolve the main promise when any of the promise from the array resolved
// Same for reject
      Promise.resolve(promise).then(resolve).catch(reject)
    })
  })
}
```

And that’s it, race is often the easiest and the most straightforward Promise polyfill to implement, as we just saw, the better we understand the concept, easier it is to implement it

### No quotes for such an easy question :)