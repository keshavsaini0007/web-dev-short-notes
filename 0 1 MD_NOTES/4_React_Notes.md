# React Notes - For B.Tech Placements

## Table of Contents
1. [Introduction](#introduction)
2. [JSX](#jsx)
3. [Components](#components)
4. [Props](#props)
5. [useState Hook](#usestate-hook)
6. [useEffect Hook](#useeffect-hook)
7. [useRef Hook](#useref-hook)
8. [useCallback & useMemo](#usecallback--usememo)
9. [useContext Hook](#usecontext-hook)
10. [Conditional Rendering](#conditional-rendering)
11. [Lists & Keys](#lists--keys)
12. [Form Handling](#form-handling)
13. [React Router](#react-router)
14. [React + Backend](#react--backend)
15. [Interview Questions](#interview-questions)

---

## 1. Introduction

### What is React?
- A JavaScript library for building user interfaces
- Developed by Facebook (Meta)
- Component-based, declarative

### Why React?
- Virtual DOM for performance
- Reusable components
- One-way data binding
- Large ecosystem

### Setup
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

---

## 2. JSX

### What is JSX?
- JavaScript XML - syntax extension for React
- Looks like HTML but is JavaScript

### Basic Syntax
```jsx
// Must return single element
const element = (
    <div>
        <h1>Hello</h1>
        <p>Welcome to React</p>
    </div>
);
```

### JSX vs HTML Differences
```jsx
// className instead of class
<div className="container">

// camelCase for attributes
<button onClick={handleClick} tabIndex={0}>

// Self-closing tags must close
<img src="..." alt="..." />
<br />

// Inline styles are objects
<div style={{ color: 'blue', fontSize: '16px' }}>
```

### JavaScript in JSX
```jsx
const name = "John";
const element = <h1>Hello, {name}!</h1>;

const isLogged = true;
<div>{isLogged ? "Welcome" : "Please login"}</div>
<div>{isLogged && "Welcome"}</div>
```

---

## 3. Components

### Functional Components
```jsx
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

// Arrow function
const Welcome = (props) => {
    return <h1>Hello, {props.name}</h1>;
};
```

### Component Export/Import
```jsx
// Named export
export function Welcome() { }
import { Welcome } from './Welcome';

// Default export
export default function Welcome() { }
import Welcome from './Welcome';
```

### Pure Components
```jsx
// Props same → Output same
function PureComponent(props) {
    return <h1>{props.title}</h1>;
}
```

---

## 4. Props

### Passing Props
```jsx
function Parent() {
    return <Child name="John" age={25} />;
}

function Child(props) {
    return <h1>{props.name} is {props.age}</h1>;
}
```

### Default Props
```jsx
function Button({ text = "Click", variant = "primary" }) {
    return <button className={variant}>{text}</button>;
}

Button.defaultProps = {
    text: "Click",
    variant: "primary"
};
```

### Children Props
```jsx
function Card({ children }) {
    return <div className="card">{children}</div>;
}

// Usage
<Card>
    <h1>Title</h1>
    <p>Content</p>
</Card>
```

---

## 5. useState Hook

### Basic Usage
```jsx
import { useState } from 'react';

function Counter() {
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </div>
    );
}
```

### Multiple State Variables
```jsx
const [name, setName] = useState("");
const [age, setAge] = useState(0);
const [isActive, setIsActive] = useState(false);
```

### State with Objects
```jsx
const [user, setUser] = useState({ name: "", email: "" });

// Update one field
setUser({ ...user, name: "John" });
```

### State with Arrays
```jsx
const [items, setItems] = useState([]);

// Add
setItems([...items, newItem]);

// Remove
setItems(items.filter(item => item.id !== id));

// Update
setItems(items.map(item => 
    item.id === id ? { ...item, name: "Updated" } : item
));
```

### Functional Updates
```jsx
// Using current state
setCount(prevCount => prevCount + 1);
setItems(prevItems => [...prevItems, newItem]);
```

---

## 6. useEffect Hook

### Purpose
- Handle side effects (data fetching, subscriptions, DOM manipulation)
- Runs after every render by default

### Syntax
```jsx
import { useEffect } from 'react';

useEffect(() => {
    // Effect code
    return () => {
        // Cleanup code
    };
}, [dependencies]);
```

### Use Cases

#### Run on every render
```jsx
useEffect(() => {
    console.log("Runs on every render");
});
```

#### Run once (mount)
```jsx
useEffect(() => {
    console.log("Runs once on mount");
    // Fetch data
    fetchData();
}, []);  // Empty dependency array
```

#### Run when dependency changes
```jsx
useEffect(() => {
    console.log("User changed");
    fetchUserData(userId);
}, [userId]);
```

#### Cleanup
```jsx
useEffect(() => {
    const handleScroll = () => {};
    window.addEventListener('scroll', handleScroll);
    
    return () => {
        window.removeEventListener('scroll', handleScroll);
    };
}, []);
```

### Fetching Data
```jsx
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
    fetch('https://api.example.com/data')
        .then(res => res.json())
        .then(data => {
            setData(data);
            setLoading(false);
        })
        .catch(err => {
            setError(err);
            setLoading(false);
        });
}, []);
```

---

## 7. useRef Hook

### Purpose
- Persist values across renders
- Access DOM elements
- Store mutable values without re-rendering

### Basic Usage
```jsx
import { useRef } from 'react';

function TextInput() {
    const inputRef = useRef(null);
    
    const handleClick = () => {
        inputRef.current.focus();
        inputRef.current.value = "Hello";
    };
    
    return (
        <div>
            <input ref={inputRef} type="text" />
            <button onClick={handleClick}>Focus</button>
        </div>
    );
}
```

### Storing Mutable Values
```jsx
function Timer() {
    const countRef = useRef(0);
    
    const increment = () => {
        countRef.current++;
        console.log(countRef.current);
    };
    
    return <button onClick={increment}>Increment</button>;
}
```

### vs useState
- `useState` causes re-render on change
- `useRef` doesn't cause re-render

---

## 8. useCallback & useMemo

### useCallback
- Memoize functions
- Prevent unnecessary re-creations

```jsx
import { useCallback } from 'react';

function Parent() {
    const [count, setCount] = useState(0);
    
    // Only changes when count changes
    const handleClick = useCallback(() => {
        console.log(count);
    }, [count]);
    
    return <Child onClick={handleClick} />;
}
```

### useMemo
- Memoize computed values
- Expensive calculations

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ data }) {
    // Only recalculates when data changes
    const sortedData = useMemo(() => {
        return data.sort((a, b) => a - b);
    }, [data]);
    
    return <ul>{sortedData.map(d => <li key={d}>{d}</li>)}</ul>;
}
```

---

## 9. useContext Hook

### Create Context
```jsx
import { createContext } from 'react';

export const ThemeContext = createContext("light");
```

### Provide Context
```jsx
function App() {
    return (
        <ThemeContext.Provider value="dark">
            <Navbar />
        </ThemeContext.Provider>
    );
}
```

### Consume Context
```jsx
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function Navbar() {
    const theme = useContext(ThemeContext);
    
    return (
        <nav className={theme}>
            <h1>Navbar</h1>
        </nav>
    );
}
```

### Multiple Contexts
```jsx
// Combine providers
<UserProvider>
    <ThemeProvider>
        <App />
    </ThemeProvider>
</UserProvider>
```

---

## 10. Conditional Rendering

### If Statement
```jsx
function Greeting({ isLogged }) {
    if (isLogged) {
        return <h1>Welcome back!</h1>;
    }
    return <h1>Please login</h1>;
}
```

### Ternary Operator
```jsx
function Greeting({ isLogged }) {
    return (
        <h1>
            {isLogged ? "Welcome back!" : "Please login"}
        </h1>
    );
}
```

### Logical AND
```jsx
function Notification({ unread }) {
    return (
        <div>
            <h1>Hello</h1>
            {unread > 0 && <p>You have {unread} messages</p>}
        </div>
    );
}
```

### Switch (with variables)
```jsx
function Status({ status }) {
    return (
        <div>
            {status === 'loading' && <Spinner />}
            {status === 'error' && <ErrorMessage />}
            {status === 'success' && <SuccessMessage />}
        </div>
    );
}
```

---

## 11. Lists & Keys

### Rendering Arrays
```jsx
function ItemList({ items }) {
    return (
        <ul>
            {items.map(item => (
                <li key={item.id}>{item.name}</li>
            ))}
        </ul>
    );
}
```

### Why Keys?
- Help React identify changed items
- Performance optimization
- Must be unique among siblings

### Common Mistakes
```jsx
// ❌ Wrong - using index as key
items.map((item, index) => (
    <li key={index}>{item.name}</li>
));

// ✅ Correct - using unique ID
items.map(item => (
    <li key={item.id}>{item.name}</li>
));
```

---

## 12. Form Handling

### Controlled Components
```jsx
function LoginForm() {
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");
    
    const handleSubmit = (e) => {
        e.preventDefault();
        console.log({ email, password });
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input 
                type="email" 
                value={email}
                onChange={e => setEmail(e.target.value)}
            />
            <input 
                type="password" 
                value={password}
                onChange={e => setPassword(e.target.value)}
            />
            <button type="submit">Login</button>
        </form>
    );
}
```

### Multiple Inputs
```jsx
function Form() {
    const [formData, setFormData] = useState({
        name: "",
        email: "",
        message: ""
    });
    
    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({ ...prev, [name]: value }));
    };
    
    const handleSubmit = (e) => {
        e.preventDefault();
        console.log(formData);
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input name="name" value={formData.name} onChange={handleChange} />
            <input name="email" value={formData.email} onChange={handleChange} />
            <textarea name="message" value={formData.message} onChange={handleChange} />
            <button type="submit">Submit</button>
        </form>
    );
}
```

---

## 13. React Router

### Setup
```bash
npm install react-router-dom
```

### Basic Usage
```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
    return (
        <BrowserRouter>
            <nav>
                <Link to="/">Home</Link>
                <Link to="/about">About</Link>
            </nav>
            
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/about" element={<About />} />
                <Route path="/user/:id" element={<User />} />
            </Routes>
        </BrowserRouter>
    );
}
```

### useParams
```jsx
import { useParams } from 'react-router-dom';

function User() {
    const { id } = useParams();
    return <h1>User ID: {id}</h1>;
}
```

### useNavigate
```jsx
import { useNavigate } from 'react-router-dom';

function Home() {
    const navigate = useNavigate();
    
    return (
        <button onClick={() => navigate('/about')}>
            Go to About
        </button>
    );
}
```

---

## 14. React + Backend

### Fetch API
```jsx
function App() {
    const [data, setData] = useState([]);
    
    useEffect(() => {
        fetch('http://localhost:5000/api/users')
            .then(res => res.json())
            .then(data => setData(data))
            .catch(err => console.error(err));
    }, []);
    
    return <UserList users={data} />;
}
```

### POST Request
```jsx
const handleSubmit = async (formData) => {
    const res = await fetch('http://localhost:5000/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
    });
    
    const data = await res.json();
    console.log(data);
};
```

---

## 15. Important Interview Questions

### Q1: What is Virtual DOM?
- Lightweight copy of real DOM
- React updates virtual DOM first, then compares with previous version (diffing), and updates only changed elements in real DOM

### Q2: How does React handle state updates?
- State updates are asynchronous
- Batch updates for performance
- Use functional updates: `setCount(c => c + 1)`

### Q3: What is the difference between controlled and uncontrolled components?
- **Controlled**: State managed by React, value via props/onChange
- **Uncontrolled**: State managed by DOM, use ref

### Q4: What is the purpose of keys in lists?
- Help React identify which items changed/added/removed
- Must be unique among siblings

### Q5: What is the difference between useEffect and useLayoutEffect?
- `useEffect`: Runs after paint (async)
- `useLayoutEffect`: Runs before paint (synchronous)
- Use useLayoutEffect for DOM measurements

### Q6: How do you optimize React performance?
- Use `React.memo` for components
- Use `useMemo` and `useCallback`
- Lazy load components with `React.lazy`
- Use production build

### Q7: What is prop drilling? How to avoid it?
- Passing props through multiple levels
- Solution: Context API or state management (Redux)

### Q8: What is the difference between useState and useReducer?
- `useState`: Simple state, single value
- `useReducer`: Complex state logic, multiple sub-values, predictable updates

### Q9: What is React.memo?
- Memoizes component
- Re-renders only if props change
```jsx
const MemoizedComponent = React.memo(function MyComponent({ data }) {
    return <div>{data.name}</div>;
});
```

### Q10: What are fragments?
- Return multiple elements without wrapper
```jsx
return (
    <>
        <h1>Title</h1>
        <p>Content</p>
    </>
);
```

### Q11: What is the purpose of super() in constructor?
- Calls parent class constructor
- Required before using `this`

### Q12: What is the difference between componentDidMount and useEffect?
- `componentDidMount`: Class component, runs once on mount
- `useEffect`: Functional component, runs based on dependencies

### Q13: What is JSX?
- JavaScript XML - syntax for React
- Gets compiled to `React.createElement()`

### Q14: What is the difference between declarative and imperative programming?
- **Declarative**: What to do (React way)
- **Imitative**: How to do (jQuery way)

### Q15: How does event handling work in React?
- Synthetic events (cross-browser)
- CamelCase: `onClick`, not `onclick`
- Pass function reference, not string

### Q16: What is the purpose of the key prop?
- Used by React's diffing algorithm
- Must be stable (not random/index)
- Unique among siblings

### Q17: What is the difference between state and props?
- **Props**: Passed from parent, read-only
- **State**: Managed internally, mutable

### Q18: What is reconciliation?
- Process of updating real DOM to match virtual DOM
- Uses diffing algorithm

### Q19: What is the purpose of StrictMode?
- Helps find potential problems
- Double renders in dev mode
- Checks for unsafe lifecycle methods

### Q20: How does routing work in React?
- Uses react-router-dom
- Uses HTML5 History API
- Components render based on URL

---

## React Hooks Cheat Sheet

```jsx
// useState
const [state, setState] = useState(initial);

// useEffect
useEffect(() => {}, []);

// useRef
const ref = useRef(null);

// useContext
const value = useContext(MyContext);

// useCallback
const fn = useCallback(() => {}, []);

// useMemo
const value = useMemo(() => compute(), []);

// useReducer
const [state, dispatch] = useReducer(reducer, initialState);

// Custom Hook
function useCustomHook() {
    const [data, setData] = useState(null);
    // logic
    return data;
}
```

---
**End of React Notes**