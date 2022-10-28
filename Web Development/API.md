# Application Programming Interface(API)

## Table of Contents
- [What is an API?](#what-is-an-api)
- [Types of Web APIs](#types-of-web-apis)
- [API Paradigms](#api-paradigms)
  - [Simple Object Access Protocol(SOAP)](#simple-object-access-protocolsoap)
  - [REpresentational State Transfer(REST)](#representational-state-transferrest)
    - [How REST Works](#how-rest-works)
  - [GraphQL](#graphql)
    - [GraphQL vs. REST API](#graphql-vs-rest-api)
    - [GraphQL Concepts](#graphql-concepts)
      - [Queries and Mutations](#queries-and-mutations)
        - [Arguments](#arguments)
        - [Aliases](#aliases)
        - [Fragments](#fragments)
        - [Variables](#variables)
        - [Directives](#directives)
      - [Schemas and Types](#schemas-and-types)
    - [How GraphQL Works](#how-graphql-works)
- [Reference](#reference)

## What is an API?
- **A software intermediary/mechanism (interface), with set rules, that allows two applications to interact with each other.**
  - i.e. a type of software interface, that offers a service to other softwares.
- "Application" refers to any piece of software with a distinct function, which can be a whole server/app or a small part of an app.
- "Interface" refers to the predetermined way of using requests and responses to communicate.
- API documents describe how to compose and send requests and responses.
### Elaboration
- Every web page visit is an interaction with some remote server’s API.
- An API is not the remote server itself - rather, it is the part of the server that receives requests and sends responses.
  - To render the whole web page, the browser/client expects a response in HTML.
  - For a specific part of an app, the browser/client could expect simply data.
- *API is essentially a waiter that takes the customer's (client) order (request) to the kitchen (system), and then delivers the food (response) back to the customer.*
- When people say "API", they are usually referring to Web API that exposes an application's data and functionality.
- It is a way to access a resource in an organized manner using neater URLs.
### How an API Call Works
1. A client application makes an API call (request) to an endpoint of a web server.
2. The API receives the request and relays it to the server.
3. The server sends a response with the requested information to the API.
4. The API delivers the payload back to the client application.

## Types of Web APIs
- Open APIs (a.k.a. Public APIs)
- Partner APIs
  - Constructs built into third-party platforms that allows us to use some of those platform's functionality in our own web pages.
  - Ex: Google Maps
- Internal APIs
- Composite APIs
- Browser APIs
  - Browsers come with APIs, which are methods that can be called from JS.
  - The JS call stack recognizes these web API functions and passes them off to the browser to take care of. Once the browser finishes those tasks, they return and are pushed onto the stackas a callback.
  - Ex: `setTimeout()`, web audio API (audio manipulation).
  - [More browser APIs](https://developer.mozilla.org/en-US/docs/Web/API)

## API Paradigms
- There are a few existing ways to define the architecture of APIs.
- They are different ways in allowing external entities to access data.
### Simple Object Access Protocol(SOAP)
- Introduced in 1998.
- SOAP is a message protocol for exchanging information in a decentralized, distributed environment.
- The client and server communicate using XML usually over HTTP in order to invoke something on the service.
- It was used more back in the days, but is now lacking flexibility.
### REpresentational State Transfer(REST)
- Introduced in 2000.
- REST is an architectural style for designing APIs.
- Data entities or resources are organized into unique URIs and made available on a server to which the client can access the resource from.
- REST is stateless. Therefore, the client and the server do not have to store any information about each other, and every request/response cycle is independent.
- RESTful API simply means that the API follows the REST architecture.
- It is currently the most prevalent API.
#### How REST Works
- A client and server communicate with each other using data (usually in JSON format) over HTTP.
  - HTTP methods are used to perform CRUD actions.
  - The client sends a request to one of the endpoints on the server via HTTP.
    - The request contains:
      - an operation type (HTTP method).
      - an endpoint.
      - some headers (metadata about the request).
        - Ex: `Accept` header to inform the server of the data format that the client wishes to receive.
        - Ex: `Authorization` header to inform the server that you have permission to make that request.
      - a body/payload.
  - The server receives the client's request and responds with the corresponding data/action.
    - The response contains:
      - an  HTTP status code.
      - some headers (metadata about the response).
      - a body/payload if any (typically in JSON).
### GraphQL
- Introduced in 2015.
- It is a query language that allows the frontend (application) to communicate with the backend (API).
- It prioritizes providing only the data that the client requested.
- It serves as the single endpoint that is able to query to multiple databases, microservices, and APIs.
- GraphQL gives frontend developers more flexibility in structuring queries, and it is up to backend developers to write code to resolve the queries.
- The Apollo Client is the most popular way to use GraphQL in the frontend. However, you can use other libraries such as React Query.
  - Apollo Client is a state management library that allows you to write GraphQL queries, then see the results automatically update in your UI.
- GraphQL is language agnostic.
  - i.e. frontend and backend can each use any language.
#### GraphQL vs. REST API
- For REST APIS, each request is mapped to its own unique endpoint, and each response from the server will contain the full JSON payload with everything that is needed.
  - Excess/unnecessary data is just filtered out in the frontend code.
- For GraphQL, instead of having multiple endpoints, there is a single entry point into the API.
  - Therefore, the actual query sent from the frontend determines what the backend will return (not based on an endpoint or any specific mapping like REST).
  - Only the requested fields of data is returned, despite requesting for multiple different resource entities.
#### GraphQL Concepts
##### Queries and Mutations
- Queries and Mutations (and Subscription) are operation types available in GraphQL.
  - Queries are interactive and has the same shape as the result.
  - If not using the shorthand syntax, the operation type is required.
- It is possible to mutate existing data and query the new value of the field with one request.
###### Arguments
- Arguments can be passed to fields.
- Unlike in REST where only a single set of arguments can be passed in (query parametsers and URL segments), GraphQL allows every field and nested object to have its own set of arguments. This allows to make a single API fetch request as to multiple round trips.
- Example
  ```gql
  # Query content

  {
    person(id: "idforkakamotobi") {
      name
      age
    }
  }
  ```
  ```json
  // Response

  {
    "data": {
      "person": {
        "name": "Kakamotobi",
        "age": 3
      }
    }
  }
  ```
###### Aliases
- It is not possible to directly query for the same field with different arguments because the fields in the result object matches the name of the fields in the query.
- Aliases allow you to rename the result of a field, without changing the original schema.
- Example
  - `appleDevices` is an array of all apple devices (Ex: `[{ name: "iPhone 14", ... }, { name: "MacBook Pro 16", ... }]`).
  ```gql
  # This throws an error.
  # "Fields 'appleDevices' conflict because they have differing arguments. Use different aliases on the fields to fetch both if this was intentional."
  
  {
    appleDevices(type: "phone") {
      name
      chip
      price
    }
    appleDevices(type: "mac") {
      name
      chip
      price
    }
  }
  ```
  ```gql
  # Use aliases
  
  {
    phones: appleDevices(type: "phone") {
      name
      chip
      price
    }
    macbooks: appleDevices(type: "macbook") {
      name
      chip
      price
    }
  }
  ```
  ```json
  // Response
  
  {
    "data": {
      "phones": [
        {
          "name": "iPhone 14",
          "chip": "A15",
          "price": 1250000
        },
        // ...
      ],
      "macbooks": [
        {
          "name": "Macbook Pro 16",
          "chip": "M1 Pro",
          "price": 2690000
        },
        // ...
      ]
    }
  }
  ```
###### Fragments
- GraphQL includes reusable units called Fragments, which allow you to construct sets of fields and then include them in queries.
- Example
  - Comparing two (or more) iPhones.
  ```gql
  {
    leftComparison: iphones(model: "iPhone 14") {
      ...comparisonFields
    }
    
    rightComparison: iphones(model: "iPhone 14 Pro") {
      ...comparisonFields
    }
  }
  
  fragment comparisonFields on Phone {
    name
    chip
    price
  }
  ```
  ```json
  // Response
  
  {
    "data": {
      "leftComparison": {
        "name": "iPhone 14",
        "chip": "A15",
        "price": 1250000
      },
      "rightComparison": {
        "name": "iPhone 14 Pro",
        "chip": "A16",
        "price": 1550000
      }
    }
  }
  ```
- Using variables inside fragments.
  ```gql
  query PhoneComparison($releaseYear: Int = 2022) {
    leftComparison: iphones(model: "iPhone 14") {
      ...comparisonFields
    }
    
    rightComparison: iphones(model: "iPhone 14 Pro") {
      ...comparisonFields
    }
  }
  
  fragment comparisonFields(releaseYear: $releaseYear) on Phone {
    name
    chip
    price
  }
  ```
###### Variables
- Arguments to fields will most likely be dynamic.
- It is not good to pass these dynamic arguments directly in the query string because that means the client-side will have to dynamically manipulate and serialize the query string into a GraphQL format at runtime.
  - **Do NOT use string interpolation (template literals) to construct queries from user-supplied values.**
- Instead, GraphQL provides a way to factory dynamic values out of the query, and pass them as a separate dictionary.
  - All arguments must be either scalars, enums, or input object types.
- Steps to use Variables
    1) Replace the static value in the query with `$variableName`.
    2) Declare `$variableName` as one of the variables accepted by the query.
    3) Pass `variableName: value` in the separate, transport-specific (usually JSON) variables dictionary.
- Example
  ```gql
  query Phones($releaseYear: Int = 2022) { # $variableName: type = defaultValue
    phone(releaseYear: $releaseYear) {
      name
      chip
      price
    }
  }
  ```
###### Directives
- Directives allow us to dynamically change the structure and shape of the query using variables.
  - Ex: summarized view and detailed view (more fields) of movie.
- A directive can be attached to a field or fragment inclusion.
- There are two directive:
  - `@include(if: Boolean)` - include this field in the result if the argument is `true`.
  - `@skip(if: Boolean)` - skip this field if the argument is `true`.
- Example
  ```gql
  query Phones($releaseYear: Int, $withSimilarPhones: Boolean!) {
    phone(releaseYear: $releaseYear) {
      name
      chip
      price
      similarPhones @include(if: $withSimilarPhones) {
        name
      }
    }
  }
  ```
##### Schemas and Types


#### How GraphQL Works
1. Define a Schema for your data using the `type` keyword.
2. Inform GraphQL how to fetch (query) and supply that Schema.
    - A query has the same shape that is expected to receive back from the API as JSON.
    - On the server, GraphQL also functions as a runtime for executing incoming queries.
      - The server defines types that are available and resolvers that actually fetch the data from a data source (Ex: DB).
##### GraphQL Request (Frontend)
- A GraphQL request consists of:
  - the **GraphQL Endpoint** - the single endpoint on the server.
  - **Headers** - the relevant authorization headers if required.
  - **Request Body** - the request body, which is usually JSON.
    - **`"operationName"`**
      - Optional, but if included, has to be included in the `query`.
    - **`"query"`**
      - The full GraphQL query containing the operation type (`query` or `mutation`), the types and fields being requested, and any variables included.
      - Example
        ```gql
        query getPlayers {
          player {
            name
            email
            stats {
              numWins
              numLosses
            }
          }
        }
        ```
    - **`"variables"`**
      - Optional if there are no variables included in the `query`.
    - Example
      ```json
      {
        "operationName": "GetUserNameAndEmail",
        "query": "query GetUserNameAndEmail($id: String) { user(id: $id) { name email } }",
        "variables": { "id": "someidstring" }
      }
      ```
###### Example
```js
import axios from "axios";

const endpoint = "iamgraphqlendpointontheserver";

const headers = {
  "content-type": "application/json",
  "Authorization": "<token>"
}

const graphqlQuery = {
  "operationName": "GetUserName",
  "query": `query GetUserName() { user { name }}`,
  "variables": {}
  
}

const res = axios({
  url: endpoint,
  method: "post",
  headers: headers,
  data: graphqlQuery
});

console.log(res.data); // name of the user
console.log(res.errors);

// OR

const res = axios.get(endpoint, {
  headers: headers,
  params: {
    query: `GetUserName() { user { name }}`,
    variables: {},
    operationName: "GetUserName"
  }
});
```
##### GraphQL Server (Backend)
- Define the schema.
- Resolve requests received from the client.
###### Example
```js
// schema.js

import { buildSchema } from "graphql";

export default buildSchema(`
  type Creator {
    id: ID!
    name: String
    numSubscribers: Int
    videos: [Video]
  }

  type Video {
    url: String!
    creator: Creator
  }
  
  // The entry point for the API's consumer to read data.
  type Query {
    videos: [Video]
    creator(id: String!): Creator
  }
  
  // The entry point for the API's consumer to mutate data.
  // Defines how data can be modified on the API.
  type Mutation {
    createVideo(url: String): Video
    deleteVideo(url: String): String
  }
`)
```
```js
// resolver.js

export default {
  createVideo: ({ url }) => {
    // create the video.
    // return a Video.
  }
  
  deleteVideo: ({ url }) => {
    // delete the video.
    // return a String.
  }
}
```
```js
// Endpoint

import schema from "./schema.js";
import resolver from "./resolver.js";

app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    resolver,
    graphiql: true
  })
)
```

## Reference
[What is an Application Proramming Interface (API) | IBM](https://www.ibm.com/cloud/learn/api)  
[Introduction to web APIs - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)  
[API란 무엇인가요? - API 초보자를 위한 가이드 - AWS](https://aws.amazon.com/ko/what-is/api/)  
[SOAP - IBM Documentation](https://www.ibm.com/docs/en/wasdtfe?topic=applications-soap)  
[What is a REST API? | IBM](https://www.ibm.com/cloud/learn/rest-apis)  
[RESTful APIs in 100 Seconds // Build an API from Scratch with Node.js Express - YouTube](https://www.youtube.com/watch?v=-MTSQjw5DrM&ab_channel=Fireship)  
[GraphQL Basics - Build an app with the SpaceX API - YouTube](https://www.youtube.com/watch?v=7wzR4Ig5pTI&ab_channel=Fireship)  
[The Ultimate Guide to API Architecture: REST, SOAP or GraphQL? | DA-14](https://da-14.com/blog/ultimate-guide-api-architecture-rest-soap-or-graphql)  
[SOAP vs REST vs gRPC vs GraphQL - DEV Community](https://dev.to/andreidascalu/soap-vs-rest-vs-grpc-vs-graphql-1ib6)  
[How to request a GraphQL API with Fetch or Axios](https://hasura.io/blog/how-to-request-a-graphql-api-with-fetch-or-axios/)  
[Queries and Mutations | GraphQL](https://graphql.org/learn/queries/)  
