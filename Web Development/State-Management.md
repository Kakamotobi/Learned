# State Management

## Table of Contents
- [What is State?](#what-is-state)
- [State Management ft. React](#state-management-ft-react)
  - [The Issues with State/Data](#the-issues-with-statedata)
  - [Some Solutions to Manage Server State](#some-solutions-to-manage-server-state)
    - [Redux](#redux)
    - [React Query](#react-query)

## What is State?
- State simply refers to the "state" of the program or of a part of it, which, by definition, is prone to change over time.
- It could refer to data (Ex: list of available hotels), authentication state (Ex: "user is logged in"), or a simple UI state (Ex: "modal is open").
### Client State
- State originating from the client.
### Server State
- State originating from the server.

## State Management ft. React
- Aside from frontend-specific state such as UI states, the vast majority of state is state that was received from the server, which is really a cache of state from a database for example.
- Therefore, they are prone to issues common to caching (Ex: cache invalidation).
  - i.e. how do we know when the state is stale and hence should be updated?
- React does not have a built-in way of managing this issue, which explains the plethora of libraries and tools (Ex: Redux, Apollo (for GraphQL), React Query) built around React targeting this issue.
### The Issues with State/Data
- The client gets data to and from the server via HTTP requests over the network. This means that there is some discrepancy between the two (hence, the need for caching).
  - This means that data on the client may be stale, which means it should be refetched for fresh data. However, if there are a lot of components using that data, it means all of them will have to refetch on their own, which is not ideal.
  - The frontend has to deal with loading/error states while fetching data, which means the user may have to stare at the spinner for a while.
- Data fetching by itself cannot solve this problem and the code to handle the issues with state is all on the frontend.
### Some Solutions to Manage Server State
#### Redux
- Tools like Redux have been used for fetching the data and making it accessible by all components.
  - This means that the data, which is server state, is being handled like a client state. The client simply got a "copy" of the most recent version of the data to render, which leaves the data prone to the above mentioned issues.
#### React Query
- Tools like React Query help mitigate the discrepancy between the client and server.
  - It works to keep the client-side cache of the server-side data as fresh as possible with background refetches and render the data as early as possible.
  - You should refrain from putting the data received from `useQuery` in local state because that would mean you have one local copy (no background refetches), and one query cache (background refetches).
  - You should also refrain from using `queryCache` to manage local state. Use React Query for managing asynchronous state (server state).

## Reference
[Remix: The Yang to React's Yin](https://kentcdodds.com/blog/remix-the-yang-to-react-s-yin)  
[Practical React Query | TkDodo's blog](https://tkdodo.eu/blog/practical-react-query)  
[React Query as a State Manager | TkDodo's blog](https://tkdodo.eu/blog/react-query-as-a-state-manager)  
[How to manage server and client state in a web application | parakey](https://www.parakey.co/blog/how-to-manage-server-and-client-state-in-a-web-application)  
