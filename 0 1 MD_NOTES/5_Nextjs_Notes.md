# Next.js Notes - For B.Tech Placements

## Table of Contents
1. [Introduction](#introduction)
2. [File-based Routing](#file-based-routing)
3. [Server Components](#server-components)
4. [Dynamic Routes](#dynamic-routes)
5. [API Routes](#api-routes)
6. [Server Actions](#server-actions)
7. [Middleware](#middleware)
8. [Authentication (Auth.js)](#authentication-authjs)
9. [Rendering Strategies](#rendering-strategies)
10. [Image Component](#image-component)
11. [Environment Variables](#environment-variables)
12. [Styling in Next.js](#styling-in-nextjs)
13. [Interview Questions](#interview-questions)

---

## 1. Introduction

### What is Next.js?
- React framework for production
- Full-stack React framework
- Features: Routing, API, SSR, SSG, ISR

### Setup
```bash
npx create-next-app@latest my-app
# or
npm create next-app@latest
cd my-app
npm run dev
```

### Project Structure
```
my-app/
├── app/
│   ├── layout.js      # Root layout
│   ├── page.js        # Home page
│   ├── globals.css    # Global styles
│   └── api/           # API routes
├── public/            # Static files
├── package.json
└── next.config.js
```

---

## 2. File-based Routing

### Basic Routes
```javascript
// app/page.js → /
export default function Home() {
    return <h1>Home Page</h1>;
}

// app/about/page.js → /about
export default function About() {
    return <h1>About Page</h1>;
}

// app/contact/page.js → /contact
export default function Contact() {
    return <h1>Contact Page</h1>;
}
```

### Nested Routes
```
app/
├── dashboard/
│   ├── page.js        → /dashboard
│   └── settings/
│       └── page.js    → /dashboard/settings
```

### Layouts
```javascript
// app/layout.js - Root layout
export default function RootLayout({ children }) {
    return (
        <html>
            <body>
                <nav>Navigation</nav>
                {children}
                <footer>Footer</footer>
            </body>
        </html>
    );
}

// app/dashboard/layout.js - Nested layout
export default function DashboardLayout({ children }) {
    return (
        <div className="dashboard">
            <sidebar>Sidebar</sidebar>
            {children}
        </div>
    );
}
```

---

## 3. Server Components

### Default Behavior
- All components in App Router are Server Components by default
- Render on server, send HTML to client
- Can directly access databases, files

```javascript
// This runs on server
export default async function Page() {
    const res = await fetch('https://api.example.com/data');
    const data = await res.json();
    
    return <div>{data.map(item => <li key={item.id}>{item.name}</li>)}</div>;
}
```

### Client Components
```javascript
'use client';  // Must be at top

import { useState } from 'react';

export default function Counter() {
    const [count, setCount] = useState(0);
    
    return (
        <button onClick={() => setCount(c => c + 1)}>
            Count: {count}
        </button>
    );
}
```

### When to Use Client Components
- Using React hooks: useState, useEffect, useRef
- Event handlers: onClick, onChange
- Browser-only APIs
- Using custom hooks that use state/effects

---

## 4. Dynamic Routes

### Syntax
```
app/user/[id]/page.js → /user/1, /user/2, etc.
app/blog/[category]/[slug]/page.js → /blog/react/intro
```

### useParams
```javascript
// app/user/[id]/page.js
import Link from 'next/link';

export default function UserProfile({ params }) {
    const { id } = params;
    
    return (
        <div>
            <h1>User ID: {id}</h1>
            <Link href={`/user/${parseInt(id) + 1}`}>
                Next User
            </Link>
        </div>
    );
}
```

### Multiple Parameters
```javascript
// app/blog/[category]/[slug]/page.js
export default function BlogPost({ params }) {
    const { category, slug } = params;
    // category = "react", slug = "intro"
    return <h1>{category}: {slug}</h1>;
}
```

### Dynamic Segments with catch-all
```
app/docs/[...slug]/page.js → /docs/a, /docs/a/b, /docs/a/b/c
```

---

## 5. API Routes

### Basic API Route
```javascript
// app/api/users/route.js
import { NextResponse } from 'next/server';

export async function GET() {
    const users = [
        { id: 1, name: "John" },
        { id: 2, name: "Jane" }
    ];
    
    return NextResponse.json(users);
}

export async function POST(request) {
    const body = await request.json();
    
    const newUser = {
        id: Date.now(),
        ...body
    };
    
    return NextResponse.json(newUser, { status: 201 });
}
```

### GET with Query Parameters
```javascript
// app/api/users/route.js
export async function GET(request) {
    const { searchParams } = new URL(request.url);
    const name = searchParams.get('name');
    
    const users = [
        { id: 1, name: "John" },
        { id: 2, name: "Jane" }
    ];
    
    if (name) {
        return NextResponse.json(
            users.filter(u => u.name.includes(name))
        );
    }
    
    return NextResponse.json(users);
}
```

### Route Parameters
```
app/api/users/[id]/route.js
```
```javascript
export async function GET(request, { params }) {
    const { id } = params;
    // Fetch user with id
    return NextResponse.json({ id, name: "John" });
}
```

---

## 6. Server Actions

### Define Server Action
```javascript
// actions/form.js
'use server';

export async function createUser(formData) {
    const name = formData.get('name');
    const email = formData.get('email');
    
    // Database operation
    await db.user.create({
        name,
        email
    });
    
    revalidatePath('/users');
    return { success: true };
}
```

### Use in Client Component
```javascript
// app/page.js
'use client';

import { createUser } from '@/actions/form';

export default function Form() {
    return (
        <form action={createUser}>
            <input name="name" type="text" />
            <input name="email" type="email" />
            <button type="submit">Submit</button>
        </form>
    );
}
```

### With useFormStatus
```javascript
'use client';

import { useFormStatus } from 'react-dom';

function SubmitButton() {
    const { pending } = useFormStatus();
    
    return (
        <button type="submit" disabled={pending}>
            {pending ? 'Submitting...' : 'Submit'}
        </button>
    );
}
```

---

## 7. Middleware

### Basic Middleware
```javascript
// middleware.js (root)
import { NextResponse } from 'next/server';

export function middleware(request) {
    // Check for token
    const token = request.cookies.get('token');
    
    if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
        return NextResponse.redirect(new URL('/login', request.url));
    }
    
    return NextResponse.next();
}

// Configure which routes to run on
export const config = {
    matcher: ['/dashboard/:path*', '/profile/:path*']
};
```

### Using NextResponse
```javascript
export function middleware(request) {
    // Redirect
    return NextResponse.redirect(new URL('/login', request.url));
    
    // Rewrite (internal)
    return NextResponse.rewrite(new URL('/old-path', request.url));
    
    // Set header
    const response = NextResponse.next();
    response.headers.set('x-custom-header', 'value');
    return response;
}
```

---

## 8. Authentication (Auth.js)

### Setup
```bash
npm install next-auth
```

### Basic Setup
```javascript
// app/api/auth/[...nextauth]/route.js
import NextAuth from "next-auth";
import GithubProvider from "next-auth/providers/github";

const handler = NextAuth({
    providers: [
        GithubProvider({
            clientId: process.env.GITHUB_ID,
            clientSecret: process.env.GITHUB_SECRET
        })
    ],
    pages: {
        signIn: '/login'
    }
});

export { handler as GET, handler as POST };
```

### Session in Server Component
```javascript
import { getServerSession } from "next-auth";

export default async function Page() {
    const session = await getServerSession();
    
    if (!session) {
        return <p>Please login</p>;
    }
    
    return <p>Welcome, {session.user.name}</p>;
}
```

### Session in Client Component
```javascript
'use client';

import { useSession, signIn, signOut } from "next-auth/react";

export default function AuthButton() {
    const { data: session } = useSession();
    
    if (session) {
        return (
            <button onClick={() => signOut()}>
                Sign out
            </button>
        );
    }
    
    return <button onClick={() => signIn()}>Sign in</button>;
}
```

---

## 9. Rendering Strategies

### SSR (Server-Side Rendering)
```javascript
// Always runs on server
export const dynamic = 'force-dynamic';

export default async function Page() {
    const data = await fetchData();
    return <div>{data.title}</div>;
}
```

### SSG (Static Site Generation)
```javascript
// Builds at build time
export default async function Page() {
    const data = await fetchData();
    return <div>{data.title}</div>;
}
```

### ISR (Incremental Static Regeneration)
```javascript
// Revalidate every 60 seconds
export const revalidate = 60;

export default async function Page() {
    const data = await fetchData();
    return <div>{data.title}</div>;
}
```

### Static Generation with Dynamic Data
```javascript
// Generate static paths
export async function generateStaticParams() {
    const posts = await fetchPosts();
    
    return posts.map((post) => ({
        slug: post.slug
    }));
}
```

---

## 10. Image Component

```javascript
import Image from 'next/image';

export default function Page() {
    return (
        <Image
            src="/hero.jpg"
            alt="Hero Image"
            width={800}
            height={600}
            priority      // Preload
            placeholder="blur"
            blurDataURL="data:image/..."
        />
    );
}
```

### External Images (next.config.js)
```javascript
module.exports = {
    images: {
        remotePatterns: [
            {
                protocol: 'https',
                hostname: 'images.unsplash.com'
            }
        ]
    }
};
```

---

## 11. Environment Variables

### .env.local
```env
# Public (client-side)
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_SITE_NAME=MyApp

# Private (server-side only)
DATABASE_URL=postgres://...
API_SECRET=secret123
```

### Usage
```javascript
// Server-side
const db = process.env.DATABASE_URL;

// Client-side (with NEXT_PUBLIC_)
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

---

## 12. Styling in Next.js

### Global CSS
```javascript
// app/globals.css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
    background: white;
    color: black;
}
```

### CSS Modules
```css
/* app/page.module.css */
.container {
    padding: 20px;
}

.title {
    font-size: 2rem;
}
```

```javascript
// app/page.js
import styles from './page.module.css';

export default function Page() {
    return (
        <div className={styles.container}>
            <h1 className={styles.title}>Title</h1>
        </div>
    );
}
```

### Tailwind CSS
```javascript
// app/page.js
export default function Page() {
    return (
        <div className="p-4 bg-blue-500">
            <h1 className="text-2xl font-bold">Hello</h1>
        </div>
    );
}
```

### Styled JSX
```javascript
export default function Page() {
    return (
        <div>
            <h1 className="title">Hello</h1>
            <style jsx>{`
                .title {
                    font-size: 2rem;
                    color: blue;
                }
            `}</style>
        </div>
    );
}
```

---

## 13. Important Interview Questions

### Q1: What is the difference between Next.js and React?
- **React**: UI library, client-side only
- **Next.js**: Framework built on React with routing, SSR, SSG, API routes

### Q2: What is App Router vs Pages Router?
- **Pages Router**: File-based routing with pages/
- **App Router**: Newer, uses app/ with layouts, server components

### Q3: What is the difference between SSR, SSG, and ISR?
- **SSR**: Render on every request
- **SSG**: Build once at compile time
- **ISR**: Build once, regenerate periodically

### Q4: What are Server Components?
- Components that render on server
- Can access database, files directly
- Reduce client-side JavaScript

### Q5: What is the purpose of 'use client'?
- Marks component as client-side
- Enables React hooks and event handlers

### Q6: How does routing work in Next.js?
- File-based routing in app/ directory
- Folders define routes, page.js defines content

### Q7: What is the difference between getServerSideProps and App Router?
- **getServerSideProps**: Pages router, runs on each request
- **App Router**: Server components by default, use fetch with cache options

### Q8: What are API routes in Next.js?
- Backend endpoints in app/api/
- Run on server, can access database

### Q9: How do you handle authentication in Next.js?
- NextAuth.js (Auth.js)
- Middleware for route protection
- Session management

### Q10: What is revalidate in Next.js?
- ISR - revalidate after time period
- `export const revalidate = 60` (seconds)

### Q11: What is the Image component in Next.js?
- Optimizes images automatically
- Lazy loading, resize, format conversion

### Q12: How do you deploy Next.js?
- Vercel (recommended)
- Netlify, AWS, Docker

### Q13: What is middleware in Next.js?
- Code runs before request completes
- Used for redirects, rewrites, authentication

### Q14: What is the difference between dynamic and static in Next.js?
- Dynamic: Server-rendered per request
- Static: Pre-built HTML at build time

### Q15: What are Server Actions?
- Run functions on server from client
- Can replace API routes for form submissions

### Q16: How does Next.js optimize performance?
- Server components
- Image optimization
- Font optimization
- Code splitting (automatic)

### Q17: What is the purpose of layout.js?
- Defines shared UI (nav, footer)
- Persists across route changes
- Can be nested

### Q18: What is the difference between generateStaticParams and getStaticPaths?
- **getStaticPaths**: Pages router
- **generateStaticParams**: App router

---

## Next.js Cheat Sheet

```bash
# Create app
npx create-next-app@latest my-app

# Run dev
npm run dev

# Build
npm run build

# Start production
npm start
```

```javascript
// Server component (default)
export default function Page() { }

// Client component
'use client';
export default function ClientComp() { }

// Dynamic route
app/blog/[slug]/page.js

// API route
app/api/posts/route.js

// Server action
export async function action() { }

// Revalidate ISR
export const revalidate = 60;
```

---
**End of Next.js Notes**