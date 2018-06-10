---
theme : "league"
transition: "zoom"
---

<style>
.fragment.current-visible.visible:not(.current-fragment) {
    display: none;
    height:0px;
    line-height: 0px;
    font-size: 0px;
}

p {
  font-size: 24px;
}

</style>

## Exploring GraphQL with React and Node.js

with

*Mihail Subtirelu*  & *Adrian Staniloiu*

<aside class="notes">
  Hello, my name is Subtirelu Mihail, I am a frontend developer at MetroSystems and togheter with my coleague Adrian Staniloiu we will be exploring Graphql with the help of React.js and Node.js
</aside>

---

### Workshop Structure

1.  <div class="fragment">An introduction to GraphQL <br />- *Mihai Subtirelu* </div>

2.  <div class="fragment">
Live coding of a chat app with React and Nodejs
<br />
 - *Adrian Staniloiu*
</div>

<aside class="notes">
The workshop will be structured in 2 parts.
In the first part I will present what GraphQL is and how we can use it and in the second part my coleague Adrian will do a live coding of a chat application that uses GraphQL. On the client he will be using React.js and on the Server he will use
</aside>

---

## GraphQL

<div class="current-visible">
  What is this ?
</div>
<div>
*  <div class="fragment">GraphQL is a *data query language* that describes how to ask for data and a server-side runtime for executing queries</div>
*  <div class="fragment">Can be use both on the client applications and also on the server.</div>
*  <div class="fragment">It isn't tied to any specific database or storage engine and is instead backed by your existing code and data.e</div>
</div>
<br />


<aside class="notes">
  GraphQL is a data query language that describes how to ask for data and a server-side runtime for executing queries <br />
  It was developed by the guys at Facebook in 2012 and release publicly in 2015 under MIT License <br />
  Can be use both on the client applications and also on the server
  <br />
  On the client you use it to request data from the server and on the server you can use it to get the data from multiple sources and send it to the client.
  <br />
  It isn't tied to any specific database or storage engine and is instead backed by your existing code and data.
  <br/ >
</aside>

--

### A new alternative to REST ?

<span>
  *  <div class="fragment">GraphQL and REST are not so different after all</div>
  *  <div class="fragment">GraphQL has many elements of the REST model built in.</div>
  *  <div class="fragment">GraphQL has 3 main characteristics</div>
</span>
<aside class="notes">
  Can we consider GraphQL as an alternative to REST APIs ?
  Well GraphQL and REST are not so different after all.
  <br/>
  GraphQL has many elements of the REST model built in.
  <br/>
  Instead GraphQL has some small changes that make a big difference to the developer experience of building or consuming an API.
</aside>

--

### 1. It uses a type system to describe data

*  The Type System is a description of which types of objects your server can return.

*  The GraphQL type system supports the following kind of types:
    *  Scalar
    *  Object
    *  Interface
    *  Union
    *  InputObject
    *  Enum

<aside class="notes">
The Type System is a description of which types of objects your server can return.
<br />
One of the main advantages of GraphQL is that it enables flexibility in your data model by using the Type System.
<br>
The GraphQL type system supports the following kind of types:
*  Scalar
*  Object
*  Union
*  InputObject
*  Enum
*  Interface - which exposes a certain set of fields that a type must include to implement the interface
</aside>

--

### 2. Aggregating data from multiple sources is less painfull

<span class="fragment">
  <img  height="500" data-src="/rest.gif" alt="Syntax">
</span>
<span class="fragment">
  <img  height="500" data-src="/graphql.gif" alt="Syntax">
</span>

<aside class="notes">
So the most basic idea of REST is the resource. <br/>
Each resource is identified by a URL, and you retrieve that resource by sending a GET request to that URL. <br />
You will likely get a JSON response, since that’s what most APIs are using these days. <br/>
<br/>
On the right side we see that the GraphQL layer lives between the client and one or more data sources (like Facebook, Twitter or other 3rd party apps), receiving client requests and fetching the necessary data according to your instructions.
</aside>

--

### 3. It lets the client specify exactly what data it needs.

<img  height="400" data-src="/guy.png" alt="Syntax">

<aside class="notes">
This means that on the server we can send the data output into whatever shape the client requires and on the client we can request the data in whatever shape we want
<br/>
So we can imagine this example: <br />
I have to do 3 tasks, the first one is to get my clothes from the dry cleaner , the second one is to go to the Post Office and get my package and the third one is to buy food from the grocery store . So I will need to go to 3 different places in order to complete this tasks, right ? Ok.
With GraphQL is the same as having a personal assistant and instead of doing those tasks by myself I will only give him a list of the the addresses of the 3 places I need to visit, and then wait for him to return with what I need.
</aside>

--


### Building Blocks

GraphQL is organized around 4 main building blocks:
<div style="text-align:left">
*  Schema
*  Queries
*  Mutations
*  Resolvers
</div>
<aside class="notes">
The GraphQL API is organized around three main building blocks: the schema, queries, and resolvers.
</aside>

--

### Schema

<span>
<div style="font-size: 26px">
In GraphQL we don’t use URLs to identify what is available in the API, instead we use a GraphQL schema that has its own type language, called the *Schema Definition Language* or SDL.
</div>
</span>
<br/>

```js
type User {
  id: ID!
  name: String!
  username: String
}

type Query {
  user(id: ID!): User!
}

type Mutation {
  user(id: ID!, name: String!): User!
}
```

<aside class="notes">
In GraphQL we don’t use URLs to identify what is available in the API. Instead, we use a GraphQL schema that has its own type language, called the Schema Definition language or SDL.<br />
In its simplest form, GraphQL SDL can be used to define types looking like this:<br/ >
The User type alone doesn’t expose any functionality to client applications, it simply defines the structure of a user model in our application. In order to add functionality to the API, you need to add fields to the root types of the GraphQL schema: Query, Mutation and Subscription.
</aside>

--

### Queries

*  Can traverse related objects and their fields
*  The queries are only used to read data from the server

<div class="fragment" style="width:49%; display: inline-block; float: left">
```js
query {
  user {
    id,
    name
  }
  messages {
    id,
    content
  }
}
```
</div>
<aside class="notes">
With the GraphQL queries we can go through a lot of related objects and their fields and get all the related data in one request, instead of making several roundtrips as we would need in a classic REST architecture.
<br />
Queries should only used to read data from the server
<br />
In this example we want to get the user with its id and name and also hes messages and for each message we want to get the id and the message content
<br />
We will start by declaring a new query using the query keyword, then asking for a field named user and for a field names messages.
So basically the query has exactly the same shape as the result we want to get
</aside>
<span class="fragment" style="width:49%; display: inline-block">
  <span>

  ```json
{
  "data": {
    "user": {
      "id": "a9120sx",
      "name": "John Doe"
    },
    "messages": [
      {
        id: "2391x8",
        content: "Hello World"
      }
    ]
  }
}
  ```
  </span>
</span>

--

### Mutations

*  <div class="fragment">Are a common accepted convention that every operation that includes writes should be sent using a mutation.</div>
*  <div class="fragment">We use mutations to write and read data</div>
<br/>
<span class="fragment">
```
  type Mutations {
    // update the user name
    updateUser(id: ID!, name: String!): User,
    // create a new message
    newMessage(content: String!): Message,
    // delete a message
    deleteMessage(id: ID!): Message
  }
```
</span>


<aside class="notes">
Technically we can modify data on the server using a query. However, the common accepted convention is that every operation that includes writes should be sent using a mutation.
So mutations are really like the Query objects because they also have a type, arguments and resolvers.

</aside>

--

### Resolvers

*  <div class="fragment">Are a collection of functions that are called to  execute the schema fields.</div>
*  <div class="fragment">The collection should have a map of resolvers for each relevant GraphQL Object Type</div>
*  <div class="fragment">It is also called a "resolver map"</div>
<br/>
<span class="fragment">
```js
const resolverMap = {
    Query: {
      user(obj, args, context, info) {
        return find(users, { id: args.id });
      },
    },
    User: {
      messages(author) {
        return filter(messages, { userId: author.id });
      },
    },
};
```
</span>


<aside class="notes">
In the GraphQL schema we already describe all of the fields, arguments, and result types, the only thing left to do is to create a collection of functions that we need call in order to actually execute these fields.
The Resolver functions cannot be included in the GraphQL schema language, so they must be added separately and we should have a map of resolvers for each relevant GraphQL Object Type we defined.
In javascript, they can look like this:
</aside>


---

### Live coding example

<aside class="notes">
Before we continue with the live coding example, I want to mention that at Metrosystems we migrating our client apps to frontend microservices and because we need to have a common query language both on the client and also on the server, GraphQL fits our needs.
Next, my colleague Adrian Staniloiu will show you how we can build small chat app
using GraphQL, React and Nodejs.
</aside>