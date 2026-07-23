# 🚀 Express.js -- Learning Notes

> Cleaned and organized from the lecture transcript. Filler words and
> unclear speech have been omitted.

## 🎯 Goal

Understand **why Express.js exists**, how it relates to **Node.js**, and
the core concepts: - Node vs Express - HTTP module - Routing -
Middleware - Request & Response

------------------------------------------------------------------------

# Node.js vs Express.js

  -----------------------------------------------------------------------
  Node.js                             Express.js
  ----------------------------------- -----------------------------------
  Runtime environment                 Framework built on Node

  Can create servers using built-in   Simplifies server development
  `http` module                       

  Powerful but verbose                Cleaner and shorter syntax
  -----------------------------------------------------------------------

### Key Idea

Express internally uses Node's **HTTP module**.

    Your Code
       ↓
    Express
       ↓
    Node HTTP Module
       ↓
    Operating System

------------------------------------------------------------------------

# Why Express?

Without Express:

-   Lots of boilerplate
-   More difficult routing
-   Harder request/response handling

With Express:

-   Cleaner APIs
-   Less code
-   Industry standard

------------------------------------------------------------------------

# Routing

A **Route** is simply a URL path.

Examples

    /
     /profile
     /about
     /contact

Routing = creating these URL endpoints.

Request Flow

    Client
       ↓
    Middleware
       ↓
    Route Matching
       ↓
    Controller Logic
       ↓
    Response

If no route matches: - 404 response (unless a catch-all route is
defined)

------------------------------------------------------------------------

# Express Starter

``` bash
npm init -y
npm i express
```

Basic server

``` js
const express = require("express");
const app = express();

app.get("/", (req,res)=>{
    res.send("Hello World");
});

app.listen(3000);
```

Visit

    http://localhost:3000

------------------------------------------------------------------------

# Multiple Routes

``` js
app.get("/profile",(req,res)=>{
   res.send("Hello from Profile");
});
```

------------------------------------------------------------------------

# Middleware

Middleware is a function that executes **before the matched route
handler**.

``` text
Request
   ↓
Middleware
   ↓
Route
   ↓
Response
```

Example

``` js
app.use((req,res,next)=>{
   console.log("Request received");
   next();
});
```

⚠️ `next()` is important. Without it, the request stops.

------------------------------------------------------------------------

# req vs res

## req

Contains incoming client information.

Examples: - URL - Headers - IP - Parameters - Query - Body

## res

Used to send data back.

Examples

``` js
res.send()
res.json()
res.status()
```

------------------------------------------------------------------------

# Important Interview Points

-   Express is **not** a replacement for Node.
-   Express uses Node's HTTP module internally.
-   Node can build servers without Express.
-   Express exists to make development easier.
-   Routing maps URLs to code.
-   Middleware runs before route handlers.
-   `req` receives data.
-   `res` sends data.
-   `next()` passes execution to the next middleware/route.

------------------------------------------------------------------------

# Quick Revision

-   ✅ Node provides runtime
-   ✅ HTTP module creates servers
-   ✅ Express simplifies server creation
-   ✅ Routes map URLs
-   ✅ Middleware runs first
-   ✅ req = incoming data
-   ✅ res = outgoing data
-   ✅ next() continues execution
