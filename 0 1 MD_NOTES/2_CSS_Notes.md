# CSS Notes - For B.Tech Placements

## Table of Contents
1. [Ways to Add CSS](#ways-to-add-css)
2. [Selectors](#selectors)
3. [Box Model](#box-model)
4. [Typography](#typography)
5. [Display Property](#display-property)
6. [Position Property](#position-property)
7. [Flexbox](#flexbox)
8. [CSS Grid](#css-grid)
9. [Sizing Units](#sizing-units)
10. [Transform & Transition](#transform--transition)
11. [Animation](#animation)
12. [Media Queries](#media-queries)
13. [CSS Variables](#css-variables)
14. [Important Interview Questions](#important-interview-questions)

---

## 1. Ways to Add CSS

### Inline CSS (Not Recommended)
```html
<p style="color: blue; font-size: 16px;">Text</p>
```

### Internal CSS
```html
<head>
    <style>
        p { color: blue; }
    </style>
</head>
```

### External CSS (Best Practice)
```html
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

**Interview Point**: External CSS is best for performance and maintainability.

---

## 2. Selectors

### Basic Selectors
```css
/* Element Selector */
p { color: blue; }

/* Class Selector */
.highlight { background: yellow; }

/* ID Selector */
#header { font-size: 24px; }

/* Universal Selector */
* { margin: 0; }
```

### Descendant Selectors
```css
/* Space = descendant (any level) */
div p { color: red; }

/* > = direct child */
div > p { color: red; }

/* + = adjacent sibling */
h1 + p { color: red; }

/* ~ = general sibling */
h1 ~ p { color: red; }
```

### Pseudo Classes
```css
/* State-based */
:hover      /* Mouse over */
:active     /* When clicked */
:focus      /* When focused */
:visited    /* Visited links */
:checked    /* Checked inputs */
:first-child
:last-child
:nth-child(n)
```

### Pseudo Elements
```css
::before    /* Insert before content */
::after     /* Insert after content */
::first-line
::first-letter
::selection /* Selected text */
```

### Attribute Selector
```css
[attribute]         /* Has attribute */
[type="text"]       /* Exact match */
[class~="btn"]      /* Contains word */
[href^="https"]     /* Starts with */
[href$=".pdf"]      /* Ends with */
[href*="example"]   /* Contains */
```

---

## 3. Box Model

```
┌─────────────────────────────┐
│         Margin              │
│  ┌─────────────────────┐   │
│  │       Border        │   │
│  │  ┌───────────────┐  │   │
│  │  │    Padding    │  │   │
│  │  │ ┌───────────┐ │  │   │
│  │  │ │  Content  │ │  │   │
│  │  │ └───────────┘ │  │   │
│  │  └───────────────┘  │   │
│  └─────────────────────┘   │
└─────────────────────────────┘
```

### Properties
```css
/* Margin (outside) */
margin: 10px;              /* All sides */
margin: 10px 20px;        /* T,B | L,R */
margin: 10px 20px 30px;   /* T | L,R | B */
margin: 10px 20px 30px 40px; /* T | R | B | L */

/* Padding (inside) */
padding: 15px;

/* Border */
border: 2px solid red;
border-width: 2px;
border-style: solid;
border-color: red;
border-radius: 10px;

/* Box Sizing */
box-sizing: border-box;  /* Include padding/border in width */
```

**Interview Point**: Default `box-sizing: content-box` adds padding to width. Use `border-box` for predictable layouts.

---

## 4. Typography

```css
/* Font Family */
font-family: 'Arial', sans-serif;
font-family: 'Times New Roman', serif;

/* Font Size */
font-size: 16px;
font-size: 1rem;     /* 16px by default */
font-size: 1em;      /* Parent's font size */

/* Font Weight */
font-weight: bold;
font-weight: 700;
font-weight: lighter;

/* Text Properties */
color: #ff0000;           /* Text color */
text-align: center;       /* Left, right, center, justify */
text-decoration: none;   /* none, underline, line-through */
text-transform: uppercase; /* uppercase, lowercase, capitalize */
line-height: 1.5;         /* Line spacing */
letter-spacing: 2px;      /* Space between letters */
word-spacing: 5px;        /* Space between words */

/* Text Shadow */
text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
```

---

## 5. Display Property

```css
/* Common Values */
display: block;      /* Full width, new line */
display: inline;     /* Same line, no width/height */
display: inline-block; /* Same line + can have dimensions */
display: none;       /* Hidden (no space) */
display: flex;      /* Flexbox */
display: grid;      /* CSS Grid */

/* Visibility */
visibility: hidden;  /* Hidden but takes space */
opacity: 0;          /* Transparent but takes space */
```

---

## 6. Position Property

```css
/* Static - Default */
position: static;    /* Normal flow */

/* Relative - Offset from normal position */
position: relative;
top: 10px;
left: 20px;

/* Absolute - Relative to nearest positioned ancestor */
position: absolute;
top: 0;
left: 0;

/* Fixed - Relative to viewport (stays on scroll) */
position: fixed;
top: 0;

/* Sticky - Sticks on scroll within container */
position: sticky;
top: 10px;
```

**Interview Point**: 
- Absolute positions relative to the nearest positioned ancestor (not static)
- Fixed position is always relative to viewport
- Sticky requires top/bottom values to work

---

## 7. Flexbox

### Container Properties
```css
display: flex;

/* Main Axis (row by default) */
justify-content: flex-start | center | flex-end | 
                space-between | space-around | space-evenly;

/* Cross Axis */
align-items: stretch | flex-start | center | flex-end | baseline;

/* Direction */
flex-direction: row | column | row-reverse | column-reverse;

/* Wrapping */
flex-wrap: nowrap | wrap | wrap-reverse;

/* Gap */
gap: 20px;
```

### Item Properties
```css
/* Growth */
flex-grow: 1;        /* Take extra space */
flex-shrink: 0;      /* Don't shrink */

/* Basis */
flex-basis: 200px;    /* Initial size */

/* Short-hand */
flex: 1 0 200px;     /* grow shrink basis */

/* Order */
order: 1;            /* Change display order */

/* Align Self */
align-self: auto | flex-start | center | flex-end;
```

---

## 8. CSS Grid

### Container Properties
```css
display: grid;

/* Columns */
grid-template-columns: 1fr 1fr 1fr;      /* 3 equal columns */
grid-template-columns: repeat(3, 1fr);   /* Same */
grid-template-columns: 200px auto 1fr;   /* Fixed, auto, fraction */

/* Rows */
grid-template-rows: 100px 200px;

/* Gap */
gap: 20px;
row-gap: 10px;
column-gap: 20px;

/* Areas */
grid-template-areas: 
    "header header"
    "sidebar content"
    "footer footer";
```

### Item Properties
```css
/* Spanning */
grid-column: 1 / 3;      /* Start / End */
grid-column: span 2;      /* Span 2 columns */

grid-row: 1 / 3;         /* Start / End */
grid-row: span 2;        /* Span 2 rows */

/* Area */
grid-area: header;
```

**Interview Point**: Flexbox is for 1D (row OR column), Grid is for 2D (rows AND columns).

---

## 9. Sizing Units

| Unit | Description | Relative To |
|------|-------------|--------------|
| `px` | Pixels | Absolute |
| `%` | Percentage | Parent |
| `rem` | Root EM | Root font-size (16px default) |
| `em` | EM | Parent font-size |
| `vh` | Viewport Height | 1% of viewport height |
| `vw` | Viewport Width | 1% of viewport width |
| `vmin` | Viewport Min | Smaller of vh/vw |
| `vmax` | Viewport Max | Larger of vh/vw |

**Interview Point**: 
- `rem` is better than `em` for consistent sizing
- `vh`/`vw` are great for responsive layouts

---

## 10. Transform & Transition

### Transform
```css
/* 2D Transforms */
transform: translate(20px, 10px);    /* Move */
transform: rotate(45deg);            /* Rotate */
transform: scale(1.5);                /* Scale */
transform: skew(20deg);              /* Skew */
transform: matrix(1, 0, 0, 1, 0, 0); /* All combined */

/* Combined */
transform: translate(20px, 10px) rotate(45deg);
```

### Transition
```css
/* Property to animate */
transition: property duration timing-function delay;

/* Example */
transition: all 0.3s ease-in-out;
transition: transform 0.3s, background-color 0.3s;

/* Timing Functions */
ease        /* Slow start, fast, slow end */
linear      /* Same speed */
ease-in     /* Slow start */
ease-out    /* Slow end */
ease-in-out /* Slow start and end */
```

---

## 11. Animation

### @keyframes
```css
@keyframes myAnimation {
    from { 
        opacity: 0; 
        transform: translateY(20px);
    }
    to { 
        opacity: 1; 
        transform: translateY(0);
    }
}

/* Or with percentages */
@keyframes bounce {
    0% { top: 0; }
    50% { top: 100px; }
    100% { top: 0; }
}
```

### Apply Animation
```css
.element {
    animation: name duration timing-function delay 
              iteration-count direction fill-mode;
    
    animation: bounce 1s ease infinite;
}
```

### Properties
- `iteration-count`: number or infinite
- `direction`: normal, reverse, alternate, alternate-reverse
- `fill-mode`: none, forwards, backwards, both

---

## 12. Media Queries

```css
/* Mobile First (default styles) */
body { font-size: 16px; }

/* Tablet */
@media (min-width: 768px) {
    body { font-size: 18px; }
}

/* Desktop */
@media (min-width: 1024px) {
    body { font-size: 20px; }
}

/* Specific conditions */
@media (max-width: 600px) { ... }
@media (min-width: 600px) and (max-width: 900px) { ... }
@media (orientation: portrait) { ... }
@media (hover: hover) { ... }
```

---

## 13. CSS Variables

```css
/* Define */
:root {
    --primary-color: #3498db;
    --secondary-color: #2ecc71;
    --font-size: 16px;
    --spacing: 20px;
}

/* Use */
.container {
    color: var(--primary-color);
    padding: var(--spacing);
    font-size: var(--font-size);
}

/* With fallback */
color: var(--primary-color, blue);
```

**Interview Point**: CSS variables make theming easy and are now widely supported.

---

## 14. Important Interview Questions

### Q1: What is the difference between flexbox and grid?
- **Flexbox**: One-dimensional (row OR column)
- **Grid**: Two-dimensional (rows AND columns)

### Q2: What is the difference between `display: none` and `visibility: hidden`?
- `display: none` - Removes element completely (no space)
- `visibility: hidden` - Hides element but keeps space

### Q3: What is z-index?
- Controls stacking order of positioned elements
- Only works on positioned elements (relative, absolute, fixed, sticky)
- Higher value = on top

### Q4: What is the difference between rem, em, and px?
- `px`: Absolute, fixed size
- `em`: Relative to parent font-size
- `rem`: Relative to root font-size (better for accessibility)

### Q5: What is the CSS Box Model?
- Content + Padding + Border + Margin

### Q6: What is the difference between `justify-content` and `align-items`?
- `justify-content`: Main axis (horizontal in row)
- `align-items`: Cross axis (vertical in row)

### Q7: What is specificity in CSS?
- ID > Class > Element > Universal
- Inline > Internal > External

### Q8: What is the difference between border-box and content-box?
- `content-box` (default): Width = content only
- `border-box`: Width = content + padding + border

### Q9: What is the difference between :nth-child() and :nth-of-type()?
- `:nth-child(n)`: Counts all children
- `:nth-of-type(n)`: Counts only of that type

### Q10: How to center a div?
```css
/* Flexbox (most common) */
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Grid */
.parent {
    display: grid;
    place-items: center;
}

/* Absolute positioning */
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### Q11: What is the difference between transition and animation?
- `transition`: A to B (needs trigger like :hover)
- `animation`: Can run automatically with keyframes

### Q12: What is the purpose of `box-sizing: border-box`?
- Includes padding and border in the element's total width/height
- Prevents layout breaks when adding padding

### Q13: What are CSS pseudo-classes vs pseudo-elements?
- **Pseudo-class**: Defines special state (`:hover`, `:focus`)
- **Pseudo-element**: Styles part of element (`::before`, `::after`)

### Q14: What is the difference between inline, internal, and external CSS?
- **Inline**: In HTML tag (highest priority, hard to maintain)
- **Internal**: In `<style>` tag (medium priority)
- **External**: Separate file (best for reusability)

### Q15: What is the difference between `vh` and `vw`?
- `vh`: 1% of viewport height
- `vw`: 1% of viewport width

---

## CSS Cheat Sheet

```css
/* Reset */
* { margin: 0; padding: 0; box-sizing: border-box; }

/* Flexbox Center */
display: flex; justify-content: center; align-items: center;

/* Grid Center */
display: grid; place-items: center;

/* Responsive Image */
img { max-width: 100%; height: auto; }

/* Truncate Text */
white-space: nowrap; overflow: hidden; text-overflow: ellipsis;

/* Custom Scrollbar */
::-webkit-scrollbar { width: 8px; }
::-webkit-scrollbar-thumb { background: #888; }
```

---
**End of CSS Notes**