# Express.js - Complete Learning Notes

> Based on: "Express.js - Learn What Matters: Mastering the Framework" H:\express\express transcript..txt(Backend Node JS Series)

## Table of Contents
1. [Node vs Express](#1-node-vs-express)
2. [What is Express.js](#2-what-is-expressjs)
3. [Why Express.js](#3-why-expressjs)
4. [Routing](#4-routing)
5. [Middleware](#5-middleware)
6. [Request & Response](#6-request--response)
7. [Route Parameters (Dynamic Routing)](#7-route-parameters-dynamic-routing)
8. [Template Engines (EJS)](#8-template-engines-ejs)
9. [Static Files](#9-static-files)
10. [Error Handling](#10-error-handling)

---

## 1. Node vs Express

### The Core Relationship
- **Node.js** is the **foundation** — it gives you the power to create a server
- **Express.js** is a **framework** built on top of Node.js that makes server-side code easier to write
- Without Express, you **can** still use Node.js; without Node.js, you **cannot** use Express

### Real-World Analogy
Think of a **CPU** (computer):
- The CPU has the **possibility** to perform calculations (like 1 + 2 = 3)
- But the CPU alone won't do it — you need a **Calculator app** (software) to use that capability
- Similarly, Node.js provides the **HTTP module** (like the CPU's capability) to create servers
- Express.js is the **convenient software** (like the Calculator app) that makes it easy to use those capabilities

### Behind the Scenes
```javascript
// Express code you write:
app.get('/', (req, res) => {
    res.send('Hello World');
});

// Behind the scenes, Express converts this to Node's HTTP module code
// Express ultimately uses Node's built-in 'http' package
```

---

## 2. What is Express.js

Express.js is:
- An **npm package** available on the npm registry
- A **framework** for building web servers in Node.js
- A **wrapper** around Node's built-in `http` module
- Used for **routing**, **request handling**, **response sending**, and **server-side management**

### Express vs Node.js HTTP Module
| Feature | Node.js HTTP | Express.js |
|---------|-------------|------------|
| Code Length | Long & complex | Short & simple |
| Learning Curve | Difficult | Easy |
| Industry Usage | Rare | Standard |
| Setup Time | High | Low |

---

## 3. Why Express.js

### Installation
```bash
npm init -y              # Initialize package.json
npm i express            # Install Express.js
```

### Basic Server Setup
```javascript
const express = require('express'); // Step 1: Import express
const app = express();              // Step 2: Initialize app (runs express internally)

app.get('/', (req, res) => {        // Step 3: Create a route
    res.send('Hello World');
});

app.listen(3000, () => {            // Step 4: Start server on port 3000
    console.log('Server is running on port 3000');
});
```

To run:
```bash
node script.js
# OR with nodemon for auto-restart:
npx nodemon script.js
```

---

## 4. Routing

### What is a Route?
A **route** is the part of a URL after the domain name, separated by slashes (`/`).

**Examples:**
- `facebook.com/` → home route
- `facebook.com/profile` → profile route
- `facebook.com/contact` → contact route
- `facebook.com/profile/harsh` → nested route

### Creating Routes
```javascript
app.get('/', (req, res) => {
    res.send('Hello World');
});

app.get('/profile', (req, res) => {
    res.send('Hello from Profile');
});

app.get('/contact', (req, res) => {
    res.send('Hello from Contact');
});
```

### How Routing Works
1. User sends a **request** from the browser to a URL
2. The request enters Express and passes through **middleware**
3. The request reaches the **routing block** where all routes are defined
4. Express matches the URL with the defined routes
5. If a match is found → the corresponding route handler executes
6. If **no match** is found → an error is returned ("Route does not exist")

---

## 5. Middleware

### What is Middleware?
Middleware is a **function** that runs **before every route**. It executes for **every single request** regardless of which route is being accessed.

### Basic Middleware Syntax
```javascript
app.use((req, res, next) => {
    console.log('Middleware is running');
    next(); // CRITICAL: Must call next() to continue
});
```

### The Three Parameters

| Parameter | Purpose |
|-----------|---------|
| `req` | Contains data from the user/client who sent the request |
| `res` | Contains methods/s controls to send a response back |
| `next` | A function that pushes control to the next middleware/route |

### Why `next()` is Important
```javascript
// ❌ WITHOUT next() — request gets stuck, browser keeps loading
app.use((req, res, next) => {
    console.log('Middleware ran');
    // No next() — control never reaches the actual route
});

// ✅ WITH next() — request proceeds to the route
app.use((req, res, next) => {
    console.log('Middleware ran');
    next(); // Now control moves to the matching route
});
```

**Key Point:** Once control enters a middleware, it does NOT automatically go to the next route. You must explicitly call `next()` to "push" it forward.

### Middleware Execution Flow
```
Request → Middleware 1 → next() → Middleware 2 → next() → Route Handler → Response
```

---

## 6. Request & Response

### `req` (Request)
Contains all data coming **from the user/client**:
- User's IP address
- Device information
- Browser type
- Location data
- URL parameters
- Form data

```javascript
app.get('/profile', (req, res) => {
    console.log(req); // Massive object with all client data
    res.send('Profile Page');
});
```

### `res` (Response)
Contains **controls/methods** to send data back to the client:
- `res.send()` — Send plain text/HTML
- `res.render()` — Render an EJS template
- `res.json()` — Send JSON data
- `res.status()` — Set HTTP status code

```javascript
app.get('/', (req, res) => {
    res.send('Hello World');      // Send text response
    // OR
    res.render('index');          // Render a view template
    // OR
    res.json({ message: 'Hello' }); // Send JSON
});
```

### Summary
- **`req`** = incoming data (from client)
- **`res`** = outgoing controls (to client)
- **`next`** = push mechanism (to move to next handler)

---

## 7. Route Parameters (Dynamic Routing)

### The Problem
Imagine a URL like `facebook.com/profile/harsh`, `facebook.com/profile/harshita`, `facebook.com/profile/hardik`. Instead of creating a separate route for each user name, we use **dynamic routing**.

### Static Route (Fixed — Only Works for One Value)
```javascript
app.get('/profile/harsh', (req, res) => {
    res.send('Hello from harsh');
});
// ❌ This won't work for /profile/harshita or /profile/hardik
```

### Dynamic Route (Using Route Parameters)
```javascript
app.get('/profile/:username', (req, res) => {
    res.send(`Hello from ${req.params.username}`);
});
// ✅ Works for /profile/harsh, /profile/harshita, /profile/anything
```

### How It Works
1. In the URL pattern, use a **colon (`:`)** followed by a variable name
2. Express captures whatever value is in that position
3. Access it via `req.params.variableName`

```javascript
// URL: /profile/ankit
app.get('/profile/:username', (req, res) => {
    // req.params.username = "ankit"
    res.send(`Hello from ${req.params.username}`);
    // Output: "Hello from ankit"
});
```

### Multiple Route Parameters
```javascript
app.get('/author/:authorName/books/:bookId', (req, res) => {
    console.log(req.params.authorName); // Captures author name
    console.log(req.params.bookId);     // Captures book ID
    res.send(`Author: ${req.params.authorName}, Book: ${req.params.bookId}`);
});
```

### Rule
```
URL pattern: /profile/:username
            \______/ \________/
             static     dynamic

URL example: /profile/ankit
              \______/ \___/
               matched   captured as req.params.username
```

---

## 8. Template Engines (EJS)

### What is a Template Engine?
A template engine lets you write **HTML with superpowers** — you can use JavaScript logic, variables, and calculations inside your HTML-like files.

### Why EJS?
- **EJS** (Embedded JavaScript) looks and feels **exactly like HTML**
- Other engines like **Pug**, **Jade**, and **Handlebars** exist, but they have different syntax (Pug uses Python-like indentation)
- EJS is the most beginner-friendly because it's **identical to HTML** with extra features

### EJS Setup (5 Steps)

**Step 1: Install EJS**
```bash
npm i ejs
```

**Step 2: Configure Express to use EJS**
```javascript
app.set('view engine', 'ejs');
// Place this BEFORE any routes
```

**Step 3: Create a `views` folder**
```
project/
├── views/        ← Create this folder (exact name: "views")
├── public/
├── script.js
└── package.json
```

**Step 4: Create `.ejs` files inside `views`**
```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html>
<head>
    <title>Express App</title>
</head>
<body>
    <h1>Welcome to Express with EJS!</h1>
</body>
</html>
```

**Step 5: Render instead of Send**
```javascript
// ❌ Old way: res.send('Hello World');
// ✅ New way:
app.get('/', (req, res) => {
    res.render('index'); // No need to write '.ejs'
});
```

### Passing Data to EJS
```javascript
// Pass data while rendering:
app.get('/', (req, res) => {
    res.render('index', { name: 'harsh', age: 12 });
});
```

```ejs
<!-- In index.ejs, access the data -->
<h1>Hello <%= name %></h1>   <!-- Output: Hello harsh -->

<!-- EJS tags -->
<%= variable %>     <!-- Outputs escaped value -->
<%- variable %>     <!-- Outputs unescaped value (for HTML) -->
<% JavaScript %>    <!-- Runs JS code without output -->
```

### Example: Dynamic Content
```javascript
app.get('/contact', (req, res) => {
    res.render('contact', { teamName: 'Harsh' });
});
```

```ejs
<!-- views/contact.ejs -->
<h2>Meet the <%= teamName %></h2>
<!-- Output: Meet the Harsh -->
```

---

## 9. Static Files

### What are Static Files?
Files that don't change dynamically:
- **CSS** stylesheets
- **Images** (jpg, png, svg)
- **Frontend JavaScript** (client-side JS like DOM manipulation)

### Static Files Setup

**Step 1: Create a `public` folder**
```
project/
├── public/
│   ├── images/
│   ├── stylesheets/
│   └── javascript/
├── views/
├── script.js
└── package.json
```

**Step 2: Configure Express to serve static files**
```javascript
app.use(express.static('./public'));
// This tells Express: "All static files are in the 'public' folder"
```

**Step 3: Link CSS in EJS files**
```ejs
<!-- ❌ WRONG — Don't include 'public' in the path -->
<link rel="stylesheet" href="public/stylesheets/style.css">

<!-- ✅ CORRECT — Express automatically maps to the public folder -->
<link rel="stylesheet" href="/stylesheets/style.css">
```

### Why This Works
When you set `express.static('./public')`, Express automatically **prepends** `public` to any path. So:
- You request `/stylesheets/style.css`
- Express looks for `./public/stylesheets/style.css`
- If you write `public/stylesheets/style.css`, it becomes `./public/public/stylesheets/style.css` (WRONG!)

### Linking Frontend JavaScript
```ejs
<script src="/javascript/script.js"></script>
```
Remember: This is **frontend JavaScript** (browser-side), NOT your backend `script.js`.

---

## 10. Error Handling

### Default Error Handler Setup
```javascript
// Place this AFTER all your routes
app.use((err, req, res, next) => {
    res.status(500).render('error', { error: err });
});
```

### Throwing Errors
Two ways to trigger an error:
```javascript
// Method 1: Using throw
app.get('/error', (req, res) => {
    throw new Error('Something went wrong!');
});

// Method 2: Using next (Express automatically passes errors to error handler)
app.get('/error', (req, res, next) => {
    next(new Error('Custom error message'));
});
```

### Complete Error Handling Example
```javascript
// Error handler middleware (at the end)
app.use((err, req, res, next) => {
    res.status(500).render('error', { error: err });
});

// Route that triggers an error
app.get('/', (req, res) => {
    throw new Error('I don\'t know what happened');
});

// In views/error.ejs:
<h2><%= error.message %></h2>
<!-- Shows: "I don't know what happened" -->
```

### How Error Flow Works
1. You `throw new Error('message')` in any route
2. Express finds the **error handler middleware** (the one with 4 parameters: `err, req, res, next`)
3. The error handler receives the error object
4. You can pass the error to an EJS page via `res.render('error', { error: err })`
5. Display the error message using `<%= error.message %>`

### HTTP Status Codes
- `200` — OK (success)
- `404` — Not Found
- `500` — Internal Server Error (used in error handler)

---

## Quick Reference

### Common Express Methods
```javascript
app.get(path, handler)      // Handle GET requests
app.post(path, handler)     // Handle POST requests
app.use(middleware)          // Use middleware
app.set('view engine', 'ejs') // Set template engine
app.listen(port, callback)   // Start server
express.static(folderPath)   // Serve static files
```

### Project Structure
```
project/
├── views/           # EJS templates
│   ├── index.ejs
│   ├── contact.ejs
│   └── error.ejs
├── public/          # Static files
│   ├── images/
│   ├── stylesheets/
│   │   ├── style.css
│   │   └── error.css
│   └── javascript/
│       └── script.js
├── script.js        # Main server file
└── package.json
```

### Boilerplate Server
```javascript
const express = require('express');
const app = express();

app.set('view engine', 'ejs');
app.use(express.static('./public'));

// Routes
app.get('/', (req, res) => {
    res.render('index');
});

// Error handler (after all routes)
app.use((err, req, res, next) => {
    res.status(500).render('error', { error: err });
});

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

---

> **Pro Tip:** Middleware + Route Parameters + Template Engines = The foundation of 90% of Express.js backend work. Master these three concepts and you'll be able to build almost any backend feature.
