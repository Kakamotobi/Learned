# TanStack's React Query

## Table of Contents
- [What Is It?](#what-is-it)
  - [Background](#background)
  - [Some Features and Use Case Examples](#some-features-and-use-case-examples)
- [Core Concepts](#core-concepts)
  - [Queries](#queries)
  - [Mutations](#mutations)
  - [Query Invalidation](#query-invalidation)
- [Example](#example)

## What Is It?
- React Query is a React library that provide hooks for fetching, caching, synchronizing and updating data from a server.
### Background
- React itself is unopinionated on how the frontend stores and provides asynchronous data throughout the application (fetching and updating data).
- The most basic approach is to use the browser's `fetch` API in the `useEffect()` hook. And then, manage the received response with the `useState()` hook.
  - This works but it is difficult to implement caching, retries, deduping, etc.
- React Query simplifies your data fetching code and also handles these complex implementations (caching, etc.) out of the box.
- Hence, it might even eliminate the need for a global state management solution (Ex: Redux).
  - *Traditional state management libraries are great for client state but not so great for asynchronous or server state.*
### Features and Use Case Examples
#### Preventing data from becoming stale.
- Using `refetchOnWindowFocus: true`, you can refetch data when the user leaves and comes back to the same window. Therefore, data will remain up to date despite the user not staying on the page.
#### Inifinite Scroll Feature
- Use the `useInfiniteQuery()` hook to easily implement infinite scroll, which requires continuously getting data from the server and updating the frontend.
#### Instantly Update UI after Mutating Data in Server
- Use optimistic updates to make changes instantly appear in the UI.
#### Debug
- React Query provides integrated dev tools for developers to debug all data fetching logic.

## Core Concepts
### Queries
- If there are multiple queries in the same component, React Query runs them in parallel.
- If a query depends on another query, the `enabled` option can be set to wait for the specified query to resolve first.
### Mutations
### Query Invalidation

## Example
- Instantiate the React Query Client.
- Provide it in your component tree to let child components be able to fetch data with React Query.
  - Include the ReactQueryDevtools as a child component in order to debug in development.
- Use the `useQuery(key, fetchFunc)` hook to send the request.
  - If we want to update the data on the server, the `useMutation()` hook is used.
  - When data is written to the server, we can hook into it with the `onSuccess` function and automatically invalidate the query we already made based on the `key`.
    - This tells React Query to invalidate and refetch the original request.
    - We can also tap into the state of this process by referencing `isFetching`.

```jsx
// App.jsx

import { QueryClient, ReactQueryDevtools } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      // Components
      <ReactQueryDevtools />
    </QueryClientProvider>
  )
}

export default App;
```
```jsx
// Things.jsx

import { useQuery, useMutation } from "@tanstack/react-query";

const fetchThings = async () => {
  const res = await fetch("/data.json");
  return res.json();
}

const updateThings = async () => {
  // ...
}

function Things() {
  const { data, status, isFetching } = useQuery("things", fetchThings);
  
  const mutation = useMutation(updateThings, {
    onSuccess: () => {
      queryClient.invalidateQueries("things");
    }
  });
  
  if (status === "loading") {
    return <p>Loading...</p>
  }
  
  if (status === "error") {
    return <p>Error!</p>
  }
  
  return (
    <ul>
      {data.map((thing) => (
        <li key={thing.id}>{thing.name}</li>
      ))}
      
      {isFetching && <p>Refreshing your data...</p>}
    </ul>
  )
}
```


## Reference
[Overview | TanStack Query Docks](https://tanstack.com/query/v4/docs/overview)  
