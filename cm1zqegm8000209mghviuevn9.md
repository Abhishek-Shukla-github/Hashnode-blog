---
title: "Finding the corresponding node in 2 identical DOM trees"
seoTitle: "Knowing JS: Locate Node in Identical DOM Trees"
seoDescription: "Knowing JS: Learn how to find corresponding nodes in two identical DOM trees and explore recursive solutions and performance considerations in this guide"
datePublished: Tue Oct 08 2024 00:57:33 GMT+0000 (Coordinated Universal Time)
cuid: cm1zqegm8000209mghviuevn9
slug: finding-the-corresponding-node-in-2-identical-dom-trees
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/aJwQEOndmKY/upload/09b42307edfbaa83a2d7888dbd6a175c.jpeg
tags: interview, js, javascript, frontend, frontend-development

---

Problem Link: [https://bigfrontend.dev/problem/find-corresponding-node-in-two-identical-DOM-tree](https://bigfrontend.dev/problem/find-corresponding-node-in-two-identical-DOM-tree)

DOM traversal is one of the crucial aspects for any Frontend Interview especially in the top tech companies.

However it’s also important from the conceptual Point of View and albeit rare that you have to manually change a DOM Node , still the understanding of the DOM Tree helps you solidify the internal workings of the frontend frameworks like React, Vue, etc.

In this problem we’ll find the corresponding node in 2 identical trees.

### Now what does corresponding node mean?

The *trees A* and *B* are of the same structure and the *Node a* in Tree A is at the same ***relative position*** as Node b in Tree B, we need to find the **b Node** given the Node a and Trees A and B

### Approach:

We’ll take a recursive approach and will try to iterate over all the childrens and sub-childrens of each node of each tree, until we get the result, or null in case we don’t

### Code:

```javascript
const findCorrespondingNode = (rootA, rootB, target) => {
//We define the recursive function which takes the 2 nodes for comparison
  const traverse = (nodeA,nodeB) => {
//In case either of the node is not defined, the trees are not the clone, 
// Hence return null
    if(!nodeA || !nodeB) return null

//As soon as the Node A matches the given target node 
// -> it's corresponing node on Tree B i.e nodeB will be the desired Node
//Hence we return it
    if(nodeA === target) return nodeB

//Else we continue finding the children of both the nodes
    let childrenA = nodeA.children
    let childrenB = nodeB.children

//We loop over the childrens of A
    for(let i=0;i<childrenA.length;i++){
// Here we make the recursive call and 
// pass the ith child of both trees to the function
      let correspondingNode = traverse(childrenA[i],childrenB[i])
 // If the corresponding node exist we return it
      if(correspondingNode) return correspondingNode
    }
// If not at this point we'll return null and this will act 
// as the termination step of the recursive function
    return null;
  }
// Make the 1st recursive call by passing both the given root nodes
  return traverse(rootA,rootB)
}
```

And that’s it, pretty straightforward, Now we do have some nice follow-up questions, which I might write later about!

* *Could you solve this problem with special DOM API for better performance?*
    
* *What are the time cost for each solution?*
    

That’s it, Hope you find this blog insightful and found something to learn from!

As usual, leaving you with a positive note to help you fuel forward the remainder of your precious day:

### “Stay hungry, stay foolish.” - *Steve Jobs*