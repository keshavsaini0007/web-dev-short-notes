# Tailwind CSS Notes - For B.Tech Placements

## Table of Contents
1. [Introduction](#introduction)
2. [Setup](#setup)
3. [Utility Classes](#utility-classes)
4. [Responsive Design](#responsive-design)
5. [State Variants](#state-variants)
6. [Custom Configuration](#custom-configuration)
7. [Interview Questions](#interview-questions)

---

## 1. Introduction

### What is Tailwind CSS?
- Utility-first CSS framework
- No pre-built components
- Low-level utility classes
- Highly customizable

### Why Tailwind?
- No context switching (HTML + CSS in one file)
- Small CSS bundle (tree-shaking)
- Responsive built-in
- Easy customization

---

## 2. Setup

### With Vite
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Configuration (tailwind.config.js)
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#3498db',
        secondary: '#2ecc71',
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
    },
  },
  plugins: [],
}
```

### CSS File (index.css)
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## 3. Utility Classes

### Spacing
```html
<!-- Margin -->
<div class="m-4">All sides</div>
<div class="mx-4">Horizontal</div>
<div class="my-4">Vertical</div>
<div class="mt-4">Top</div>
<div class="mb-4">Bottom</div>
<div class="ml-4">Left</div>
<div class="mr-4">Right</div>

<!-- Padding -->
<div class="p-4">All sides</div>
<div class="px-4">Horizontal</div>
<div class="py-4">Vertical</div>

<!-- Gap -->
<div class="gap-4">Grid/Flex gap</div>
<div class="gap-x-4">Column gap</div>
<div class="gap-y-4">Row gap</div>
```

### Colors
```html
<!-- Text -->
<p class="text-blue-500">Blue text</p>
<p class="text-red-700">Dark red</p>
<p class="text-gray-500">Gray</p>
<p class="text-white">White</p>
<p class="text-black">Black</p>

<!-- Background -->
<div class="bg-blue-500">Blue bg</div>
<div class="bg-red-100">Light red</div>
<div class="bg-white">White</div>
<div class="bg-transparent">Transparent</div>

<!-- Opacity -->
<div class="bg-blue-500/50">50% opacity</div>
<div class="text-white/75">75% opacity</div>
```

### Typography
```html
<!-- Font Size -->
<p class="text-xs">Extra small</p>
<p class="text-sm">Small</p>
<p class="text-base">Base</p>
<p class="text-lg">Large</p>
<p class="text-xl">Extra large</p>
<p class="text-2xl">2XL</p>
<p class="text-4xl">4XL</p>

<!-- Font Weight -->
<p class="font-light">Light (300)</p>
<p class="font-normal">Normal (400)</p>
<p class="font-medium">Medium (500)</p>
<p class="font-semibold">Semibold (600)</p>
<p class="font-bold">Bold (700)</p>

<!-- Text Align -->
<p class="text-left">Left</p>
<p class="text-center">Center</p>
<p class="text-right">Right</p>
<p class="text-justify">Justify</p>

<!-- Text Decoration -->
<p class="underline">Underline</p>
<p class="line-through">Line through</p>
<p class="no-underline">No underline</p>

<!-- Text Transform -->
<p class="uppercase">UPPERCASE</p>
<p class="lowercase">lowercase</p>
<p class="capitalize">Capitalize</p>
```

### Sizing
```html
<!-- Width -->
<div class="w-1">4px</div>
<div class="w-4">16px</div>
<div class="w-10">40px</div>
<div class="w-1/2">50%</div>
<div class="w-full">100%</div>
<div class="w-auto">Auto</div>
<div class="w-screen">100vw</div>

<!-- Height -->
<div class="h-4">16px</div>
<div class="h-1/2">50%</div>
<div class="h-full">100%</div>
<div class="h-screen">100vh</div>

<!-- Min/Max -->
<div class="min-h-screen">Min height 100vh</div>
<div class="max-w-md">Max width</div>
<div class="max-h-screen">Max height</div>
```

### Flexbox
```html
<!-- Display -->
<div class="flex">Flex container</div>
<div class="inline-flex">Inline flex</div>

<!-- Direction -->
<div class="flex-row">Left to right</div>
<div class="flex-row-reverse">Reverse</div>
<div class="flex-col">Top to bottom</div>
<div class="flex-col-reverse">Reverse</div>

<!-- Justify Content -->
<div class="justify-start">Start</div>
<div class="justify-center">Center</div>
<div class="justify-end">End</div>
<div class="justify-between">Between</div>
<div class="justify-around">Around</div>
<div class="justify-evenly">Evenly</div>

<!-- Align Items -->
<div class="items-start">Top</div>
<div class="items-center">Center</div>
<div class="items-end">Bottom</div>
<div class="items-stretch">Stretch</div>

<!-- Flex Wrap -->
<div class="flex-nowrap">No wrap</div>
<div class="flex-wrap">Wrap</div>
<div class="flex-wrap-reverse">Reverse wrap</div>

<!-- Flex Properties -->
<div class="flex-1">flex: 1 1 0%</div>
<div class="flex-auto">flex: 1 1 auto</div>
<div class="flex-none">flex: none</div>
<div class="flex-grow">flex-grow: 1</div>
<div class="flex-shrink-0">No shrink</div>
```

### CSS Grid
```html
<!-- Display -->
<div class="grid">Grid container</div>

<!-- Columns -->
<div class="grid-cols-1">1 column</div>
<div class="grid-cols-2">2 columns</div>
<div class="grid-cols-3">3 columns</div>
<div class="grid-cols-4">4 columns</div>
<div class="grid-cols-12">12 columns</div>

<!-- Spans -->
<div class="col-span-2">Span 2 columns</div>
<div class="col-start-2">Start at 2</div>

<!-- Rows -->
<div class="grid-rows-1">1 row</div>
<div class="grid-rows-3">3 rows</div>
<div class="row-span-2">Span 2 rows</div>
```

### Borders
```html
<!-- Border Width -->
<div class="border">1px border</div>
<div class="border-2">2px border</div>
<div class="border-4">4px border</div>
<div class="border-8">8px border</div>

<!-- Border Sides -->
<div class="border-t">Top</div>
<div class="border-b">Bottom</div>
<div class="border-l">Left</div>
<div class="border-r">Right</div>

<!-- Border Colors -->
<div class="border-blue-500">Blue</div>
<div class="border-red-500/50">50% opacity</div>

<!-- Border Radius -->
<div class="rounded">4px</div>
<div class="rounded-sm">2px</div>
<div class="rounded-lg">8px</div>
<div class="rounded-xl">12px</div>
<div class="rounded-2xl">16px</div>
<div class="rounded-full">Circle/Full</div>

<!-- Radius Sides -->
<div class="rounded-t">Top</div>
<div class="rounded-b">Bottom</div>
<div class="rounded-l">Left</div>
<div class="rounded-r">Right</div>
<div class="rounded-tl">Top-left</div>
```

### Position
```html
<div class="static">Static</div>
<div class="relative">Relative</div>
<div class="absolute">Absolute</div>
<div class="fixed">Fixed</div>
<div class="sticky">Sticky</div>

<!-- Top/Right/Bottom/Left -->
<div class="top-0">Top 0</div>
<div class="right-0">Right 0</div>
<div class="bottom-0">Bottom 0</div>
<div class="left-0">Left 0</div>
<div class="inset-0">All 0</div>
```

### Display
```html
<div class="block">Block</div>
<div class="inline">Inline</div>
<div class="inline-block">Inline block</div>
<div class="hidden">Hidden</div>
<div class="contents">Contents</div>
```

### Shadows
```html
<div class="shadow">Small shadow</div>
<div class="shadow-md">Medium</div>
<div class="shadow-lg">Large</div>
<div class="shadow-xl">Extra large</div>
<div class="shadow-2xl">2XL</div>
<div class="shadow-inner">Inner</div>
<div class="shadow-none">No shadow</div>
```

### Transitions
```html
<!-- Transition Property -->
<div class="transition">Default</div>
<div class="transition-none">None</div>
<div class="transition-colors">Colors only</div>
<div class="transition-opacity">Opacity</div>
<div class="transition-transform">Transform</div>

<!-- Duration -->
<div class="duration-75">75ms</div>
<div class="duration-100">100ms</div>
<div class="duration-200">200ms</div>
<div class="duration-300">300ms</div>
<div class="duration-500">500ms</div>

<!-- Timing -->
<div class="ease-linear">Linear</div>
<div class="ease-in">In</div>
<div class="ease-out">Out</div>
<div class="ease-in-out">In-out</div>
```

### Transforms
```html
<div class="transform">Transform</div>
<div class="transform-gpu">GPU</div>
<div class="rotate-0">Rotate 0</div>
<div class="rotate-45">Rotate 45deg</div>
<div class="rotate-90">Rotate 90deg</div>
<div class="scale-50">Scale 50%</div>
<div class="scale-100">Scale 100%</div>
<div class="scale-150">Scale 150%</div>
<div class="translate-x-4">Translate X</div>
<div class="translate-y-4">Translate Y</div>
```

---

## 4. Responsive Design

### Breakpoints
| Prefix | Min Width |
|--------|-----------|
| `sm` | 640px |
| `md` | 768px |
| `lg` | 1024px |
| `xl` | 1280px |
| `2xl` | 1536px |

### Usage
```html
<!-- Mobile first (default) -->
<div class="w-full md:w-1/2 lg:w-1/3">
    Responsive width
</div>

<!-- Hide on mobile, show on desktop -->
<div class="hidden md:block">
    Visible on md and up
</div>

<!-- Different sizes -->
<button class="px-4 py-2 sm:px-6 sm:py-3 lg:px-8 lg:py-4">
    Responsive button
</button>
```

---

## 5. State Variants

### Hover
```html
<button class="bg-blue-500 hover:bg-blue-700">
    Hover me
</button>
```

### Focus
```html
<input class="border-gray-300 focus:border-blue-500 focus:ring" />
```

### Active
```html
<button class="bg-blue-500 active:bg-blue-800">
    Click me
</button>
```

### Group Hover
```html
<div class="group">
    <div class="group-hover:text-blue-500">
        When parent is hovered
    </div>
</div>
```

### Disabled
```html
<button class="disabled:opacity-50 disabled:cursor-not-allowed">
    Disabled button
</button>
```

---

## 6. Custom Configuration

### Custom Colors
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          100: '#E6F2FF',
          500: '#3B82F6',
          900: '#1E3A8A',
        }
      }
    }
  }
}
```

### Custom Breakpoints
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'xs': '480px',
      '3xl': '1920px',
    }
  }
}
```

### Custom Fonts
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    fontFamily: {
      'display': ['Oswald', 'sans-serif'],
      'body': ['Open Sans', 'sans-serif'],
    }
  }
}
```

### Arbitrary Values
```html
<div class="w-[300px]">Custom width</div>
<div class="bg-[#ff0000]">Custom color</div>
<div class="text-[20px]">Custom text size</div>
```

---

## 7. Important Interview Questions

### Q1: What is Tailwind CSS?
- Utility-first CSS framework
- Low-level utility classes
- No pre-built components

### Q2: How is Tailwind different from Bootstrap?
- **Bootstrap**: Component-based, pre-styled components
- **Tailwind**: Utility-first, highly customizable, smaller bundle

### Q3: What is the difference between utility classes and custom CSS?
- **Utility**: Pre-defined, reusable classes
- **Custom**: Write your own CSS when needed

### Q4: How do you make responsive design with Tailwind?
- Prefix system: `sm:`, `md:`, `lg:`, `xl:`, `2xl:`
- Mobile-first approach

### Q5: How do you customize Tailwind?
- Edit `tailwind.config.js`
- Add custom colors, fonts, breakpoints
- Use arbitrary values

### Q6: What is JIT (Just-in-Time)?
- Tailwind v3+ compiles on demand
- Generates only used CSS
- Smaller file sizes

### Q7: How do you use hover/focus states?
- Prefix: `hover:`, `focus:`, `active:`
- Example: `hover:bg-blue-500`

### Q8: What is the purpose of `@tailwind` directives?
- `@tailwind base`: Tailwind's reset/base styles
- `@tailwind components`: Component classes
- `@tailwind utilities`: Utility classes

### Q9: How do you add custom CSS with Tailwind?
- Add to `index.css` after @tailwind directives
- Use arbitrary values `class="w-[300px]"`
- Extend in config

### Q10: What is the difference between `md:flex` and `flex md:flex`?
- `md:flex`: Apply flex on md and above
- `flex md:flex`: Apply flex by default, flex on md

### Q11: How do you handle dark mode?
- Add `darkMode: 'class'` to config
- Use `dark:` prefix: `dark:bg-gray-900`

### Q12: What are custom variants?
- Add in config: `variants: { custom: ['hover'] }`
- Use: `custom:btn`

---

## Tailwind Cheat Sheet

```html
<!-- Common classes -->
<div class="flex justify-center items-center">Center</div>
<div class="grid grid-cols-3 gap-4">Grid</div>
<div class="p-4 m-4">Spacing</div>
<div class="text-xl font-bold">Typography</div>
<div class="bg-blue-500 text-white rounded-lg">Colors</div>
<div class="shadow-lg">Shadow</div>
<div class="hidden md:block">Responsive</div>
<button class="hover:bg-blue-700">Hover</button>
```

```javascript
// tailwind.config.js
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: { extend: {} },
  plugins: [],
}
```

```css
/* index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```bash
# Commands
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
npx tailwindcss -i ./input.css -o ./output.css --watch
```

---
**End of Tailwind CSS Notes**