---
title: "Optimizing Large Data Rendering in React with Virtualization"
seoTitle: "Optimizing React Performance: Rendering Lists with Virtualization"
seoDescription: "Learn how to optimize large data rendering in React using virtualization techniques for improved performance and scalability"
datePublished: Mon Mar 10 2025 01:33:16 GMT+0000 (Coordinated Universal Time)
cuid: cm82e1qjm000208l2gic2gerq
slug: optimizing-large-data-rendering-in-react-with-virtualization
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1741570386350/d16de29e-9d26-47fd-b641-d4cec364a755.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1741570275362/56db94c5-bb20-4a4d-b344-d32fb7999d8a.jpeg
tags: interview, js, javascript, nodejs, reactjs, ui, frontend-development

---

One of the best attributes while engineering any piece of software is to center the development around the scalability aspect in future. What if the data object on which you’re trying to build the component contains tens of thousands of entries ? How will it impact the performance, the resource consumption and most importantly, the end-user’s experience?

One of the most frequent occurrence in frontend development is the rendering of lists. We happen to have the instinctive notion to use .map and render out the list. Without having a second thought, what if this array of objects contains 1,00,000 items ?

To really see it’s impact, let’s try to render out a list with merely 5000 items using the traditional approach, with no consideration of future performance issues:

## Traditional Approach

So we’ll utilize [JSON Placeholder API](https://jsonplaceholder.typicode.com/) and fetch 5000 photos and render them at once on the UI. To do so we will use 2 states, one for storing the load state and 2nd to actually store the fetched photos (The photos are not actual image files but rather placeholder urls which simulate actual fetching and rendering of images on the DOM)

This is how our LargeList component would look like:

<iframe src="https://codesandbox.io/embed/2562f4?view=editor+%2B+preview&module=%2Fsrc%2FLargeList.js" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Now before proceeding further let’s get our `App.jsx` ready but in order to see the difference we will need a utility component called `PerformanceWidget.jsx`

**Why do we need it?**

So the purpose of this component is to assess the number of DOM nodes being rendered on the UI depending on the view. To do so we use ref and document selector and select all the elements with class `list-item`

```javascript
// targetRef tracks DOM nodes to show virtualization’s impact.

useEffect(() => {
  const items = targetRef.current.querySelectorAll(".list-item");
  setDomCount(items.length);
}, [targetRef, isVirtualized]);
```

Below is the entire code of the `PerformanceMonitor` component:

<iframe src="https://codesandbox.io/embed/2562f4?view=editor+%2B+preview&module=%2Fsrc%2FPerformanceMonitor.js" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Lastly, we’ll import these components in our `App.jsx` file:

We will use a state called `isVirtualized` to decide which view to render

```javascript
import React, { useRef, useState } from "react";
import LargeList from "./LargeList";
import VirtualizedList from "./VirtualizedList";
import PerformanceMonitor from "./PerformanceMonitor";
import "./styles.css";

const App = () => {
  const [isVirtualized, setIsVirtualized] = useState(true);
  const listRef = useRef(null);

  return (
    <div className="app-container">
      <h1>Optimized Large Lists in React</h1>
      <div className="toggle-container">
        <button
          className={`toggle-btn ${!isVirtualized ? "active non-virtualized-btn" : ""}`}
          onClick={() => setIsVirtualized(false)}
        >
          Non-Virtualized
        </button>
        <button
          className={`toggle-btn ${isVirtualized ? "active virtualized-btn" : ""}`}
          onClick={() => setIsVirtualized(true)}
        >
          Virtualized (react-window)
        </button>
      </div>
      <div ref={listRef} className="list-wrapper">
        {isVirtualized ? <VirtualizedList /> : <LargeList />}
        <PerformanceMonitor targetRef={listRef} isVirtualized={isVirtualized} />
      </div>
    </div>
  );
};

export default App;
```

We’ll further look into the Virtualized view but before that let’s check the performance metrics of the normal view by rendering the `LargeList` component:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741549498822/6db2fd02-b48b-4ec7-9cbd-13d0716b7a9d.png align="center")

Few things we observe:

* The number of DOM Nodes being rendered are 5000, which leads to greater memory consumption.
    
* This leads to slower and lagging User Experience, especially on interactive operations (eg: scroll , click, etc)
    

To back what we observed , statistically , we’ll first observe the memory consumption using Chrome Devtools:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741550086336/57fee989-e36f-483e-99e5-edcc137d5abc.png align="center")

Apart from the memory monitoring, we can utilize Lighthouse to analyze the performance of our web app. Attached below is a Lighthouse report

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741563664560/95c70254-f0d9-434b-a4c4-b555e1ffbef7.png align="center")

The most critical parameter is the Total Block Time (TBT) which denotes the time for which the main JS Thread (Thread of Execution) is blocked. Higher TBT time means longer delay before the page becomes interactive, thereby stalling responsiveness.

Another critical parameter which is highlighted by the Lighthouse report is the higher number of DOM Elements:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741563980444/2b9c9d7f-ae2d-4f9e-b9e8-efea5750cc0b.png align="center")

Now that we have a fair idea of how traditional rendering affects Performance, let’s now take a look at the Virtualized Approach

## Virtualized Approach

First things first, What exactly is Virtualization ?

Virtualization, in the context of React and frontend development, means rendering only a subset of what would be visible in the viewport (what users see). So let’s say if a list contains 5000 items, through virtualization, only 10/20 (configurable) is rendered on the UI and is later swapped with the next items when scrolled.

**How do we achieve this ?**

With the help of library called `react-window` which helps to implement virtualization in React. It does so by taking in a fixed-size list and then calculating which items would be visible in the viewport and accordingly renders it on the DOM. On scroll, it unmounts the current elements and re-mounts it with the new one.

**Implementation**

The fetching logic largely remains the same but in this case we use pagination by fetching the next set of data on scroll.

```javascript
// Since fetchPhotos would be used in useEffect later, it is a best practise to ensure 
// it is not re-rendered until one of the actual dependencies change, this ensures there's 
// no additional re-renders
const fetchPhotos = useCallback(async () => {
    if (!hasMore || loading) return;

    setLoading(true);
// The fetch URL has 2 variable parameters: page and PAGE_SIZE which changes on scroll
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/photos?_start=${page * PAGE_SIZE}&_limit=${PAGE_SIZE}`
    );
    const data = await response.json();
    if (data.length === 0) {
      // hasMore flag monitors if there's more data to be fetch
      setHasMore(false); // Stop fetching if no more data
    } else {
      setPhotos((prevPhotos) => [...prevPhotos, ...data]);
      setPage((prevPage) => prevPage + 1);
    }
    setLoading(false);
  }, [page, hasMore, loading]);
```

**Rendering using react-window:**

```javascript
// FixedSizeList is imported from react-window which takes in a definite height and then
// decides what items to render and unmounts the old items and re-mounts the current items 
// that would be visible in the viewport

    <FixedSizeList
        height={350}  // defines the viewport height
        itemSize={80} // Adjusted for images
        itemCount={photos.length + (hasMore ? 1 : 0)}
        className="list-content virtualized-list"
        // onItemsRendered is called when the indices rendered in the viewport changes, in our case
        // it'll change due to scroll
        onItemsRendered={({ visibleStopIndex }) => {
           // if the last index of the list that is visible is greater than the number of fetched photos, 
           // it means API call to fetch next set of list should be made
          if (visibleStopIndex >= photos.length - 1) {
            fetchPhotos();
          }
        }}
      >
        {({ index, style }) => {
          if (index >= photos.length) {
            return (
              <div style={style} className="list-item loading">
                Loading more photos...
              </div>
            );
          }
          return (
            <div style={{ ...style, display: "flex", alignItems: "center", gap: "10px" }} className="list-item">
              <img src={photos[index].thumbnailUrl} alt={photos[index].title} width="50" height="50" />
              <span>{photos[index].title}</span>
            </div>
          );
        }}
      </FixedSizeList>
```

Below is how the whole `VirtualizedList` component would like:

<iframe src="https://codesandbox.io/embed/2562f4?view=editor+%2B+preview&amp;module=%2Fsrc%2FVirtualizedList.js" style="width:100%;height:500px;border:0;border-radius:4px;overflow:hidden" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### Performance Analysis

**Number of DOM Nodes:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741567298163/0744887f-2078-4f09-ae49-84e611a58e4c.png align="center")

We observe that the number of DOM nodes remain significantly less, this improves load speed and reduces the Block Time of the Main thread

**Heap Memory Usage:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741567519399/9e29c984-b7bb-4f32-86f6-4fab92e56846.png align="center")

We observe a significant lesser usage of memory

**Lighthouse report:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741567629634/c5c97cf1-0219-411e-9b33-a6bf7ac48747.png align="center")

We observe a significantly lesser TBT and overall increased performance along with pruning the excess DOM nodes

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1741567757298/9143393b-6b0f-4cfb-bde2-eaff4877c5d0.png align="center")

Ok, so there’s a lot of numbers floating around, let’s get it altogether, after all that’s what a good blog should conclude with!

| **Parameter** | **Traditional (Non-Virtualized)** | **Virtualized (react-window)** | **Notes** |
| --- | --- | --- | --- |
| Number of DOM Nodes | 5000 | ~10-20 (configurable) | Traditional renders all 5000 items; Virtualized renders only viewport items. |
| Total Blocking Time (TBT) | High (~1000-2000 ms) | Low (~100-300 ms) | Higher TBT in Traditional due to rendering all items, blocking the main thread. |
| Heap Memory Usage | High (~150-200 MB) | Low (~20-40 MB) | Traditional consumes more memory due to full list rendering. |
| Lighthouse Performance Score | Lower (~40-60) | Higher (~80-95) | Virtualized approach improves responsiveness and reduces DOM overhead. |
| Load Speed | Slower (e.g., ~3-5 seconds) | Faster (e.g., ~0.5-1 second) | Virtualized renders fewer items initially, improving perceived load time. |
| Scroll Performance | Lagging (Noticeable jank) | Smooth (No jank) | Traditional struggles with large DOM; Virtualized swaps items efficiently. |
| Excess DOM Elements | High (5000 elements) | Minimal (~10-20 elements) | Lighthouse flags excess DOM in Traditional; Virtualized prunes effectively. |

Thus we conclusively witnessed how a researched approach future-proofs our code to a large extent, at least from the scalability point-of-view. I hope this blog taught you as much as it taught me!

If you’ve read till here, give yourself a round of applause, you’re truly one passionate engineer!

I really appreciate it, would love to hear your feedback, your takeaway or anything you wish to say!