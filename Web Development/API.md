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
        - [Built-in Types](#built-in-types)
      - [Validation](#validation)
      - [Execution](#execution)
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

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/graphql-vs-rest.png" alt="GraphQL vs REST" width="60%" />
</p>

##### Key Pointers
- Multiple requests &rarr; One request.
  - Mitigate underfetching and overfetching.
- Control of the data is handed over to the frontend, which gives more flexibility and efficiency.
#### GraphQL Ecosystem
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Web%20Development/refImg/graphql-ecosystem.png" alt="GraphQL vs REST" width="60%" />
</p>

- **GraphQL Client**
  - Libraries that construct GraphQL queries and send them to the server.
  - Ex: Apollo Client, GraphQL Request, Relay.
- **GraphQL Gateway**
  - A mechanism to find issues with the API quickly and improve performance.
  - It is placed on top of the server or as a standalone proxy to route to the server to provide features including:
    - query execution tracing (complete route of the query).
    - query caching.
    - error tracking.
    - performance tracking.
  - Ex: Apollo Engine.
- **GraphQL Server**
  - A server that receives GraphQL requests and responds with data.
  - Ex: Apollo Server, GraphQL Yoga.
- **Database-to-GraphQL Server**
  - Middleman between the GraphQL server and the database that provides features including:
    - writing SQL queries in your resolvers.
    - simplifying complex database operations.
  - Ex: Prisma.
#### GraphQL Concepts
##### Queries and Mutations
- Queries and Mutations (and Subscription) are operation types available in GraphQL.
  - Queries are interactive and has the same shape as the result.
  - If not using the shorthand syntax, the operation type is required.
- It is possible to mutate existing data and query the new value of the field with one request.
###### Arguments
- Arguments can be passed to fields.
  - Every field can have zero or more arguments (arguments are passed by name specifically; not in an ordered list).
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
- A fragment cannot refer to itself or create a cycle because it could result in an unbounded result.
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
- **Using variables inside fragments.**
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
- **Inline Fragments**
  ```gql
  {
    phone {
      name
      ... on Android {
        isFoldable
      }
    }
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
- The GraphQL Type System describes what data can be queried.
- The point of **Schemas** is to exactly describe the data that can be queried.
  - i.e. what fields are available to select and what objects they return.
  - *Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, the received queries are validated and executed against that schema.*
  - Every GraphQL service has a `Query` type and may or may not have a `Mutation` type.
    ```gql
    # Query

    query {
      phone {
        name
        similarPhones
      }
      laptop(os: "macos") {
        name
      }
    }
    ```
    ```gql
    # The above query means that the GraphQL service has to have a `Query` type with a `phone` field.
    # Define the fields on the `Query`/`Mutation` type to make available as the root fields that can be called in the query.

    type Query {
      phone: Phone
      laptop(os: OS!): Laptop
    }
    ```
###### Built-in Types
- **Scalar Types**
  - Types that resolve to a single scalar object, and cannot have sub-selections in the query.
    - i.e. the leaves of the query.
  - `String`, `Int`, `Float`, `Boolean`, `ID`.
  - Custom scalar types can also be specified.
    - Ex: `scalar Date`.
    - Then, our implementation defines how that type should be serialized, deserialized, and validated.
- **Enumeration Types**
  - Special kind of scalar that is restricted to a particular set of allowed values.
  - Enums can be used to validate that any arguments of this type are one of the allowed values.
  - Example
    - When the type `Phone` is used in our Schema, it is expected to be either one of `ANDROID`, `IOS`, or `BLACKBERRY`.
    ```gql
    enum Phone {
      ANDROID
      IOS
      BLACKBERRY
    }
    ```
- **Object Types**
  - Object types are 
    - *Object types do not include arrays.*
  - *`query` and `mutation` are the same as a regular object types but are only different in that they define the entry point into the Schema.*
- **Lists and Non-Null(`!`)**
  - Scalars, Enums, and Object Types are the only types that can be defined in GraphQL. However, additional *type modifiers* can be applied in other parts of the schema or in query variable declarations.
  - Using the Non-Null type modifier means that the server will always expect to return a non-null value.
  - Using the `[]` type modifier indicates that the field will return an array of the specific type.
  - Example
    ```gql
    type Actor {
      name: String!
      filmography: [Movie]!
    }
    ```
- **Interfaces**
  - An abstract type that includes a certain set of fields that a type must include to implement the interface, and can add additional fields.
  - Useful for when returning an object or set of objects that may be of several different types.
  - Example
    ```gql
    interface Human {
      name: String!
      age: Int!
    }
    
    type SuperHero implements Human {
      name: String!
      age: Int!
      power: String!
      filmography: [Movie]!
    }
    ```
##### Validation
- You can only query for a field that exists on the given type.
  - Ex: if the query `phone` returns a `Phone`, you can query fields on `Phone`.
- A valid query must return a scalar or an enum.
##### Execution
- A GraphQL query cannot be executed without a type system.
- Each **field** in a query is essentially a function/method of the previous type, whih returns the next type.
  - Each field on each type is backed by a function called the resolver (provided by the GraphQL server).
  - When a field executes, the corresponding resolver is called.
  - The execution stops when scalar or enum values are returned.
- During execution, GraphQL waits for Promises to complete before continuing.
###### Root Fields and Resolvers
- At the top level of a GraphQL server, there is a type called the **Root** type or **Query** type that represents all entry points into the API.
- Example
  ```gql
  # Query type with resolver function that accesses DB, constructs and returns an `Actor` object.
  
  Query: {
    actor(obj, args, context, info) {
      return context.db.findByName(args.name).then(
        actorData => new Actor(actorData);
      )
    }
  }
  ```
    - `obj` - the previous object.
    - `args` - the arguments provided to the field in the query.
    - `context` - a value that is provided to every resolver and holds contextual information (Ex: currently logged in user, provide access to DB).
    - `info` - a value that is field-specific relevant to the currenty query, incl. Schema details.
- **List Resolvers Example**
  - Returning a list of Promises.
  - An `Actor` object has a list of ids of his `Film`s. These ids need to be used to get the real Film objects.
  ```gql
  Actor: {
    filmography(obj, args, context, info) {
      return obj.filmIDs.map(
        id => context.db.findFilmByID(id).then(
          filmData => new Film(filmData);
        )
      )
    }
  }
  ```
##### Example
- Type system
  ```gql
  type Query {
    actor(name: String!): Actor
  }
  
  type Actor {
    name: String
    age
    filmography: [Media]
    socialprofiles: [SocialProfile]
  }
  
  enum Media {
    MOVIE
    TVSHOW
    MUSICVIDEO
  }
  
  type SocialProfile {
    platform: String
    accountName: String
    numFollowers: Int
  }
  ```
- Query
  ```gql
  {
    actor(name: "Steve Carell") {
      name
      filmography
      socialprofiles {
        platform
        accountName
      }
    }
  }
  ```
- Response
  ```gql
  {
    "data": {
      "actor": {
        "name": "Steve Carell",
        "filmography": [
          "The Office",
          "The Big Short"
        ],
        "socialProfiles": [
          {
            "platform": "Twitter",
            "accountName": "@SteveCarell"
          }
        ]
      }
    }
  }
  ```
#### How GraphQL Works
1. The backend (GraphQL Server) provides a predefined Schema.
    - The Schema is basically the middle ground between the client and the server as it defines what types of data can be fetched and in what shape, and how to access them.
    - The Schema can be written in human readable SDL or programmatically using a code-first approach.
      - Use the `type` keyword to describe the objects that can be queried on the server and the fields that they have.
2. The GraphQL Server prepares resolvers for all possible GraphQL requests.
    - Each possible request has a corresponding resolver function that executes the necessary task (Ex: fetch data from the DB).
    - On the server, GraphQL also functions as a runtime for executing incoming queries.
3. The frontend (GraphQL Client) assembles a request (GraphQL Operation) based on the predefined Schema and sends it to the server.
4. The GraphQL Server receives the GraphQL Operation, interprets it against the Schema, and resolves the operation accordingly.
    - query &rarr; resolver &rarr; response
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
  "query": `query GetUserName($id: String) { user(id: $id) { name }}`,
  "variables": { "id": "someidstring" }
}

const res = axios({
  url: endpoint,
  method: "get",
  headers: headers,
  data: graphqlQuery
});

console.log(res.data); // name of the user
console.log(res.errors);

// OR

const res = axios.get(endpoint, {
  headers: headers,
  params: {
    query: `GetUserName($id: String) { user(id: $id) { name }}`,
    variables: { "id": "someidstring"},
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

// Approach 1 - using SDL
// `buildSchema` receives a schema in SDL and returns a `GraphQLSchema` object.
// `buildSchema` does not allow you to specify a resolver function for any field in the schema. You have to rely on GraphQL's default resolver behavior (GraphQL tries to find a property on the parent value that matches the name of the field) and pass in a `root` value.
import { buildSchema } from "graphql";

export default buildSchema(`
  // Entity types.
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

// Approach 2 - using the `GraphQLSchema` constructor
import { GraphQLSchema, GraphQLObjectType } from "graphql";
import resolver from "./resolver.js";

// Entity types
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

// Schema Constructor
export default new GraphQLSchema({
  query: new GraphQLObjectType({
    name: "Query",
    fields: {
      videos: {
        type: [Video],
        args: {}, // the arguments that the `videos` query accepts
        resolve: () => resolver.videos(),
      },
      creator: {
        type: creator,
        args: {
          id: { type: GraphQLString },
        },
        resolve: (obj, {id}) => resolver.creator(id),
      },
    },
  }),
});
```
```js
// resolver.js
// A resolver is a function that is responsible for populating the data for a single field in your schema.
// You can fetch it from your own DB, or a third-party API.
// These resolvers run against the schema, hence, the top-level fields (Ex: "Query", "Mutation") should correspond to the types defined in the schema. And each resolver function belongs to whichever type its corresponding field belongs to. i.e. names should match.

export default {
  Query: {
    videos: (obj, args, context, info) => {
      return context.db.getVideos(args.id);
    },
    creator: (obj, args, context, info) => {
      return context.db.getCreator(args.id);
    }
  },
  
  Mutation: {
    createVideo: ({ url }) => {
      // create the video.
      // return a Video.
    },
    deleteVideo: ({ url }) => {
      // delete the video.
      // return a String.
    }
  } 
}
```
- Every resolver receives these arguments.
  - `obj`: the previous object.
    - a.k.a. `root`, `parent`.
  - `args`: all the arguments provided to the field in the query.
  - `context`: a ***mutable object*** (execution context) that is provided to all resolvers. It holds information such as the currently logged in user, or access to a DB.
    - `context` is created and destroyed between every request.
    - It is great to store common auth data, models/fetchers for APIs and DBs, etc.
      - Ex: store Express' `req` in `context`.
    - It should not be used as a general purpose cache since the order of invocation of functions is not guaranteed. Therefore, avoid mutating context inside of resolvers.
  - `info`: field-specific information (also includes schema details) relevant to the query.

```js
// Endpoint
// Pass the Schema definition and Resolvers to the GraphQL server constructor you are using (Ex: ApolloServer).

import express from "express";
import { graphqlHTTP } from "express-graphql";
import schema from "./schema.js";
import resolver from "./resolver.js";

const app = express();

app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    resolver,
    graphiql: true
  })
);

app.listen(3000);
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
[GraphQL: Core Features, Architecture, Pros and Cons | AltexSoft](https://www.altexsoft.com/blog/engineering/graphql-core-features-architecture-pros-and-cons/)  
[How to request a GraphQL API with Fetch or Axios](https://hasura.io/blog/how-to-request-a-graphql-api-with-fetch-or-axios/)  
[Queries and Mutations | GraphQL](https://graphql.org/learn/queries/)  
[Constructing Types | GraphQL](https://graphql.org/graphql-js/constructing-types/)  
[Resolvers - Apollo GraphQL Docs](https://www.apollographql.com/docs/apollo-server/data/resolvers/)  
[GraphQL Resolvers: Best Practices | by Mark Stuart | The PayPal Technology Blog | Medium](https://medium.com/paypal-tech/graphql-resolvers-best-practices-cd36fdbcef55) 
[graphql - Notable differences between buildSchema and GraphQLSchema? - Stack Overflow](https://stackoverflow.com/questions/53984094/notable-differences-between-buildschema-and-graphqlschema)  
