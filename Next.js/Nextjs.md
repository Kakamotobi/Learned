# Next.js

## Table of Contents
- [What is Next.js?](#what-is-nextjs)
- [Client-side vs Server-side Rendering](#client-side-vs-server-side-rendering)
- [Static vs Dynamic Rendering vs Streaming](#static-vs-dynamic-rendering-vs-streaming)
- [Routing and Navigation](#routing-and-navigation)
- [Caching and Revalidation](#caching-and-revalidation)
- [Reference](#reference)

## What is Next.js?
> Next.js is a React framework for building full-stack web applications. You use React Components to build user interfaces, and Next.js for additional features and optimizations. | Next.js
### Main Features
- **Routing**
  - Provides a file-system based router built on top of React Server Components(RSC).
- **Rendering**
  - Provides Client-side and Server-side Rendering with Client/Server components.
  - Rendering is further optimized with Static/Dynamic Rendering on the server.
- **Data Fetching**
  - Data fetching is possible in RSC since they can be asynchronous.
  - Provides an extended `fetch` API for memoizing requests, and caching and revalidating data.
- **Styling**
  - Supports CSS Modules, Tailwind, CSS-in-JS.
  - Note: the target CSS-in-JS libraries must support Next.js.
- **Optimizations**
  - Provides images, fonts, scripts optimizations to improve performance and user experience.
- **TypeScript**
  - Provides improved support for TS (type checking, compilation, etc).

## Client-side vs Server-side Rendering
### Client-side Rendering
1. The Server sends a minimal HTML document to the Client.
2. The Client requests and receives JS files and executes them.
3. The Client makes necessary data requests to the Server.
4. The Client constructs the components and renders the UI. - TTV, TTI
- _Notes_
  - To use Client Components, we need to opt-in to it by including the `"use-client"` directive. By doing so, all imported modules including child components are part of the client bundle.
    - `"use-client"` is essentially the boundary between a Server Component and Client Component (including its child components).
  - Any use of browser APIs (Ex: event listeners, hooks) require this.
  - [How client components are rendered](https://nextjs.org/docs/app/building-your-application/rendering/client-components#how-are-client-components-rendered)
### Server-side Rendering
<div align="center">
  <img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2" alt="Server-side Rendering" width="75%" />
  <p><i>Next.js Docs</i></p>
</div>

1. All data for the given page is fetched on the Server.
2. The Server then renders the HTML for the page.
3. The Server sends the HTML, CSS, JS for the page to the Client.
4. The Client renders a non-interactive user interface using the generated HTML and CSS. - TTV
5. React hydrates the user interface to make it interactive. - TTI
- _Notes_
  - These steps are sequential and blocking.
  - Server Components can be rendered in three ways: Static, Dynamic, Streaming.
    - [Server-side Rendering Strategies](https://nextjs.org/docs/app/building-your-application/rendering/server-components#server-rendering-strategies)

## Static vs Dynamic Rendering vs Streaming
- By default, Next.js applies Static Rendering.
- Next.js opts out of Static Rendering and instead Dynamically Renders the whole route if it discovers a dynamic function (functions that rely on information that can only be known at request time. Ex: `cookies`, `searchParams`) or if data is specified to not be cached (Ex: `cache: “no-store”` in fetch request).

<div align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Next.js/refImg/static-dynamic-rendering-conditions.png" alt="Static vs Dynamic Rendering Conditions" width="75%" />
  <p><i>Next.js Docs</i></p>
</div>

- [Dynamic Functions](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-functions)
- [Opting out of Data Caching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#opting-out-of-data-caching)
- _Note_
  - Next.js chooses between Static and Dynamic Rendering automatically based on the features and APIs used in each route. As developers, we only need to choose when to cache or revalidate specific data, and whether and what parts of the UI to stream.
  - A lot of the times, routes are not fully static or fully dynamic.
    - Ex: a product page may use both cached product data and uncached personalized customer data.
    - You can have dynamically rendered routes that have both cached and uncached data; since RSC payload and data are cached separately.
### Static Rendering
- **Routes are rendered at build time or in the background after data revalidation.**
- The result is cached and can be pushed to a CDN.
- Useful for routes with unpersonalized data (Ex: product page).
- For static routes, the entire route is prefetched and cached.
  - `prefetch` defaults to `true`.
### Dynamic Rendering
- **Routes are rendered at request time for each user.**
- Useful for routes with personalized data or data that can only be known at request time (Ex: cookies, URL search params).
- Dynamically rendered routes can have both cached and uncached data.
    - This is possible because RSC payload and data are cached separately.
- For dynamic routes, only the shared layout is prefetched and cached for 30s.
    - `prefetch` defaults to `automatic`.
### Streaming
- Streaming allows us to render UI from the Server and send them to the Client as they become ready. Therefore, users will be able to view and interact with parts of the page before the entire page finishes rendering.
- A component can be considered a "chunk" in the stream.
- While waiting for the component to be streamed, we can show a loader.
  - `loading.tsx` file at the page level.
  - `<Suspense>` at the component/page level.
    - The App Router supports streaming with `Suspense`.
    - Wrapping dynamic components with Suspense basically allows us to stream specific components.
      - Ex: unlike `ComponentA`, `ComponentB` is streamed.
        ```tsx
        <div>
          <ComponentA />
          <Suspense>
            <ComponentB />
          </Suspense>
        </div>
        ```
 
<div align="center">
  <img src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fserver-rendering-with-streaming-chart.png&w=3840&q=75&dpl=dpl_4ykYFHvrysxFPMKSgipbGVFm9BQ2" alt="Streaming" width="75%" />
  <p><i>Next.js Docs</i></p>
</div>

## Routing and Navigation
- **On the Server, your application code is automatically code-split by route segments.** Therefore, only the code needed for the current route is loaded on navigation.
  - Code-splitting refers to breaking down your source code into small bundles to be downloaded and executed by the browser. Thereby, the amount of data transferred and execution time for each request is reduced.
- **On the Client, the route segments are prefetched and cached.**
  - Prefetched means to preload a route in the background before the user visits it.
  - Therefore, when a user navigates to a new route, the browser doesn’t reload the page. Instead, only the route segments that change are re-rendered.
  - When `<Link>` components become visible in the viewport, the corresponding pages are prefetched.
    - The entire route is prefetched for Static Routes.
    - Only the shared layout is prefetched and cached for 30s for Dynamic Routes.
  - `router.prefetch()` provided by `useRouter` can be used manually.
  - _Note_
    - Prefetching is only enabled in production.

## Caching and Revalidation
- Next.js maintains two different caches: Router Cache and Data Cache.
### Router Cache
- **The Router Cache is an in-memory cache on the Client.**
- The Router Cache stores the RSC payload of prefetched route segments and visited routes.
- By default, Next.js in production pre-renders and caches all components in the Router Cache.
  - Since it gets the cached component containing stale data, it doesn't fetch new data upon CRUD. It simply uses the pre-generated component from the Router Cache.
  - _This means that we need to tell Next.js to revalidate this data cache being used by this route segment._
- [Router Cache](https://nextjs.org/docs/app/building-your-application/caching#router-cache)
### Data Cache
- **The Data Cache is a persistent HTTP cache on the Server.**
- By default, values returned from `fetch` are cached in the Data Cache (Ex: `fetch('https://...', { cache: 'force-cache' })` by default).
- Data can be fetched at build time or request time, cached, and reused on each data request.
- [Data Cache](https://nextjs.org/docs/app/building-your-application/caching#data-cache)
- [Opting out of Data Caching](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#opting-out-of-data-caching)
#### Revalidation
- When an error occurs upon revalidation, the previous cached data will be served. Next.js will try revalidating on the next request.
##### Time-based Revalidation
- Automatically revalidates data after the specified amount of time.
- Useful for data that doesn't change frequently and being fresh is not significant.
- Example
  - Individual `fetch`.
    ```ts
    fetch('https://...', { next: { revalidate: 3600 } })
    ```
  - Segment Config Options
    ```ts
    // layout.js or page.js
    
    export const revalidate = 3600;
    ```
##### On-Demand Revalidation
- Manully revalidate data based on an event (Ex: form submit).
- Trigger these revalidations inside a Server Action or Route Handler.
- Useful for ensuring latest data.
- **`revalidatePath`**
  - Revalidates Data Cache used by this route path.
  - Ex: `revalidatePath(“/my-profile”)` revalidates the Router Cache belonging to this route path.
    - _`This doesn’t include nested paths._
    - To also revalidate nested paths, do `revalidatePath(“/my-profile”, “layout”)`.
- **`revalidateTag`**
  - Revalidates Data Cache by the cache tag.
  - Example
    ```tsx
    // Assign tags to the cache.
    async function Component() {
      const res = await fetch('https://...', { next: { tags: ["collection"] } });
      const data = await res.json();
    }
    ```
    ```ts
    // Server Action
    "use-server";

    import { revalidateTag } from 'next/cache'
    
    export default async function action() {
      revalidateTag("collection")
    }
    ```

## Reference
[Docs | Next.js](https://nextjs.org/docs)  
