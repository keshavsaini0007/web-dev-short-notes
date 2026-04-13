# Quick Revision - Interview Ready 🚀

## HTML
- Semantic tags (header, nav, main, article, section, aside, footer)
- Forms: GET vs POST, input types
- SEO meta tags, Open Graph
- Core Web Vitals: LCP < 2.5s, FID < 100ms, CLS < 0.1

## CSS
- Box Model: content → padding → border → margin
- Position: static, relative, absolute, fixed, sticky
- Flexbox: justify-content, align-items, flex-direction
- Grid: grid-template-columns, grid-gap
- Selectors priority: ID > Class > Element
- Responsive: @media queries, mobile-first

## JavaScript
- Variables: let, const (avoid var)
- Arrays: map, filter, reduce, forEach
- Objects: destructuring, spread operator
- DOM: getElementById, querySelector, addEventListener
- Async: callbacks → promises → async/await
- Fetch API for HTTP requests
- Closures, event bubbling, hoisting

## React
- Components: functional, props
- Hooks: useState, useEffect, useRef, useContext
- useMemo/useCallback for optimization
- Conditional rendering: &&, ternary
- Lists: map with keys
- React Router: Routes, Route, Link, useParams

## Next.js
- App Router (file-based routing)
- Server Components (default) vs Client Components ('use client')
- Dynamic routes: [id]
- API Routes: app/api/...
- SSR, SSG, ISR (revalidate)
- Server Actions

## Backend
- Express.js: routing, middleware
- MongoDB + Mongoose
- CRUD: Create, Read, Update, Delete
- REST API principles
- JWT authentication

## Tailwind
- Utility-first CSS
- Responsive: sm:, md:, lg:, xl:
- State: hover:, focus:, active:
- Flex & Grid utilities

---

# Common Interview Questions Quick Answers

## HTML
**Q: div vs span?**
A: div is block-level, span is inline

**Q: GET vs POST?**
A: GET data in URL, POST in body

**Q: alt attribute purpose?**
A: Accessibility, SEO, fallback

## CSS
**Q: Box model?**
A: Content + Padding + Border + Margin

**Q: Flex vs Grid?**
A: Flex = 1D, Grid = 2D

**Q: How to center div?**
A: display: flex; justify-content: center; align-items: center;

## JavaScript
**Q: == vs ===?**
A: == loose (value), === strict (value + type)

**Q: var vs let vs const?**
A: var = function scope, let/const = block scope

**Q: What is closure?**
A: Inner function accessing outer variables

**Q: Event bubbling?**
A: Events propagate from child to parent

## React
**Q: Virtual DOM?**
A: Lightweight copy, React updates efficiently

**Q: State vs Props?**
A: Props passed from parent (read-only), State internal (mutable)

**Q: useEffect dependencies?**
A: Empty [] = run once, [dep] = run on change

**Q: Key prop purpose?**
A: Help React identify changed items

## Next.js
**Q: SSR vs SSG?**
A: SSR = every request, SSG = build time

**Q: When use use client?**
A: When using hooks or event handlers

## Backend
**Q: REST principles?**
A: Resources as nouns, proper HTTP methods

**Q: Middleware?**
A: Functions between request and response

**Q: MongoDB vs SQL?**
A: Document-based vs relational

---

# Projects to Mention in Interview
1. Netflix/Spotify Clone (HTML/CSS/JS)
2. Todo App (React)
3. Weather App (React + API)
4. Blog App (React + Express + MongoDB)
5. E-commerce (Next.js + Stripe)
6. Authentication System (NextAuth.js)

---

# Must-Know Commands
```bash
# HTML/CSS/JS
npm create vite@latest           # React setup

# React
npm run dev                     # Start dev server

# Next.js
npx create-next-app@latest      # New app

# Tailwind
npx tailwindcss init            # Init config
npm install -D tailwindcss postcss autoprefixer

# Node/Express
npm install express mongoose cors dotenv
node server.js                  # Run server

# Git
git init
git add .
git commit -m "message"
git push origin main
```

---

**Good Luck! 🎯**
