---
title: "Vite :- A 5-step guide to get you started"
seoTitle: "Build React apps feeling like The Flash with Vite"
seoDescription: "Vite :- A 5-step guide to get you started building React apps feeling like The Flash"
datePublished: Sat Jul 08 2023 19:59:20 GMT+0000 (Coordinated Universal Time)
cuid: cljufintr000509mi6e488spa
slug: vite-a-5-step-guide-to-get-you-started
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/UYsBCu9RP3Y/upload/9915e652c987ed3c436b288eb08907c5.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1688846059527/e2d2b1e9-fb18-482e-acaa-e6a3c3c10727.png
tags: js, javascript, npm, reactjs, vite

---

## What is Vite?

Vite which translates to "quick" in French, is a frontend build tool for JS, developed by Evan You, who also happens to be the creator of VueJS.

Now what does a **frontend build tool** mean?

When developing or shipping a web app (using ReactJS in our example) to production, we create a build that involves combining all our code files into bundles. The household technology being used in the React ecosystem is perhaps [Webpack](https://webpack.js.org/) which ships by default with *create-react-app*.

However, the Webpack way of spinning up a dev server and generating build files prolongs wait time as the project scales, and this slow feedback loop leads to diminished productivity. This is where Vite comes into the play and let's look at how Vite does it so.

## How Vite works and achieves such speed?

Vite spins up a dev server quickly because it categorizes the entire codebase into 2 categories:

1. **Dependencies:** Vite pre-bundles dependencies (eg:-  library packages, node\_modules) using [esbuild](https://esbuild.github.io/) which is a GO-based bundler and is several times faster than the JS-based bundlers. The pre-bundling happens as soon as we run the `vite` command for the very first time. This reduces load time in subsequent reloads.
    
2. **Source Code**:  Vite serves the source code (the code that is often subject to change eg: components, utils, etc) using browser-based native [ECMAScript Modules (ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules). With this approach, Vite only needs to transform and serve the module as and when the browser requests one. This approach of leveraging the ESM also allows Vite to effectively use Hot Module Replacement (HMR) feature where the changes made to a file are reflected almost instantaneously because Vite needs to only replace the changed module rather than re-building the entire app.
    

When it comes to creating builds, Vite uses [Rollup](https://rollupjs.org/) bundler, which has better optimization techniques to create smaller chunks thereby resulting in faster build time than traditional Webpack bundler.

## Hands-on

Now That we have a decent understanding of Vite, let’s get hands-on experience by setting up a React app with Vite.

Ensure your system has Node and NPM installed (Yarn can be used too as your preferred Package Manager)

To install Vite:

```bash
npm create vite@latest
```

You'll then be asked to enter the project name and select the framework, in our case it's React

![Choose the framework](https://cdn.hashnode.com/res/hashnode/image/upload/v1688830857205/42d1216b-e876-40a4-b5da-ee930f7c7d45.png align="center")

Select the language support as JS (or TS if you wish to)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688831293412/05539e47-39c1-416e-9499-a9fa45c7aab9.png align="center")

We can now install the node modules by running :

```bash
npm install
```

Once the packages are installed we can now spin up the dev server by running:

```bash
npm run dev
```

There you go, now you have a React app ready which is much faster (with respect to the feedback loop) and provides a rich Developer Experience(DX).

![Preview of the running server by Vite](https://cdn.hashnode.com/res/hashnode/image/upload/v1688831841541/09ff5554-80e1-4251-b685-5368b385a70d.png align="center")

We can now fiddle around and create our components.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688831571614/667b7203-d95c-45aa-bf0d-585da0d51547.png align="center")

As observed, the time elapsed to get the server up and running was *502ms* which is blazing fast, as compared to a Webpack server.

To create the production-grade build we need to run:

```bash
npm run build
```

This will generate the build files which can then be served locally for testing or can be deployed on any node server

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1688837020256/16f8e217-8bce-4df8-9b89-cdc786810fbd.png align="center")

As we see, the build took *972ms* which is relatively fast for a simple application as compared to a typical vanilla React-based Webpack build for the same. The reason Vite achieves this is by utilizing Rollup bundler for creating production builds, which (without going into the intricacies) is a better bundler than Webpack since it produces smaller bundle sizes, has better tree-shaking capabilities and optimizes build by leaving out the unused code.

# Why you should use Vite?

As we saw, with very minimum dev effort in setting up a Vite-based React app we were able to unlock better capabilities and functionalities which easily boosts-up productivity and DX. To list them:

* Hot Module Replacement (HMR) by utilizing ESM
    
* Faster dev server start speed due to pre-bundling of dependencies
    
* Better control over chunking and overall faster build process due to Rollup bundler
    

# Conclusion and Takeaway:

So is Vite the one-stop solution to all your FE development frustrations?

It's the answer you hate :) ..... It depends!

Well, for the most part, Vite is a brilliant software technology to enhance the overall experience of FE development. However, we cannot disregard Webpack completely.

One interesting occurrence where Vite starts bottling is when there are too many JS files to be loaded (1000+) and although rare, it happens because browsers are not meant to fetch thousands of files, definitely not in seconds, which results in a long load time. If you're keen on learning about this threshold and its viable solution here's the [link](https://betterprogramming.pub/is-vite-really-faster-than-webpack-b414f6cc751c)