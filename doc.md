// Q: where is __remixManifest actually used by client?

Inventing Remix from first principles

HTML
CSS
JS
static assets like favicon, fonts, images, etc...

- SSR means HTML is not statically defined, but rather server rendered.
- let's code-split the JS
- let's fingerprint the assets

Since Remix is doing SSR, we have a server.
In theory, this means we can preload resources for the initial server render and prefetch them for client-side transitions.
But to do that, the server needs to know all the dependencies for each route.

That's what the asset manifest is all about.
Conceptually, its just a dependency graph that says what all the (recursive) dependencies of a given route.

Now we just need the server to use this manifest and to hydrate loaded routes.
That's what Remix does!

But we also want the server to be yours to control.
So that's why `entry.server.tsx` exists.

```ts
remixRequestHandler = createRemixRequestHandler(serverEntry)
remixServer = createServer(remixRequestHandler)
remixServer(assetsDirectory, assetManifest)
```

Start with a assets directory (often called a public web folder) that includes anything needed to run an app in the browser, like: static assets, fingerprinted asset imports, code-split JS.
- 

For Remix to do its thing, it just needs a couple things:
1. A browser build. This includes automatic code splitting by route, fingerprinted asset imports (like CSS and images), etc. Anything needed to run an application in the browser
2. 

Remix needs to know what your routes are, and specifically _what should be fetched_ for each route.
This let's Remix avoid the network waterfall, both by preloading resources for the initial server render and for prefetching them for client side transitions.

To keep track of this information, Remix depends on its asset manifest.
The asset manifest

It also has some implementation details encoded in it, like the version, manifest url, etc..

// from current docs
An asset manifest.
Both the client and the server use this manifest to know the entire dependency graph.
This is useful for preloading resources in the initial server render as well as prefetching them for client side transitions.
This is how Remix is able to eliminate the render+fetch waterfalls so common in web apps today.