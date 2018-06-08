---
theme : "black"
transition: "zoom"
---

<style>
.fragment.current-visible.visible:not(.current-fragment) {
    display: none;
    height:0px;
    line-height: 0px;
    font-size: 0px;
}
</style>

## Exploring GraphQL with React and Node.js

with

*Mihail Subtirelu*  & *Adrian Staniloiu*

<aside class="notes">
  Hello, my name is Subtirelu Mihail, I am a frontend developer at MetroSystems and togheter with my coleague Adrian Staniloiu we will to show you a new alternative to REST APIs.
  <br />
  First of all we want to know a little bit about you guys, so we have a few questions: <br/>
  1. First question: How many people here have worked or work with Javascript ?<br/>
      No > Yes: Wow, a lot of you didn't work with Javascript, Oh well, I am sorry for you guys but the examples in this presentation are written in JS. I am sure you will understand what we try to explain :)<br />

  2. Second question: How many of you know what a REST API is ? <br />
      No > Yes: Ok, so basically a REST API defines a set of functions which you can perform requests and receive responses via HTTP protocol such as GET and POST.
      <br />

  3. Ok, and how many of you know about or worked with Nodejs ? <br/>
  Nice!
</aside>


---

## GraphQL

What is this ?

<aside class="notes">
  GraphQL is a data query language that describes how to ask for data. <br />
  It was internally developed by the guys at Facebook in 2012 and release publicaly in 2015
  GraphQL can be use both on the client applications and also on the server. <br/>
  On the client you use it to request data from the server and on the server you can use it to get the data from multiple sources and send it to the client.
  So it can be used as an alternative to the REST APIs and ad-hoc web arhitecture
</aside>


--

```
http://server.io?query={ news{ id, title },user{ id, name } }
```

<img data-src="/syntax.png" alt="Syntax">

<aside class="notes">
  So basically, GraphQL is about asking for specific fields on objects. Let's look at a very simple query and the result we get from it.
</aside>

--

### Why an alternative to REST ?


<aside class="notes">
  GraphQL and REST are not so different after all, but GraphQL has some small changes that make a big difference to the developer experience of building or consuming an API.
</aside>

<span class="fragment">GraphQL has 3 main characteristics:</span>

--

#### It uses a type system to describe data.


```
  type User {
    id: ID!,
    name: String!
  }

  type Post {
    id: ID!,
    title: String!,
    content: String
  }
```

<aside class="notes">

The GraphQL Type system describes the capabilities of a GraphQL server in which it is used to determine if a query is valid and also describes the input types of query variables to determine if values provided at runtime are valid.<br/>
So the core of the GraphQL consists in the this Type schemas.
</aside>


--

#### Aggregating data from multiple sources is less painfull

<span class="fragment">
  <img  height="500" data-src="/diagram.png" alt="Syntax">
</span>


<aside class="notes">
The most basic idea of REST is the resource. Each resource is identified by a URL, and you retrieve that resource by sending a GET request to that URL. You will likely get a JSON response,since that’s what most APIs are using these days. <br/>
So it looks something like this on the right:
<br/>
On the left we can see that the GraphQL layer lives between the client and one or more data sources (like Facebook, Twitter or other 3rd party apps), receiving client requests and fetching the necessary data according to your instructions.
</aside>

--

#### It lets the client specify exactly what data it needs.

<img  height="600" data-src="/guy.png" alt="Syntax">

<aside class="notes">
With GraphQL you can simply ask for what you want and wait for them to return.
<br/>
So it is like going to a fast-food and ordering food (“I want a pizza, a Cola and a cupcake”), and then
you get a small bag with everything you ordered in return
</aside>

--


### Components

3 main building blocks: the Schema, Queries, and Resolvers.

<aside class="notes">
The GraphQL API is organized around three main building blocks: the schema, queries, and resolvers.
</aside>

--

### Schema

Schema Definition Language (SDL)

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
Before starting to build your server, GraphQL requires you to design a schema which in turn defines the API of your server.
GraphQL has its own type language that’s used the write GraphQL schemas
In its simplest form, GraphQL SDL can be used to define types looking like this:
The User type alone doesn’t expose any functionality to client applications, it simply defines the structure of a user model in our application. In order to add functionality to the API, you need to add fields to the root types of the GraphQL schema: Query, Mutation and Subscription.
</aside>

--

### Queries

Query
```js
  query {
    user {
      id
      name
    }
  }
```

<aside class="notes">
The request you make to your GraphQL API is the query, and it looks something like this:
<br />
We’re declaring a new query using the query keyword, then asking for a field named user.
The query has exactly the same shape as the result
</aside>
<span class="fragment">
Response

```json
{
  "data": {
    "user": {
      "id": "a9120sx",
      "name": "John Doe"
    }
  }
}
```
</span>

--

### Mutations

```
        type Mutation {
          user(id: ID!, name: String!): User!
        }
```

<aside class="notes">
Technically we can modify data on the server using a query. However, the common accepted convention is that every operation that includes writes should be sent using a mutation.
Despite that, mutations are really much like the query objects we have been writing so far. They too have a type, arguments and a resolve function.
</aside>

---

### Live example

<aside class="notes">
Next, my colleague Adrian Staniloiu will live code an example of a simple Blog like application
</aside>

