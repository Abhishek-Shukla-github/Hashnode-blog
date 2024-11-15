---
title: "Implement Array.prototype.flat()"
seoTitle: "JavaScript Array.flat Implementation"
seoDescription: "Learn how to implement JavaScript's Array.prototype.flat function with a step-by-step guide and examples"
datePublished: Fri Nov 15 2024 08:39:22 GMT+0000 (Coordinated Universal Time)
cuid: cm3ihmqo1000m09l90e2cdwqc
slug: implement-arrayprototypeflat
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/CmF_5GYc6c0/upload/cd4b01f75dc21619a451854cb8dc14be.jpeg
tags: interview, js, javascript, frontend, typescript, frontend-development

---

In this blog we’ll implement one of the tricky JS Array prototype functions which is the `flat` function

Practice Link: [https://bigfrontend.dev/problem/implement-Array-prototype.flat](https://bigfrontend.dev/problem/implement-Array-prototype.flat)

MDN: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

### What is the flat function?

Flat function recursively (can be done iteratively too) concatenates sub-array elements to the new Array, of specified depth.

Few Examples:

```javascript
const arr = [0, 1, [2, [3, [4, 5]]]];

console.log(arr.flat());
// output: Array [0, 1, 2, Array [3, Array [4, 5]]]

console.log(arr.flat(2))
// output: Array [0, 1, 2, 3, Array [4, 5]]

console.log(arr.flat(Infinity))
output: [0,1,2,3,4,5] // Infinity will flatten all the nestings
```

### Intuition

We know that flat function works recursively, so for every sub-array element in the array, we’ll spread it to the main result because we need to concatenate the sub-array elements. So 2 keys steps: **Recursive Spreading**

### Steps

* Create a `result` array
    
* Iterate through the given array and for each item
    
    * If the item is an Array and depth is greater than 0:
        
        * make the recursive call to the `flat` function with this item and `depth -1`
            
        * now spread the returned array, and push to the `result` array
            
    * else, item is a normal element, hence directly push it to the `result` array
        
* return the `result` array
    

```javascript
function flat(arr, depth = 1) {
  const result = []    //Stores the final result of flattening

//V.Imp: for..of would fail on "holes" i.e [12,,,13] because it treates the empty
//holes as undefined and will push undefined to the res
//forEach works because it never invokes the callback function on holes 
  arr.forEach((item) =>{
//Check if the item is an array and if the depth is greater than 0 ,
//i.e we can flatten this array item
    if(Array.isArray(item) && depth > 0){
//Make the recursive call on this item and reduce the depth
//and spread the result that will be returned by the flat function
//and push it to result array
      result.push(...flat(item,depth-1))
    }
//if the item is not an array, directly push it to the the result array
    else result.push(item)
  })
  return result;
}
```

I’ve tried and created a flowchart of the recursion flow with an example

I hope it helps clear your understanding, if not , I’ll be readily available to enhance this explanation further

### Recursive Flowchart:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1731658727862/7559fab3-dd6a-4b9c-8068-a3b8dff8b1d7.jpeg align="center")

I hope the above recursive flowchart gives you a better understanding of how exactly our function works.

And that’s it for today, if you read it till here, let me know if the blogs are lacking in any aspects, be it brevity, clarity or elaboration. I’m open to and passionately look forward to any feedbacks and discussions.

Thank you and wish you a good day ahead.

Leaving you with a quote, as usual :)

### “We can only see a short distance ahead, but we can see plenty there that needs to be done.” - *Alan Turing*