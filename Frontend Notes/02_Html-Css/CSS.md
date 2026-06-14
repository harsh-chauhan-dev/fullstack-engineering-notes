# 📘 CSS — Complete Foundation Notes

> **Goal:** Understand, build, and debug layouts confidently.  
> **Rule:** No theory without practice.

---

## Table of Contents

1. [3 Ways to Add CSS](#1-3-ways-to-add-css)
2. [Selectors](#2-selectors)
3. [Box Model](#3-box-model)
4. [Display Property](#4-display-property)
5. [Positioning](#5-positioning)
6. [Flexbox](#6-flexbox)
7. [CSS Grid](#7-css-grid)
8. [Responsive Design](#8-responsive-design)
9. [Typography](#9-typography)
10. [Colors & Backgrounds](#10-colors--backgrounds)
11. [Units](#11-units)
12. [Pseudo-Classes & Pseudo-Elements](#12-pseudo-classes--pseudo-elements)
13. [Specificity](#13-specificity)
14. [Z-Index](#14-z-index)
15. [Overflow](#15-overflow)
16. [Foundation Mastery Checklist](#-foundation-mastery-checklist)

---

## 1. 3 Ways to Add CSS

### ❌ Inline (Avoid)
```html
<p style="color: red;">Hello</p>
```

### ⚠️ Internal
```html
<style>
  p { color: blue; }
</style>
```

### ✅ External (Best Practice — Always Use in Real Projects)
```html
<link rel="stylesheet" href="style.css">
```

---

## 2. Selectors

### Basic Selectors
```css
p {}        /* element */
.box {}     /* class */
#header {}  /* id */
* {}        /* universal */
```

### Combinators
```css
div p {}    /* descendant — any p inside div */
div > p {}  /* direct child only */
h1 + p {}   /* adjacent sibling — first p after h1 */
```

### Pseudo-Classes
```css
button:hover {}
input:focus {}
li:first-child {}
```

### Pseudo-Elements
```css
p::before {}
p::after {}
```

> **Rule:** Clear selectors = no layout bugs.

---

## 3. Box Model

Every HTML element is a rectangular box.

```
[ Margin ]
  [ Border ]
    [ Padding ]
      [ Content ]
```

### Example
```css
div {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}
```

### The Problem
Default CSS adds padding + border on top of width — elements get bigger than expected.

### The Fix — Always Use This
```css
* {
  box-sizing: border-box;
}
```

`border-box` → width **includes** padding & border. Width stays predictable. ✅  
Industry standard. No exceptions.

---

## 4. Display Property

Controls how an element behaves in the layout.

```css
display: block;         /* full width, new line */
display: inline;        /* only as wide as content, no width control */
display: inline-block;  /* inline but accepts width/height */
display: none;          /* removes from layout entirely */
display: flex;          /* modern 1D layout */
display: grid;          /* modern 2D layout */
```

| Value | Behavior |
|---|---|
| `block` | Full width, starts new line |
| `inline` | Only content width, no height/width control |
| `inline-block` | Inline but respects dimensions |
| `flex` | Flexible alignment and spacing |
| `grid` | Row AND column control |

---

## 5. Positioning

Controls where elements are placed on the page.

| Value | Meaning |
|---|---|
| `static` | Default — normal flow |
| `relative` | Moves from its original position; space preserved |
| `absolute` | Removed from flow; positioned to nearest `relative` parent |
| `fixed` | Fixed to viewport; doesn't scroll |
| `sticky` | Normal flow until scroll threshold, then sticks |

### Example
```css
.parent {
  position: relative; /* Required for absolute child */
}

.child {
  position: absolute;
  top: 0;
  right: 0;
}
```

> ⚠️ `absolute` needs a `relative` parent — or it positions to `<body>`.

### Sticky Header
```css
header {
  position: sticky;
  top: 0;
}
```

---

## 6. Flexbox

**1D layout system** — arranges items in a **row or column**.

### Setup
```css
.container {
  display: flex;
}
```

- **Parent** = Flex Container
- **Children** = Flex Items

### Container Properties

```css
.container {
  flex-direction: row | column;
  justify-content: flex-start | center | space-between | space-around | space-evenly;
  align-items: stretch | center | flex-start | flex-end;
  flex-wrap: nowrap | wrap;
  gap: 16px;
}
```

### Axis Logic

| `flex-direction` | Main Axis | Cross Axis |
|---|---|---|
| `row` (default) | Horizontal → | Vertical ↕ |
| `column` | Vertical ↕ | Horizontal → |

- `justify-content` → controls **main axis**
- `align-items` → controls **cross axis**

### Item Properties
```css
.item {
  flex: 1;           /* grow and fill available space equally */
  align-self: center; /* override container alignment for this item */
}
```

### Common Use Cases
- Navbar alignment
- Centering elements
- Card rows
- Footer layout

### Perfect Center
```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

---

## 7. CSS Grid

**2D layout system** — controls **rows AND columns** simultaneously.

### Setup
```css
.container {
  display: grid;
}
```

### Defining Columns & Rows
```css
.container {
  grid-template-columns: 200px 1fr 1fr;
  grid-template-rows: 100px auto;
}
```

### The `fr` Unit
```css
grid-template-columns: 1fr 2fr;
/* Total = 3 parts: col1 gets 1/3, col2 gets 2/3 */
```

`fr` makes layouts **naturally responsive**. Use it.

### Gap
```css
gap: 20px;
/* or */
row-gap: 10px;
column-gap: 20px;
```

### Repeat Function
```css
grid-template-columns: repeat(3, 1fr);
/* Same as: 1fr 1fr 1fr */
```

### Item Placement
```css
.item {
  grid-column: 1 / 3;  /* span from line 1 to 3 */
  grid-row: 1 / 2;
}

/* Shortcut */
.item {
  grid-column: span 2;
}
```

### Grid Areas (Interview Gold 🔥)
```css
/* Step 1: Define layout */
.container {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
}

/* Step 2: Assign areas */
.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

### Auto-Responsive Grid (No Media Query Needed)
```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
```

Columns auto-adjust based on screen size. Professional-grade. ✅

### Grid Alignment
```css
/* Container */
justify-items: center;   /* horizontal */
align-items: center;     /* vertical */

/* Individual item */
justify-self: end;
align-self: start;
```

### Grid vs Flexbox

| Feature | Grid | Flexbox |
|---|---|---|
| Dimensions | 2D (rows + cols) | 1D (row or col) |
| Page layout | ✅ Best | ❌ Not ideal |
| Component alignment | Good | ✅ Best |
| Use for | Dashboards, pages | Navbars, cards |

> **Rule:** Use Grid for structure, Flexbox for alignment. Use them **together**.

---

## 8. Responsive Design

### Viewport Meta Tag — Never Skip This
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Without this, your responsive CSS won't work on mobile.

### Mobile-First Approach ✅

Write styles for mobile first, then scale up.

```css
/* Mobile (default) */
.container {
  display: flex;
  flex-direction: column;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    flex-direction: row;
  }
}
```

### Media Query Syntax
```css
@media (max-width: 768px) {
  /* styles for screens 768px and below */
}

@media (min-width: 768px) {
  /* styles for screens 768px and above — mobile-first */
}
```

### Common Breakpoints
| Device | Width |
|---|---|
| Mobile | ≤ 480px |
| Large Mobile | ≤ 576px |
| Tablet | ≤ 768px |
| Small Laptop | ≤ 992px |
| Desktop | ≥ 1200px |

> Don't memorize blindly — test your design and adjust.

### Responsive Images
```css
img {
  max-width: 100%;
  height: auto;
}
```

### Responsive Container Pattern
```css
.container {
  width: 90%;
  max-width: 1200px;
  margin: 0 auto;
}
```

### Responsive Typography
```css
/* Simple */
font-size: 1rem;

/* Modern — clamp(min, preferred, max) */
font-size: clamp(1rem, 2vw, 2rem);
```

### Real Layout Strategy
| Task | Tool |
|---|---|
| Page structure | Grid |
| Component alignment | Flexbox |
| Major layout shifts | Media queries |
| Responsive cards | `auto-fit + minmax` |
| Typography scaling | `clamp()` |

---

## 9. Typography

```css
font-size: 1rem;
font-family: 'Inter', sans-serif;
font-weight: 400 | 600 | 700;
line-height: 1.5;     /* Good readability baseline */
text-align: left | center | right;
letter-spacing: 0.5px;
text-transform: uppercase | capitalize;
```

---

## 10. Colors & Backgrounds

```css
color: #333;
background-color: #fff;
background-image: url('image.jpg');
background-size: cover | contain;
background-position: center;
background-repeat: no-repeat;
```

### Color Formats
```css
color: #ff0000;           /* Hex */
color: rgb(255, 0, 0);    /* RGB */
color: rgba(255, 0, 0, 0.5); /* RGB + opacity */
color: hsl(0, 100%, 50%); /* HSL */
```

---

## 11. Units

| Unit | Type | Use For |
|---|---|---|
| `px` | Absolute | Borders, shadows |
| `%` | Relative to parent | Widths, layout |
| `rem` | Relative to root `font-size` | Font sizes, spacing |
| `em` | Relative to parent `font-size` | Component-level spacing |
| `vh` | % of viewport height | Full-screen sections |
| `vw` | % of viewport width | Full-width elements |
| `fr` | Grid fraction | Grid columns/rows |

### Best Practice
- Fonts → `rem`
- Layout widths → `%`
- Screen-size elements → `vh` / `vw`
- Grid → `fr`
- Avoid hardcoding `px` for layout

---

## 12. Pseudo-Classes & Pseudo-Elements

### Pseudo-Classes — Target Element **State**

Syntax: `selector:pseudo-class` ← single colon

#### User Interaction
```css
button:hover  { }   /* mouse over */
button:active { }   /* being clicked */
input:focus   { }   /* selected/focused */
a:visited     { }   /* visited link */
```

#### Structural
```css
li:first-child    { }   /* first sibling */
li:last-child     { }   /* last sibling */
li:nth-child(2)   { }   /* 2nd child */
li:nth-child(odd) { }   /* odd items */
```

#### Form State
```css
input:disabled { }
input:checked  { }
input:required { }
input:valid    { }
input:invalid  { }
```

---

### Pseudo-Elements — Target **Part** of Element

Syntax: `selector::pseudo-element` ← double colon

```css
p::before {
  content: "🔥 ";   /* content property is MANDATORY */
}

p::after {
  content: " ✔";
}

p::first-letter {
  font-size: 2rem;
}

p::first-line {
  font-weight: bold;
}

::selection {
  background: yellow;  /* text highlight color */
}
```

---

### Key Differences

| | Pseudo-Class | Pseudo-Element |
|---|---|---|
| Syntax | `:` single colon | `::` double colon |
| Targets | Element state | Part of element |
| Creates content? | No | Yes (via `content`) |
| Example | `:hover` | `::before` |

---

### Interview Question: `:nth-child()` vs `:nth-of-type()`

```html
<div>
  <p>Para 1</p>
  <span>Span</span>
  <p>Para 2</p>
</div>
```

```css
:nth-child(2)    → selects <span>  (counts ALL siblings)
:nth-of-type(2)  → selects second <p>  (counts same type only)
```

---

## 13. Specificity

When multiple rules target the same element, specificity decides which wins.

### Priority Order (Low → High)
1. Element selector → `p {}`
2. Class selector → `.box {}`
3. ID selector → `#header {}`
4. Inline style → `style=""`

### Specificity Score
| Selector | Score |
|---|---|
| Element (`p`) | 0,0,1 |
| Class (`.box`) | 0,1,0 |
| ID (`#id`) | 1,0,0 |
| Inline style | Highest |

> ⚠️ Avoid `!important` — it breaks the cascade and causes debugging nightmares.

---

## 14. Z-Index

Controls **stacking order** of overlapping elements.

```css
.modal {
  position: fixed;
  z-index: 100;
}
```

- Higher value = appears **on top**
- Only works when `position` is `relative`, `absolute`, `fixed`, or `sticky`

---

## 15. Overflow

Controls what happens when content is larger than its container.

```css
overflow: visible;  /* Default — content spills out */
overflow: hidden;   /* Clips content */
overflow: scroll;   /* Always shows scrollbar */
overflow: auto;     /* Scrollbar only when needed ✅ */

/* Axis-specific */
overflow-x: hidden;
overflow-y: auto;
```

---

## ✅ Foundation Mastery Checklist

You're foundation-ready when you can do all of these **without referencing notes**:

- [ ] Build a navbar using Flexbox
- [ ] Create a page layout using Grid areas
- [ ] Center elements both horizontally and vertically
- [ ] Use `position: absolute` correctly inside a `relative` parent
- [ ] Fix spacing issues using the Box Model
- [ ] Write clean, mobile-first media queries
- [ ] Build a responsive card grid using `auto-fit + minmax`
- [ ] Use pseudo-classes for hover/focus states
- [ ] Apply `box-sizing: border-box` globally
- [ ] Debug layout issues in DevTools

---

## 🔥 Quick Reference: When to Use What

| Situation | Use |
|---|---|
| Page layout (header, sidebar, main, footer) | CSS Grid |
| Navbar or component alignment | Flexbox |
| Center a div | Flexbox |
| Responsive cards | Grid `auto-fit + minmax` |
| Major layout shifts across screen sizes | Media queries |
| Scalable typography | `clamp()` |
| Overlay / modal | `position: fixed` + `z-index` |
| Sticky header | `position: sticky` |

---

*Notes compiled from Module 1 CSS Foundation — 2026*