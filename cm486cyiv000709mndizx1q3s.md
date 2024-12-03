---
title: "Transforming Undefined to Null: A Guide to Standardizing JSON Data Processing"
seoTitle: "Transforming Undefined to Null: A Guide to Standardizing JSON Data Pro"
seoDescription: "Learn how to convert undefined to null in JSON data for consistent API processing using recursion or Lodash"
datePublished: Tue Dec 03 2024 08:05:51 GMT+0000 (Coordinated Universal Time)
cuid: cm486cyiv000709mndizx1q3s
slug: transforming-undefined-to-null-a-guide-to-standardizing-json-data-processing
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Zs5X1KnHUzw/upload/7a3803d458b5c6cc085c02c60eee2c17.jpeg
tags: interview, blogging, javascript, technology, typescript, frontend-development

---

Practice Link: [https://bigfrontend.dev/problem/undefined-to-null](https://bigfrontend.dev/problem/undefined-to-null)

undefined and null are the 2 primitive data types in JS but they behave very differently especially when it comes to JSON.stringify()

We'll understand this with help of an example:

```javascript
JSON.stringify({a: null})      // '{"a":null}'
JSON.stringify({a: undefined}) // '{}'
```

This can lead to broader issues when dealing with indeterminate backend data from API calls.

In order to standardize the request and response cycle, it's always a good practice to convert one data type into another usually undefined to null.

```javascript
undefinedToNull({a: undefined, b: 'abshukla.com'})
// {a: null, b: 'abshukla.com'}
undefinedToNull({a: ['abshukla.com', undefined, 'bigfrontend.dev']})
// {a: ['abshukla.com', null, 'bigfrontend.dev']}
```

Now that we got our intention as to why we're doing what we're doing in the first place, let's talk about the intuition.

### Intuition

Our goal is to iterate over the given `argument` and replace all the `undefined` values to `null`.

Once we begin traversing the children, we may encounter further iterables, such as `arrays` or `objects`. So we need to again do the same process of iterating over its children and replacing the undefined to null.

What does this sound like? It’s like calling a function on an entity, then calling it again on its child entities, repeatedly

Yep, Recursion is the answer.

So we'll write a **recursive function** that would have 4 cases

If entity itself is `undefined`, we'll replace it with `null`. This will act as our base case

If entity is an `Array`, we'll call recursion on all its element

If entity is an `object`, we'll call recursion on all it’s keys

If entity is a **primitive** data type eg: `string`, `number`, etc we'll return it unchanged

## Code

```javascript
function undefinedToNull(arg) {
  // the base case if arg is undefined, return null in it's place
  if(arg === undefined) return null

  // if arg is an array, iterate over each element and make the recursive call
  // .map will create the new Array with all undefined converted to null
 // make sure to return it!
if(Array.isArray(arg)) return arg.map(item =>undefinedToNull(item))

  // if arg is an object, iterate over all the keys and make the recursive call
  if(typeof arg === "object" && arg !== null){
// have a storage object to store the modified key values    
    const result = {} 
// call the recursion on the value of all the keys 
    for(let key in arg){
// and store the returned value in the new object 
// corresponding to the original key
      result[key] = undefinedToNull(arg[key]) 
    }
    return result    // return the newly created object
  }

  //if arg is a primitive data type, return as is
  return arg
}
```

So we have successfully created an undefined to null helper function.

I usually keep them as utils function and use it as a callback when dealing with async API calls.

### Limitations and further considerations

Talking about the scaling aspect, the recursive solution may or may not be the best approach depending on the incoming data, as JS’ call stack has a limit beyond which we can encounter stack overflow issues.

One of the counter approaches could be using the iterative approach but the code get’s quite complex and is counter intuitive.

One of the more standard and hands-off approach would be to use something like `mapValues` from [**lodash**](https://lodash.com/docs/4.17.15#mapValues) library because when operating on large datasets , a tried-and-tested library like lodash is much a better choice.

Below is a sample usage:

```javascript
const _ = require('lodash');

function undefinedToNull(arg) {
  if (arg === undefined) return null;
  if (Array.isArray(arg)) return arg.map(undefinedToNull);
  if (_.isObject(arg)) return _.mapValues(arg, undefinedToNull);
  return arg;
}
```

So we understood the need, the approach , scaling limitations and measures to counter it.

In summary, converting `undefined` to `null` in JSON data ensures consistent, predictable serialization. Whether you choose the recursive or Lodash approach, this is a valuable technique for ensuring your API data is standardized and error-free.

I’ll leave you with that. Let me know if you enjoyed the blog - any feedback would be greatly appreciated. If you’ve read this far, not many do! :)

As usual I like to end my blogs with something sanguine and uplifting.

## “The will must be stronger than the skill.” - **Muhammad Ali**