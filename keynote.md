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
  GraphQL is a syntax that describes how to ask for data. You can use it both on the client and also on the server. <br/>
  On the client you use it to request data from the server and on the server you can use it to get the data from multiple sources and send it to the client.
</aside>


--

```
http://server.io?query={ news{ id, title },user{ id, name } }
```

<img data-src="/syntax.png" alt="Syntax">

<aside class="notes">
  The query language is basically about selecting fields on objects.
</aside>

--

### Why an alternative to REST ?


<aside class="notes">
  GraphQL and REST are not so different after all, but GraphQL has some small changes that make a big difference to the developer experience of building and consuming an API.
</aside>

<span class="fragment">GraphQL has 3 main characteristics:</span>

--

It uses a type system to describe data.

<img data-src="/type.png" alt="Syntax">

<aside class="notes">

The GraphQL Type system describes the capabilities of a GraphQL server and is used to determine if a query is valid. The type system also describes the input types of query variables to determine if values provided at runtime are valid.<br/>
It has 3 main definitions:<br/>
Schema Definition<br/>
Type Definition<br/>
Directive Definition

</aside>


--

Aggregating data from multiple sources is less painfull

<img  height="500" data-src="/diagram.png" alt="Syntax">


<aside class="notes">
The core idea of REST is the resource. Each resource is identified by a URL, and you retrieve that resource by sending a GET request to that URL. You will likely get a JSON response, since that’s what most APIs are using these days. So it looks something like:
<br />
With GraphQL you can simply ask for what you want (“get me the news, the user info and its authentification information”) and wait for them to return.
</aside>


---

# Nodejs
