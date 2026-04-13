# Backend Notes (Node.js, Express, MongoDB) - For B.Tech Placements

## Table of Contents
1. [Node.js Basics](#nodejs-basics)
2. [npm & Packages](#npm--packages)
3. [File System (fs)](#file-system-fs)
4. [Express.js](#expressjs)
5. [Middleware](#middleware)
6. [RESTful APIs](#restful-apis)
7. [MongoDB & Mongoose](#mongodb--mongoose)
8. [EJS Template Engine](#ejs-template-engine)
9. [Interview Questions](#interview-questions)

---

## 1. Node.js Basics

### What is Node.js?
- JavaScript runtime for server-side
- Built on Chrome V8 engine
- Non-blocking I/O (async)
- Event-driven

### Setup
```bash
# Check version
node -v

# Run JavaScript file
node filename.js
```

### CommonJS Modules
```javascript
// Export
module.exports = function add(a, b) {
    return a + b;
};

// Or with exports
exports.add = (a, b) => a + b;

// Import
const add = require('./math');
```

### ES Modules (package.json type: "module")
```javascript
// Export
export function add(a, b) {
    return a + b;
}

// Import
import { add } from './math.js';
```

---

## 2. npm & Packages

### Commands
```bash
npm init -y           # Initialize package.json
npm install express   # Install package
npm install -D nodemon  # Dev dependency
npm install -g typescript  # Global
npm list              # List packages
npm update            # Update packages
npm uninstall express # Remove
```

### package.json
```json
{
    "name": "my-app",
    "version": "1.0.0",
    "main": "index.js",
    "type": "module",
    "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js"
    },
    "dependencies": {
        "express": "^4.18.2"
    },
    "devDependencies": {
        "nodemon": "^3.0.0"
    }
}
```

### Dependencies vs DevDependencies
- **dependencies**: Needed in production (express, mongoose)
- **devDependencies**: Only for development (nodemon, jest)

---

## 3. File System (fs)

### Synchronous (avoid)
```javascript
const fs = require('fs');

const data = fs.readFileSync('file.txt', 'utf8');
fs.writeFileSync('file.txt', 'Hello');
```

### Asynchronous (preferred)
```javascript
const fs = require('fs').promises;

async function readFile() {
    try {
        const data = await fs.readFile('file.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

### Callback Style
```javascript
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data);
});
```

### Common Methods
```javascript
// Read
fs.readFile()      // Async read
fs.readFileSync()  // Sync read

// Write
fs.writeFile()     // Async write (overwrites)
fs.appendFile()    // Async append

// File info
fs.stat()          // Get file stats

// Create/Remove
fs.mkdir()         // Create directory
fs.rmdir()         // Remove directory
fs.unlink()        // Delete file
```

### Path Module
```javascript
const path = require('path');

path.join('/folder', 'sub', 'file.txt');  // /folder/sub/file.txt
path.resolve('file.txt');                  // Absolute path
path.extname('file.txt');                  // .txt
path.basename('/path/file.txt');           // file.txt
path.dirname('/path/file.txt');            // /path
```

---

## 4. Express.js

### Basic Server
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

### Request Properties
```javascript
app.get('/user/:id', (req, res) => {
    req.params.id      // URL parameters
    req.query.name     // Query strings ?name=John
    req.body          // Body data (needs parser)
    req.headers        // Headers
});
```

### Response Methods
```javascript
res.send('Hello');           // Send text/JSON
res.json({ name: 'John' });  // Send JSON
res.status(404).send('Not Found');
res.redirect('/url');         // Redirect
res.download('/file.txt');   // Download file
res.render('template');      // Render template
```

### Routes
```javascript
// GET
app.get('/api/users', (req, res) => {
    res.json(users);
});

// POST
app.post('/api/users', (req, res) => {
    const newUser = req.body;
    users.push(newUser);
    res.status(201).json(newUser);
});

// PUT
app.put('/api/users/:id', (req, res) => {
    const { id } = req.params;
    // Update user
    res.json({ message: 'Updated' });
});

// DELETE
app.delete('/api/users/:id', (req, res) => {
    const { id } = req.params;
    // Delete user
    res.json({ message: 'Deleted' });
});

// All methods
app.all('/path', (req, res) => {
    // Handles all methods
});
```

### Route Parameters
```javascript
// Required
app.get('/users/:id', (req, res) => {
    const id = req.params.id;
});

// Optional
app.get('/users/:id/:name?', (req, res) => {
    const { id, name } = req.params;
});

// Regex
app.get('/users/:id([0-9]+)', (req, res) => {
    // Only numbers
});
```

### Query Strings
```javascript
// URL: /search?name=john&age=25
app.get('/search', (req, res) => {
    const { name, age } = req.query;
});
```

---

## 5. Middleware

### What is Middleware?
- Functions that execute before route handlers
- Can modify request/response
- Can end request or pass to next

### Built-in Middleware
```javascript
const express = require('express');
const app = express();

// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));

// Static files from specific folder
app.use('/static', express.static('public'));
```

### Custom Middleware
```javascript
// Logger middleware
function logger(req, res, next) {
    console.log(`${req.method} ${req.url}`);
    next();  // Important!
}

app.use(logger);

// Authentication middleware
function auth(req, res, next) {
    if (req.headers.authorization) {
        next();
    } else {
        res.status(401).json({ error: 'Unauthorized' });
    }
}

// Route-specific middleware
app.get('/dashboard', auth, (req, res) => {
    res.send('Dashboard');
});
```

### Error Handling Middleware
```javascript
// 404 handler
app.use((req, res) => {
    res.status(404).json({ error: 'Not found' });
});

// Error handler (must have 4 params)
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ error: 'Something went wrong' });
});
```

---

## 6. RESTful APIs

### HTTP Methods
| Method | Purpose | Idempotent |
|--------|---------|------------|
| GET | Read | Yes |
| POST | Create | No |
| PUT | Update (replace) | Yes |
| PATCH | Update (partial) | No |
| DELETE | Delete | Yes |

### REST Principles
- Resources: Use nouns (/users not /getUsers)
- HTTP methods: Proper verb usage
- Status codes: Proper response codes
- Stateless: No client state on server

### Status Codes
```javascript
// Success
200 OK
201 Created
204 No Content

// Client Errors
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
422 Unprocessable Entity

// Server Errors
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

### Example REST API
```javascript
const express = require('express');
const app = express();
app.use(express.json());

let users = [
    { id: 1, name: 'John', email: 'john@example.com' }
];

// GET all
app.get('/api/users', (req, res) => {
    res.json(users);
});

// GET one
app.get('/api/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).json({ error: 'Not found' });
    res.json(user);
});

// POST
app.post('/api/users', (req, res) => {
    const { name, email } = req.body;
    const newUser = {
        id: users.length + 1,
        name,
        email
    };
    users.push(newUser);
    res.status(201).json(newUser);
});

// PUT
app.put('/api/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).json({ error: 'Not found' });
    
    user.name = req.body.name || user.name;
    user.email = req.body.email || user.email;
    res.json(user);
});

// DELETE
app.delete('/api/users/:id', (req, res) => {
    const index = users.findIndex(u => u.id === parseInt(req.params.id));
    if (index === -1) return res.status(404).json({ error: 'Not found' });
    
    users.splice(index, 1);
    res.status(204).send();
});
```

---

## 7. MongoDB & Mongoose

### MongoDB Basics
- NoSQL document database
- Stores JSON-like documents
- Collections = tables
- Documents = rows

### Mongoose
- ODM (Object Data Modeling)
- Schema validation
- Connection management

### Connection
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/mydb')
    .then(() => console.log('Connected'))
    .catch(err => console.error(err));

// Connection string with options
mongoose.connect('mongodb://localhost:27017/mydb', {
    useNewUrlParser: true,
    useUnifiedTopology: true
});
```

### Schema & Model
```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
        trim: true
    },
    email: {
        type: String,
        required: true,
        unique: true,
        lowercase: true
    },
    age: {
        type: Number,
        min: 0,
        max: 150
    },
    createdAt: {
        type: Date,
        default: Date.now
    }
});

const User = mongoose.model('User', userSchema);
```

### CRUD Operations

#### Create
```javascript
// Single document
const user = new User({ name: 'John', email: 'john@example.com' });
await user.save();

// Or create
const user = await User.create({ name: 'John', email: 'john@example.com' });
```

#### Read
```javascript
// Find all
const users = await User.find();

// Find with conditions
const users = await User.find({ age: { $gte: 18 } });

// Find one
const user = await User.findOne({ email: 'john@example.com' });

// Find by ID
const user = await User.findById(id);
```

#### Update
```javascript
// Update one
await User.updateOne(
    { _id: id },
    { name: 'Jane' }
);

// Find and update
const user = await User.findByIdAndUpdate(
    id,
    { name: 'Jane' },
    { new: true }  // Return updated doc
);

// Find and update
await User.findOneAndUpdate(
    { email: 'john@example.com' },
    { name: 'Jane' }
);
```

#### Delete
```javascript
await User.deleteOne({ _id: id });
await User.deleteMany({ age: { $lt: 18 } });
await User.findByIdAndDelete(id);
```

### Query Methods
```javascript
// Select fields
User.find().select('name email');

// Sort
User.find().sort({ name: 1 });  // 1 ascending, -1 descending

// Limit
User.find().limit(10);

// Skip (pagination)
User.find().skip(20).limit(10);

// Count
const count = await User.countDocuments({ age: { $gte: 18 } });
```

### Middleware (Hooks)
```javascript
const userSchema = new mongoose.Schema({ ... });

// Before save
userSchema.pre('save', function(next) {
    console.log('About to save:', this.name);
    next();
});

// After save
userSchema.post('save', function(doc) {
    console.log('Saved:', doc.name);
});
```

### Relationships
```javascript
// Embedded
const postSchema = new mongoose.Schema({
    title: String,
    content: String,
    author: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User'
    }
});
```

---

## 8. EJS Template Engine

### Setup
```bash
npm install ejs
```

### Configuration
```javascript
app.set('view engine', 'ejs');
app.set('views', './views');
```

### Basic Template
```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1><%= heading %></h1>
    
    <ul>
        <% users.forEach(user => { %>
            <li><%= user.name %></li>
        <% }) %>
    </ul>
</body>
</html>
```

### EJS Syntax
```html
<%= %>   // Output escaped HTML
<%- %>   // Output unescaped HTML
<% %>    // JavaScript code (no output)
<%- include('partial') %>
```

### Render from Route
```javascript
app.get('/', (req, res) => {
    res.render('index', {
        title: 'Home',
        heading: 'Welcome',
        users: [{ name: 'John' }, { name: 'Jane' }]
    });
});
```

---

## 9. Important Interview Questions

### Q1: What is Node.js? Why use it?
- JavaScript runtime for server
- Non-blocking I/O (async)
- Same language for front-end and back-end
- Great for real-time apps, APIs

### Q2: What is the difference between synchronous and asynchronous?
- **Sync**: Blocking, waits for operation to complete
- **Async**: Non-blocking, continues execution

### Q3: What is npm?
- Node Package Manager
- Manages dependencies
- Largest ecosystem of packages

### Q4: What is Express.js?
- Web application framework for Node.js
- Minimal, flexible
- Handles routing, middleware

### Q5: What is middleware?
- Functions that execute between request and response
- Can modify req/res
- Chainable

### Q6: What is the difference between PUT and PATCH?
- **PUT**: Replace entire resource
- **PATCH**: Partial update

### Q7: What is MongoDB? How is it different from SQL?
- NoSQL document database
- No fixed schema
- Stores JSON-like documents
- SQL is relational, MongoDB is document-based

### Q8: What is Mongoose?
- ODM for MongoDB
- Schema-based modeling
- Validation, relationships

### Q9: What is REST?
- Representational State Transfer
- Architecture style for APIs
- Uses HTTP methods properly

### Q10: What is the difference between req.params and req.query?
- **params**: URL parameters /users/:id
- **query**: Query string ?name=John

### Q11: What is middleware error handling?
- 4 parameters: (err, req, res, next)
- Catches errors from middleware/routes

### Q12: What is JWT?
- JSON Web Token
- Stateless authentication
- Encodes user info

### Q13: What is the difference between MongoDB and Mongoose?
- **MongoDB**: Database
- **Mongoose**: ODM library (abstraction)

### Q14: What is the purpose of express.json()?
- Parses JSON request bodies
- Makes data available in req.body

### Q15: How do you handle errors in Express?
- try-catch in async routes
- Error handling middleware
- Express error handlers

### Q16: What is the difference between embedding and referencing in MongoDB?
- **Embedding**: Store related data in same document
- **Referencing**: Store ID reference to other collection

### Q17: What is the event loop in Node.js?
- Mechanism handling async operations
- Processes callbacks
- Phases: timers, I/O callbacks, idle, poll, check, close

### Q18: What are streams in Node.js?
- Handle streaming data
- Read/write piece by piece
- Memory efficient for large files

### Q19: What is clustering in Node.js?
- Use multiple CPU cores
- child_process or cluster module

### Q20: What is the difference between process.nextTick() and setImmediate()?
- **nextTick**: Execute after current operation
- **setImmediate**: Execute in next iteration

---

## Backend Cheat Sheet

```javascript
// Express basic
const express = require('express');
const app = express();
app.use(express.json());

// Routes
app.get('/', (req, res) => res.send('Hello'));
app.post('/api/users', (req, res) => res.json(req.body));

// Listen
app.listen(3000);

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/mydb');

// Schema
const schema = new mongoose.Schema({ name: String });
const Model = mongoose.model('Model', schema);

// CRUD
await Model.find();
await Model.create({ name: 'John' });
await Model.findByIdAndUpdate(id, { name: 'Jane' });
await Model.findByIdAndDelete(id);
```

```bash
# Common commands
npm init -y
npm install express mongoose ejs
npm install -D nodemon
node index.js
```

---
**End of Backend Notes**