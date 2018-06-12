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

## Exploring GraphQL with React and Nodejs

with

*Mihail Subtirelu*  & *Adrian Staniloiu*

<aside class="notes">
  Hello and thank you for joining or workshop, my name is  Mihail Subtirelu, I am a frontend developer at MetroSystems and togheter with my coleague Adrian Staniloiu we will be exploring Graphql with the help of React.js and Node.js
</aside>

---

### Workshop Structure

1.  An introduction to GraphQL <br />with *Mihai Subtirelu*

2.  Implementing a chat app with React and Nodejs
<br />
with *Adrian Staniloiu*

2. Question and Answers

<aside class="notes">
This workshop will be structured in 3 parts.<br/>
In the first part I will present what GraphQL is and how we can use it and in the second part my coleague Adrian will do a live coding of a chat application that uses GraphQL.
</aside>

---

## GraphQL

<div class="current-visible">
  What is this ?
</div>
<div>
*  <div class="fragment">GraphQL is a *data query language* that describes how to ask for data</div>
*  <div class="fragment">Can be use both on the client applications and also on the server.</div>
*  <div class="fragment">It isn't tied to any specific database or storage engine and is instead backed by your existing code and data</div>
</div>
<br />


<aside class="notes">
  GraphQL is a data query language that describes how to ask for data <br />
  It was developed by Facebook in 2012 and release publicly in 2015 under MIT License <br />
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
  <br/>
  So GraphQL has 3 main characteristics:
</aside>

--

### 1. It lets the client specify exactly what data it needs.

<img  height="400" data-src="/guy.png" alt="Syntax">

<aside class="notes">
This means that on the server we can send the data output into whatever shape the client requires and on the client we can request the data in whatever shape we want
<br/>
So we can imagine this example: <br />
I have to do 3 tasks, the first one is to get my clothes from the dry cleaner , the second one is to go to the Post Office and get my package and the third one is to buy food from the grocery store . So I will need to go to 3 different places in order to complete this tasks, right ? Ok.
With GraphQL is the same as having a personal assistant and instead of doing those tasks by myself I will only give him a list of the the addresses of the 3 places I need to visit, and then wait for him to return with what I need.
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
So in the left image you can see a standard Rest API.
The most basic idea of REST is the resource. <br/>
Each resource is identified by a URL, and you retrieve that resource by sending a GET request to that URL. <br />
You will likely get a JSON response, since that’s what most APIs are using these days. <br/>
<br/>
On the right side we see that the GraphQL layer lives between the client and one or more data sources (like Facebook, Twitter or other 3rd party apps), receiving client requests and fetching the necessary data according to your instructions.
</aside>

--

### 3. It uses a type system to describe data

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


### Building Blocks

GraphQL is organized around 4 main building blocks:
<div style="text-align:left">
*  Schema
*  Queries
*  Mutations
*  Resolvers
</div>
<aside class="notes">
The GraphQL API is organized around four main building blocks: the schema, queries, mutations, and resolvers.
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
With the GraphQL queries we can go through a lot of related objects and their fields and get all the requested data in a single request, instead of making several roundtrips as we would need in a classic REST architecture.
<br />
Queries should only be used to read data from the server
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
Technically we can modify data on the server using a query. <br/>
However, the common accepted convention is that every operation that includes writes should be sent using a mutation.<br />
So mutations are really like the Query objects because they also have a type, arguments and resolvers.<br/>
As you can see we defined a mutation type that has the following mutation:
updateUser, newMessage, deleteMessage.
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
In the GraphQL schema we already describe all of the fields, arguments, and result types, the only thing left to do is to create a collection of functions that we need call in order to actually execute these fields.<br/>
The Resolver functions cannot be included in the GraphQL schema language, so they must be added separately and we should have a map of resolvers for each relevant GraphQL Object Type we defined.<br/>
In javascript, they can look like this:
</aside>

<aside class="notes">
So, just to wrap things up, at Metrosystems we are migrating our current client apps to frontend microservices arhitecture and because we need to have a common query language both on the client and also on the server this is way GraphQL fits our needs. <br/>
Next up, my colleague Adrian Staniloiu will show you how we can build small chat appusing GraphQL, React and Nodejs.
</aside>

---


### Realtime Chat app

https://github.com/adrianstaniloiu/nodejs-graphql-react-chat


--


<aside class="notes">
Ok, so for today we will demonstrate how to use graphql by implementng a chat application.
For the purpose of thi my dependencies are already installed and also I hvae created my the structure of the sever and will destructure the implemenation.
For those who want to follow the implementatio on your own station you can clone our repositiory and checkout master branch.
The first step will be to create the folder structure of the app.
We will have a folder for the client and one for the server.
- client
- server
Now let's go and build our server.
</aside>


### Server
- nodejs
- graphql
- prisma
- prisma-binding
- graphql-yoga
- docker 

<aside class="notes">
For this we will have the following dependencies:
- nodejs: as many of you know, is a platform for building server applications using JavaScript.
- graphql: is a query language for APIs and a runtime queries.
- prisma: is the glue between your GraphQL API and database. Implement resolvers using  bindings and delegate queries to  the  query engine
- prisma-binding: is a tool that simplifies the GraphQL implementions of the resolvers by delegating execution of queries to the API.
- graphql-yoga: is a Fully-featured GraphQL Server that  handles the network configurations, cors, middlewares and other server stuff.
- docker: is a container that will allow our app to be packaged as an application with all of the parts it needs.
</aside>

<aside class="notes">
Also make sure to have ngraphql, prisma installed globally.
Now I will go through all the steps.

For the database we have created a folder prisma where we have initiated prisma using the prisma init command. Prisma will create for us a prisma.yml and a datamodel.graphql file where the main types will be held and a docker-compose.yml file if we have choosen this in the setup.

For the next we will define our types in the prisma / datamodel, in our case will be the Message like this; The message will have an ID which is unique and author field, string and content also string.

type Message {
    id: ID! @unique
    author: String!
    content: String!
}

Prisma will generate the schema for the prisma types and for we will need to deploy prisma, using docker-compose up -d and after the prisma deploy --force command.

Based on your data model, Prisma generates a ready-to-use GraphQL API exposing a powerful CRUD GraphQL schema.

The next will be to create the file schema.graphql that will contain our schema definition the queries, mutations and subscriptions.
First will import the message type and subscription payload from the generated prsima schema:

#import Message, MessageSubscriptionPayload from "./generated/prisma.graphql"

After that will create a query for the messages:
type Query {
    messages: [Message!]!
}

Some mutations like:
type Mutation {
    addMessage(author: String!, content: String!): Message!,
    modifyMessage(id: ID!, content: String): Message!
    removeMessage(id: ID!): Message!
}

and a subscriptions for the last messages added.
type Subscription {
    newMessage: MessageSubscriptionPayload!
}

After the scheme is done we can start implementing our server and adding logic in the 
-index.js file.
Here we've import the following packages:

const {GraphQLServer} = require('graphql-yoga');
const {Prisma} = require('prisma-binding');
 
We will start to create our resolvers and begin with the the query:

    Query: {
        messages: (obj, args, context, info) => {
            return context.db.query.messages({}, info)
        },
    },

    ...
    obj: The object that contains the result returned from the resolver on the parent field, or, in the case of a top-level Query field, the rootValue passed from the server configuration. This argument enables the nested nature of GraphQL queries.
args: An object with the arguments passed into the field in the query. For example, if the field was called with author(name: "Ada"), the args object would be: { "name": "Ada" }.
context: This is an object shared by all resolvers in a particular query, and is used to contain per-request state, including authentication information, dataloader instances, and anything else that should be taken into account when resolving the query. If you’re using Apollo Server, read about how to set the context in the setup documentation.
info: This argument should only be used in advanced cases, but it contains information about the execution state of the query, including the field name, path to the field from the root, and more. It’s only documented in the GraphQL.js source code....

The next step will be to create some mutations for our data model:

Mutation: {
        addMessage: (root, args, context, info) => {
            return context.db.mutation.createMessage(
                {
                    data: {
                        author: args.author,
                        content: args.content
                    }
                },
                info,
            )
        },
        removeMessage: (root, args, context, info) => {
            return context.db.mutation.deleteMessage({
                where: {
                    id: args.id
                }
            })
        }
    },

    And last a Subscription for any time a new message is added:

    Subscription: {
        newMessage: {
            subscribe: async (root, args, context, info) => {
                return context.db.subscription.message(
                    {
                        where: {
                            mutation_in: ['CREATED'],
                        },
                    },
                    info
                )
            },
        },
    }

After the resolver are finsihed we can now configure our server by instantiating our Grapqh Yoga server and passing the schema, resolvers and configure it.

One last step to do is to generate the graphqlconfig.yml file using grapghql init command. THis file ill keep the project overview settings.
 
After this we can start our server and check the playground and do some operations in real time in the database.

We will do a mutation by addding a new message manually, query and subscription.
</aside>


---

### Client
- create-react-app
- react
- graphql
- apollo-client
- react-apollo
- subscriptions-transport-ws

<aside class="notes">
For the client side we will use create-react-app that will generate a simple react applications and graphql dependencies.
THe major ones I can mention are:
- apollo-client: The client is designed to help you quickly build a UI that fetches data with GraphQL, and can be used with any JavaScript front-end.
-react-apollo: will allow us to fetch data from your GraphQL server and use it in building complex and reactive UIs components.
- subscriptions-transport-ws: A GraphQL WebSocket server and client to facilitate GraphQL queries, mutations and subscriptions over WebSocket.

Will start by connection to our API by creating a http link for it:

const httpLink = new HttpLink({uri: `http://localhost:4000`});


after that will initiate also our websocket connection for the subscriptions:

const wsLink = new WebSocketLink({
    uri: `ws://localhost:4000`,
    options: {
        reconnect: true
    }
});

Will create the link for the client and also treat aeach connection separate.

const link = split(
    ({query}) => {
        const {kind, operation} = getMainDefinition(query)
        return kind === 'OperationDefinition' && operation === 'subscription'
    },
    wsLink,
    httpLink
);

And then will instantiate the client with the link and a use as cache the memory:

const client = new ApolloClient({
    link,
    cache: new InMemoryCache(),
});

And after that we will need to provide the client to our app using the APollo provide from apollo client.

ReactDOM.render(
    <ApolloProvider client={client}>
        <App/>
    </ApolloProvider>,
    document.getElementById('root'),
);

So we setup our connection and now we will create s dumb componet to paint our messages:

import React from 'react';

const Message = (props) => {
    return (
        <div className="message" id={props.id}>
            <h2 className="message-author">{props.author}</h2>
            {props.content && <p className="message-content">{props.content}</p>}
        </div>
    )
}

export default Message;


and will start coding the app logic.
Will import our message so we can use it in the queries.

Firstly we will query using graphql sintax the messages:

 <Query query={gql`
      {
          messages {
              id,
              author,
              content
          }
      }
  `}>
  {
      ({loading, error, data, refetch}) => {

and will treat the each state of the component

if (loading) {
                                return (<h3>Loading...</h3>);
                            }

                            if (error) {
                                return (<h3>{error.message}</h3>);
                            }



and will paint the messages:

return (
                                <div className="messages">

                                    {
                                        data.messages.map((message, i) => {
                                            return (
                                                <Message key={i} {...message} />
                                            )
                                        })
                                    }

                                

                                </div>
                            );

     }
                    }

                </Query>

So now let's check if any message are displayed in our app.
Ok, everyhing is fine, let's subscribe to the new messages by adding the subscrition in the listing.

    <Subscription
    subscription={gql`
      subscription newMessage {
        newMessage {
            node{
                id,
                author,
                content
            },

        }
      }
    `}
>
    {({data, loading}) => {
        if (loading) {
            return ('Listening...');
        }
        if (data) {
            return (<Message {...data.newMessage.node} />);
        }

    }}
                                    </Subscription>


                                    OK let's add a message in the api and see if that is automatically added.


The last step will be to create simple form that will allow user to post:

<Mutation mutation={gql`
                    mutation addMessage($author: String!, $content: String!) {
                        addMessage(author: $author, content: $content) {
                                                id,
                                                author,
                                                content,
                                            }
                                        }
                                    `}>
                    {addMessage => (
                        <form action="/" className="add-message" onSubmit={e => {
                            e.preventDefault();

                            addMessage({
                                variables: {author: this.author.value, content: this.content.value}
                            })
                        }}>
                            <input type="text" ref={(node) => this.author = node}/><br/>
                            <textarea ref={(node) => this.content = node} cols="30"
                                      rows="5"></textarea><br/>
                            <button type="submit">Add Message</button>
                        </form>
                    )}
                </Mutation>

</aside>