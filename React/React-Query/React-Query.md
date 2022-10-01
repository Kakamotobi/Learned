# TanStack's React Query

## Table of Contents
- [What Is It?](#what-is-it)
  - [Background](#background)
  - [Important Disclaimer](#important-disclaimer)
  - [Use Case Examples](#use-case-examples)
- [Conceptual State of Data](#conceptual-state-of-data)
- [State Management with React Query](#state-management-with-react-query)
  - [React Query Caching](#react-query-caching)
- [Core Concepts](#core-concepts)
  - [Queries (`useQuery`)](#queries-usequery)
  - [Mutations (`useMutation`)](#mutations-usemutation)
  - [Query Invalidation (`queryClient.invalidateQueries`)](#query-invalidation-queryclientinvalidatequeries)
- [React Query Devtools](#react-query-devtools)
- [Concerns](#concerns)
- [Example](#example)

## What Is It?
> React Query is a **server-state** library, responsible for managing asynchronous operations between your server and client | Tanstack

- cf. Redux is a client-state library that is used to store asynchronous data.
- React Query is a React library that provide hooks for fetching, caching, synchronizing and updating data from a server.
### Background
- React itself is unopinionated on how the frontend stores and provides asynchronous data throughout the application (fetching and updating data).
- The most basic approach is to use the browser's `fetch` API in the `useEffect()` hook. And then, manage the received response with the `useState()` hook.
  - This works but it is difficult to implement caching, retries, deduping, etc.
- React Query simplifies your data fetching code and also handles these complex implementations (caching, etc.) out of the box.
- Hence, it might even eliminate the need for a global state management solution (Ex: Redux).
  - *Traditional state management libraries are great for client state but not so great for asynchronous or server state.*
### Important Disclaimer
- React Query does NOT fetch any data for you.
  - You have to use something else like `fetch`, `axios`, `graphql-request` to actually fetch the data.
- React Query is:
  - **an Async State Manager**
    - It manages any form of asynchronous state.
      - i.e. it receives a Promise.
    - It handles loading and error states.
    - It acts as a "global state manager".
      - The `queryKey` is a unique identifier for your particular query. Therefore, calling the query with the same key in different places will result to the same data.
      - This is ideally abstracted away with a custom hook so that the actual data fetching function doesn't have to be accessed twice.
      - Components under the same *QueryClientProvider* will get the same data.
        - Even though multiple components request the same data, React Query **deduplicates** requests so there will only be one network request.
      - Example
        ```ts
        // useUser.ts
        import { useQuery } from "@tanstack/react-query";
        
        export const useUser = () => useQuery(["user"], fetchUser);
        ```
        ```tsx
        // ComponentA.tsx
        import useUser from "./useUser";
        
        function ComponentA() {
          const { data: user } = useUser();
          
          return (
            // ...
          )
        }
        ```
        ```tsx
        // ComponentB.tsx
        import useUser from "./useUser";
        
        function componentB() {
          const { data: user } = useUser(); // Exactly the same data as ComponentA.
          
          return (
            // ...
          )
        }
        ```
  - **a Data Synchronization Tool**
    - Since the one true source of data is the server, once the frontend receives the data, the data is considered "stale" (depending on the nature of the data).
    - React Query provides a way to synchronize the data displayed on the frontend with the actual data in the backend.
### Use Case Examples
#### Preventing data from becoming stale.
- Using `refetchOnWindowFocus: true`, you can refetch data when the user leaves and comes back to the same window. Therefore, data will remain up to date despite the user not staying on the page.
#### Inifinite Scroll Feature
- Use the `useInfiniteQuery()` hook to easily implement infinite scroll, which requires continuously getting data from the server and updating the frontend.
#### Instantly Update UI after Mutating Data in Server
- Use optimistic updates to make changes instantly appear in the UI.
#### Debug
- React Query provides integrated dev tools for developers to debug all data fetching logic.

## Conceptual State of Data
- **fetching**
  - Data is being requested.
- **fresh**
  - Data is fresh.
  - *Data is not refetched even if the component state changes.*
  - *Data is refetched when the page is refreshed.*
- **stale**
  - Data is stale.
  - *Any data that the client received from the server is, in effect, stale data that needs to be updated.*
    - *This is because another user could mutate (Ex: add, edit, delete) the data on the server after you received the data from the server.*
    - *Data is refetched when the component mounts/updates.*
    - *Therefore, cached data is also stale.*
  - **`staleTime`**
    - The time it takes for fresh data to become stale.
    - Default value is 0.
    - The `staleTime` option can be used to change the time.
      - Longer `staleTime` means queries will not refetch their data as often.
- **inactive**
  - If there is no other action in the query or the query instance unmounts, the data remains inactive in the cache for 5 minutes, and then garbage collected.
  - **`cacheTime`**
    - The time it takes for inactive data to stay cached in memory.
    - Default value is 5 minutes.
    - `cacheTime` is irrelevant to `staleTime` in thta `cacheTime` starts the moment the data becomes inactive.
- **delete**
  - Data is removed from the cache by the garbage collector.

## State Management with React Query
- After calling `useQuery` to invoke the query function you defined, React Query stores the data in the **cache** that is globally available (just like the Redux store).
- This caching mechanism is the gist of React Query's state management.
  - React Query caches data for you and give it to you when you need it, even if the data is stale. "Stale data is better than no data" because no data usually means users will perceive the loading spinner as slow. Stale data is temporarily shown while it performs a background refetch to revalidate the data.
### React Query Caching
- The received data is stored in React Query's cache.
  - `staleTime` is an option that sets the time until the fresh data is to be considered stale.
- **Data is cached but is never fresh under the default options (`staleTime` and `cacheTime`).**
  - If `staleTime` is not specified when calling `useQuery`, it assumes cached data is always stale, and therefore will keep refetching the data from the server.
- **Setting `enabled: false` will only call the data fetching function once upon mounting, and prevent the query from retrying.**
  - Since `enabled: false` means that you do not wish to use `useQuery`'s features, you need to manually call it using the `refetch` function, which is included in the returned object from calling `useQuery`.
  - **The `refetch` function does NOT check the cache and just proceeds to send an AJAX request.**
  - **Therefore, do NOT set `enabled: false` if you wish to use cache.**
- Example
  - Data is fresh for 5s. Therefore, data is refetched from the server when 5s have passed after receiving the data.
  - Data remains in the cache forever.
  ```jsx
  const { data } = useQuery("users", getUsers, { // options
    staleTime: 5000,
    cacheTime: Infinity
  });
  ```

## Core Concepts
### Queries (`useQuery`)
> A query is a declarative dependency on an asynchronous source of data that is tied to a unique key. A query can be used with any Promise based method (including GET and POST methods) to fetch data from a server. If your method modifies data on the server, we recommend using Mutations instead. | TanStack

- If there are multiple queries in the same component, React Query runs them in parallel.
- If a query depends on another query, the `enabled` option can be set to wait for the specified query to resolve first.
- Use the [`useQuery`](https://tanstack.com/query/v4/docs/reference/useQuery) hook to subscribe to a query in your component.
- Syntax: `const query = useQuery(queryKey, queryFn, options)`
  - **`queryKey`**
    - A unique key for the query.
    - This key is used by React Query for refetching, caching, and sharing your queries throughout the application.
    - It can be a string or array.
    - *If the query relies on a variable, the variable needs to be included in the array as well.*
      - Example
        ```js
        const { data, isLoading, error } = useQuery(['todos', id], () => axios.get(`http://.../${id}`));
        ```
  - **`queryFn`**
    - **A (data fetching) function that returns a promise, which resolves the data or throws an error.**
  - **`options`**
    - **`enabled`**
      - `true`: automatically refetch data (default).
      - `false`: prevent automatically refetching data.
        - Ex: only fetch data when a button is clicked.
    - **`retry`**
      - `true`: if data fetching failed, keep trying.
      - `false`: if data fetching failed, stop trying.
      - `number`: if data fetching failed, try only `number` times to fetch again.
    - **`staleTime`**
      - `number`: number of milliseconds until fresh data becomes stale (default is 0).
      - `Infinity`: treat data to always be fresh.
    - **`cacheTime`**
      - `number`: number of milliseconds until data stays cached.
      - `Infinity`: always keep the data cached.
    - **`onSuccess: (data) => `**
      - The callback when data fetching succeeded.
    - **`onError: (error) => `**
      - The callback to be executed when data fetching failed.
    - **`onSettled: (data, error) => `**
      - The callback to be executed regardless of success or error in fetching data.
    - **`select: (data) => `**
      - Function to process the data.
      - The returned value in this function will be the shape of the requested data.
    - **`keepPreviousData`**
      - Keep the previous data rendered on the screen while new data is being fetched.
    - **`initialData`**
      - The initial value to use when there is no cached data.
      - You can use values stored in local storage for this value.
  - **Returns**
    - The result returned by `useQuery` contains all the information about the query and usage of the data.
      - Ex: `isLoading`, `isError`, `isSuccess` states to indicate the status of the query.
      - Ex: `error` if the query is in an `isError` state.
      - Ex: `data` if the query is in a `isSuccess` state.
      - Ex: `refetch` to manually refetch data (ignores cache, straight to server).
#### Example
```tsx
function Todos() {
  const { isLoading, isError, data, error } = useQuery(['todos'], fetchTodoList, {
    staleTime: 5000,
    cacheTime: 5000
  });

  if (isLoading) {
    return <span>Loading...</span>
  }

  if (isError) {
    return <span>Error: {error.message}</span>
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map(todo => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```
### Mutations (`useMutation`)
> Unlike queries, mutations are typically used to create/update/delete data or perform server side-effects. | TanStack

- Use the [`useMutation`](https://tanstack.com/query/v4/docs/reference/useMutation) hook to mutate data on the server.
- Syntax: `const mutation = useMutation(mutationFn, options)`
  - **`mutationFn: (variables) => Promise`**
    - **A (data fetching) function that returns a promise, which resolves the data or throws an error.**
    - It accepts a `variables` object.
  - **`options`**
    - **`cacheTime`**
    - **`mutationKey`**
    - **`networkMode`**
    - **`onMutate`**
    - **`onSuccess`**
    - **`onError`**
    - **`onSettled`**
    - **`retry`**
    - **`retryDelay`**
    - **`useErrorBoundary`**
    - **`meta`**
    - **`context`**
  - **Returns**
    - **`mutate: (variables: TVariables, { onSuccess, onSettled, onError }) => void`**
      - The mutation function you can call with variables to trigger the mutation and optionally override options passed to `useMutation`.
      - Does not return the server's response from the data after mutating.
    - **`mutateAsync: (variables: TVariables, { onSuccess, onSettled, onError }) => Promise<TData>`**
      - Similar to `mutate` but returns a Promise resolving with the response.
    - **`status`**, **`isIdle`**, **`isLoading`**, **`isSuccess`**, **`isError`**, **`isPaused`**, **`data`**, **`error`**.
    - **`reset: () => void`**
      - A function to reset the mutation to its initial state.
#### Example
```tsx
function App() {
  const mutation = useMutation(newTodo => {
    return axios.post('/todos', newTodo)
  })

  return (
    <div>
      {mutation.isLoading ? (
        'Adding todo...'
      ) : (
        <>
          {mutation.isError ? (
            <div>An error occurred: {mutation.error.message}</div>
          ) : null}

          {mutation.isSuccess ? <div>Todo added!</div> : null}

          <button
            onClick={() => {
              mutation.mutate({ id: new Date(), title: 'Do Laundry' })
            }}
          >
            Create Todo
          </button>
        </>
      )}
    </div>
  )
}
```
### Query Invalidation (`queryClient.invalidateQueries`)
> Waiting for queries to become stale before they are fetched again doesn't always work, especially when you know for a fact that a query's data is out of date because of something the user has done. For that purpose, the `QueryClient` has an `invalidateQueries` method that lets you intelligently mark queries as stale and potentially refetch them too! | TanStack

- Query Invalidation refers to declaring a query to be invalid, marking the cache to be stale.
  - Invoke a manual invalidation in certain points in the application that you know should declare the data stale.
- Two Effects
  - The query is marked as stale, any `staleTime` configurations specified in `useQuery` are overridden.
  - If the query is currently being rendered via `useQuery`, it will also be refetched in the background.
- When should you invalidate queries?
  - After **mutating** some data, data currently on the screen should also be updated. However, since the query key has not been changed, it needs to be manually refreshed.
#### Example
```tsx
// Invalidate every query in the cache
queryClient.invalidateQueries()
// Invalidate every query with a key that starts with `todos`
queryClient.invalidateQueries(['todos'])
```
#### Query Revalidation (Smart Refetch)
- React Query provides strategic points for triggering a refetch.
- **refetchOnMount**
  - Whenever a component that calls `useQuery` mounts, revalidate.
- **refetchOnWindowFocus**
  - Whenever the user focuses the browser tab, revalidate.
- **refetchOnReconnect**
  - Whenever the user regains network connect after losing it, revalidate.

## React Query Devtools
- React Query Devtools are only included in bundles when `process.env.NODE_ENV === "development"`. Therefore, there is no need to exclude it in production build.
### Installation
```zsh
npm i @tanstack/react-query-devtools
```
### Usage
- Place the Devtools inside the Query Client Provider, as high as possible.
- [Options](https://tanstack.com/query/v4/docs/devtools#install-and-import-the-devtools)

## Concerns
- React Query is focused on keeping things up-to-date. Therefore, be mindful of not overloading the server with lots of network requests (`staleTime` defaults to 0 means a background refresh upon page refresh).
  - React Query cannot deduplicate in these situations.

## Example
1. Instantiate the React Query Client.
2. Provide it in your component tree to let child components be able to fetch data with React Query.
    - Include the ReactQueryDevtools as a child component in order to debug in development.
3. Use the `useQuery(key, fetchFunc)` hook to send the request.
    - If we want to update the data on the server, the `useMutation()` hook is used.
    - When data is written to the server, we can hook into it with the `onSuccess` function and automatically invalidate the query we already made based on the `key`.
      - This tells React Query to invalidate and refetch the original request.
      - We can also tap into the state of this process by referencing `isFetching`.
```jsx
// App.jsx

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

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

export default Things;
```

## Reference
[Overview | TanStack Query Docs](https://tanstack.com/query/v4/docs/overview)  
[Important Defaults | TanStack Query Docs](https://tanstack.com/query/v4/docs/guides/important-defaults?from=reactQueryV3&original=https://react-query-v3.tanstack.com/guides/important-defaults)  
[React Query as a State Manager | TkDodo's blog](https://tkdodo.eu/blog/react-query-as-a-state-manager)  
[React Query in 100 Seconds - YouTube](https://www.youtube.com/watch?v=novnyCaa7To&ab_channel=Fireship)  
