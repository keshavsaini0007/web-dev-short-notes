# HTML Notes - For B.Tech Placements

## Table of Contents
1. [Basic Structure](#basic-structure)
2. [Headings & Paragraphs](#headings--paragraphs)
3. [Links & Images](#links--images)
4. [Lists & Tables](#lists--tables)
5. [Forms & Input](#forms--input)
6. [Semantic Tags](#semantic-tags)
7. [Media Elements](#media-elements)
8. [SEO & Core Web Vitals](#seo--core-web-vitals)
9. [Interview Questions](#interview-questions)

---

## 1. Basic Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
</head>
<body>
    <!-- Content goes here -->
</body>
</html>
```

### Key Tags:
- `<!DOCTYPE html>` - Declares HTML5 document
- `<html>` - Root element
- `<head>` - Metadata container
- `<body>` - Visible content

---

## 2. Headings & Paragraphs
```html
<h1>Main Heading</h1>  <!-- Most important -->
<h2>Sub Heading</h2>
<h3>Section Title</h3>
<h4>Sub Section</h4>
<h5>Minor Heading</h5>
<h6>Smallest Heading</h6>

<p>This is a paragraph text.</p>
```

**Interview Point**: There are 6 heading levels (h1-h6). h1 should be used once per page for SEO.

---

## 3. Links & Images

### Links
```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    Click Here
</a>
```
- `href` - URL destination
- `target="_blank"` - Opens in new tab
- `rel="noopener noreferrer"` - Security (prevents tabnabbing)

### Images
```html
<img src="image.jpg" alt="Description" width="300" height="200">
```
- `alt` - Alternative text (important for accessibility & SEO)
- Lazy loading: `<img loading="lazy" src="...">`

---

## 4. Lists & Tables

### Unordered List
```html
<ul type="circle">
    <li>Item 1</li>
    <li>Item 2</li>
</ul>
```

### Ordered List
```html
<ol type="A" start="1">
    <li>First Item</li>
    <li>Second Item</li>
</ol>
```

### Table
```html
<table border="1">
    <thead>
        <tr>
            <th>Header 1</th>
            <th>Header 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Data 1</td>
            <td>Data 2</td>
        </tr>
    </tbody>
</table>
```

---

## 5. Forms & Input

```html
<form action="/submit" method="POST">
    <label for="name">Name:</label>
    <input type="text" id="name" name="username" required>
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="useremail">
    
    <label for="pass">Password:</label>
    <input type="password" id="pass" name="password">
    
    <input type="radio" name="gender" value="male"> Male
    <input type="radio" name="gender" value="female"> Female
    
    <input type="checkbox" name="terms"> I agree
    
    <select name="country">
        <option value="in">India</option>
        <option value="us">USA</option>
    </select>
    
    <textarea name="message" rows="4" cols="50"></textarea>
    
    <button type="submit">Submit</button>
</form>
```

### Input Types:
- `text`, `email`, `password`, `number`, `tel`
- `radio`, `checkbox`, `file`
- `date`, `time`, `color`
- `range`, `url`

---

## 6. Semantic Tags

```html
<header>
    <nav>Navigation Menu</nav>
</header>

<main>
    <article>
        <section>
            <h1>Article Title</h1>
            <p>Content...</p>
        </section>
    </article>
    
    <aside>Sidebar Content</aside>
</main>

<footer>
    <p>&copy; 2024 Company Name</p>
</footer>
```

### Semantic Elements:
| Tag | Purpose |
|-----|---------|
| `<header>` | Page/section header |
| `<nav>` | Navigation links |
| `<main>` | Main content area |
| `<article>` | Independent content |
| `<section>` | Thematic grouping |
| `<aside>` | Sidebar content |
| `<footer>` | Page footer |

**Interview Point**: Semantic tags improve SEO and accessibility (screen readers).

---

## 7. Media Elements

### Video
```html
<video controls width="500" poster="thumbnail.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.ogg" type="video/ogg">
    Your browser doesn't support video.
</video>
```

### Audio
```html
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
</audio>
```

### Attributes:
- `controls` - Play/pause/volume
- `autoplay` - Auto start
- `muted` - Start muted
- `loop` - Repeat
- `poster` - Video thumbnail

---

## 8. SEO & Core Web Vitals

### Meta Tags for SEO
```html
<meta name="description" content="Learn web development">
<meta name="keywords" content="HTML, CSS, JavaScript">
<meta name="author" content="Your Name">
<meta name="robots" content="index, follow">
```

### Open Graph (Social Media)
```html
<meta property="og:title" content="Title">
<meta property="og:image" content="image.jpg">
<meta property="og:url" content="https://site.com">
```

### Core Web Vitals (Important for Interview!)
| Metric | Description | Good Score |
|--------|-------------|-------------|
| LCP | Largest Contentful Paint | < 2.5s |
| FID | First Input Delay | < 100ms |
| CLS | Cumulative Layout Shift | < 0.1 |

---

## 9. Interview Questions

### Q1: What is the difference between div and span?
- `div` is a block-level element (takes full width)
- `span` is an inline element (takes only required width)

### Q2: What are data attributes?
- Custom attributes starting with `data-`
- Used to store extra information: `<div data-id="123">`

### Q3: What is the purpose of the `alt` attribute?
- Provides alternative text for images
- Used by screen readers
- Shows if image fails to load

### Q4: What is the difference between GET and POST?
| GET | POST |
|-----|------|
| Data in URL | Data in request body |
| Limited data | Large data |
| Cacheable | Not cacheable |
| For reading | For creating |

### Q5: What is the purpose of `viewport` meta tag?
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- Makes site responsive
- Sets initial zoom level

### Q6: What are void elements?
- Elements without closing tag: `<img>`, `<br>`, `<hr>`, `<input>`

### Q7: What is the difference between semantic and non-semantic tags?
- **Semantic**: Describe meaning (header, nav, article)
- **Non-semantic**: No meaning (div, span)

### Q8: What is localStorage vs sessionStorage?
- `localStorage` - Persists until explicitly deleted
- `sessionStorage` - Cleared when tab closes

---

## Quick Reference: HTML5 New Features
- `<video>`, `<audio>` - Media elements
- `<canvas>`, `<svg>` - Graphics
- `localStorage`, `sessionStorage`
- Geolocation API
- Drag and Drop API
- Web Workers

---
**End of HTML Notes**