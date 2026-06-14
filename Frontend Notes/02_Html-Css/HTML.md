# 🌐 HTML (HyperText Markup Language) – Complete Notes

---

## 1️⃣ What is HTML?

- **HTML** = Standard markup language for creating web pages
- Defines the **structure** of a webpage
- Uses **tags** to organize content
- Works with:
  - **CSS** → Styling
  - **JavaScript** → Behavior

---

## 2️⃣ HTML Document Structure Diagram

```
┌─────────────────────────────────┐
│         <!DOCTYPE html>         │  ← Declares HTML5
├─────────────────────────────────┤
│           <html>                │  ← Root element
│  ┌──────────────────────────┐   │
│  │         <head>           │   │  ← Metadata
│  │   <title>My Page</title> │   │
│  │   <meta>, <link>, <style>│   │
│  └──────────────────────────┘   │
│  ┌──────────────────────────┐   │
│  │         <body>           │   │  ← Visible content
│  │   <h1>, <p>, <img>...    │   │
│  └──────────────────────────┘   │
│           </html>               │
└─────────────────────────────────┘
```

### Example:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

| Tag               | Purpose                      |
| ----------------- | ---------------------------- |
| `<!DOCTYPE html>` | Declares HTML5               |
| `<html>`          | Root element                 |
| `<head>`          | Metadata (title, links, SEO) |
| `<body>`          | Visible content              |

---

## 3️⃣ HTML Elements & Tags

### Tag Structure Diagram

```
  Opening Tag     Content     Closing Tag
      ↓               ↓            ↓
  <tagname>   Your Content   </tagname>

  Self-Closing:
  <img src="img.jpg" />
  <br />
```

### Types of Tags

| Type         | Example                          |
| ------------ | -------------------------------- |
| Paired Tag   | `<p></p>`, `<div></div>`         |
| Self-Closing | `<img />`, `<br />`, `<input />` |

---

## 4️⃣ Headings

```html
<h1>Main Heading</h1>
<!-- Most important -->
<h2>Sub Heading</h2>
<h3>Section</h3>
<h4>Sub-section</h4>
<h5>Minor</h5>
<h6>Smallest Heading</h6>
<!-- Least important -->
```

> ✅ **Best Practice:** Use only **one `<h1>`** per page — it impacts SEO.

---

## 5️⃣ Paragraph & Text Formatting

```html
<p>This is a paragraph</p>
<b>Bold</b>
<i>Italic</i>
<strong>Important (bold + semantic)</strong>
<em>Emphasized (italic + semantic)</em>
<u>Underline</u>
<mark>Highlighted</mark>
<small>Small text</small>
<br />
<!-- Line break -->
<hr />
<!-- Horizontal rule -->
```

---

## 6️⃣ Links (Anchor Tag)

```html
<!-- Basic link -->
<a href="https://google.com">Visit Google</a>

<!-- Open in new tab -->
<a href="https://google.com" target="_blank">Open in New Tab</a>

<!-- Link to page section -->
<a href="#section1">Jump to Section 1</a>
```

| Attribute         | Purpose                     |
| ----------------- | --------------------------- |
| `href`            | URL or path                 |
| `target="_blank"` | Opens in new tab            |
| `rel="noopener"`  | Security for `_blank` links |

---

## 7️⃣ Images

```html
<img src="image.jpg" alt="A description" width="200" height="150" />
```

| Attribute          | Purpose                         |
| ------------------ | ------------------------------- |
| `src`              | Image path or URL               |
| `alt`              | Accessibility text (important!) |
| `width` / `height` | Dimensions in px                |

---

## 8️⃣ Lists

### Ordered List

```html
<ol>
  <li>Step One</li>
  <li>Step Two</li>
  <li>Step Three</li>
</ol>
```

### Unordered List

```html
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Cherry</li>
</ul>
```

### Nested List

```html
<ul>
  <li>
    Fruits
    <ul>
      <li>Apple</li>
      <li>Mango</li>
    </ul>
  </li>
</ul>
```

---

## 9️⃣ Tables

### Structure Diagram

```
┌───────────┬───────────┐
│   <th>    │   <th>    │  ← Header Row (<tr>)
├───────────┼───────────┤
│   <td>    │   <td>    │  ← Data Row (<tr>)
├───────────┼───────────┤
│   <td>    │   <td>    │
└───────────┴───────────┘
```

### Example

```html
<table border="1">
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Harsh</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
```

| Tag       | Role               |
| --------- | ------------------ |
| `<table>` | Container          |
| `<thead>` | Header section     |
| `<tbody>` | Body section       |
| `<tr>`    | Table row          |
| `<th>`    | Header cell (bold) |
| `<td>`    | Data cell          |

---

## 🔟 Forms

```html
<form action="/submit" method="POST">
  <input type="text" placeholder="Enter name" />
  <input type="email" placeholder="Enter email" />
  <input type="password" placeholder="Password" />
  <input type="radio" name="gender" value="male" /> Male
  <input type="checkbox" /> Accept Terms
  <input type="file" />
  <textarea rows="4" cols="30">Enter message...</textarea>
  <button type="submit">Submit</button>
</form>
```

### Common Input Types

| Type       | Usage                 |
| ---------- | --------------------- |
| `text`     | Plain text input      |
| `email`    | Email with validation |
| `password` | Hidden characters     |
| `radio`    | Single choice         |
| `checkbox` | Multiple choices      |
| `file`     | File upload           |
| `number`   | Numeric input         |
| `date`     | Date picker           |
| `submit`   | Submit button         |

---

## 1️⃣1️⃣ Semantic Tags (Very Important!)

### Semantic Layout Diagram

```
┌──────────────────────────────┐
│           <header>           │  ← Logo, nav bar
├──────────────────────────────┤
│            <nav>             │  ← Navigation links
├──────────────────────────────┤
│           <main>             │
│  ┌────────────┐  ┌────────┐  │
│  │  <section> │  │<aside> │  │  ← Sidebar
│  │            │  │        │  │
│  │ <article>  │  │        │  │
│  └────────────┘  └────────┘  │
├──────────────────────────────┤
│           <footer>           │  ← Footer info
└──────────────────────────────┘
```

```html
<header>Site logo and navbar</header>
<nav>Navigation links</nav>
<main>
  <section>
    <article>Blog post content</article>
  </section>
  <aside>Sidebar</aside>
</main>
<footer>Copyright info</footer>
```

> ✅ **Why it matters:** Better SEO, cleaner structure, easier backend integration.

---

## 1️⃣2️⃣ Div vs Span

| Tag      | Type   | Use                     |
| -------- | ------ | ----------------------- |
| `<div>`  | Block  | Groups sections/layouts |
| `<span>` | Inline | Styles part of text     |

```html
<!-- Block element - takes full width -->
<div style="background: lightblue;">This is a block</div>

<!-- Inline element - only wraps content -->
<p>This is <span style="color: red;">highlighted</span> text.</p>
```

---

## 1️⃣3️⃣ HTML Attributes

```html
<tag attribute="value">Content</tag>
```

| Attribute     | Purpose                     |
| ------------- | --------------------------- |
| `id`          | Unique identifier           |
| `class`       | CSS/JS targeting (reusable) |
| `style`       | Inline CSS                  |
| `href`        | Link URL                    |
| `src`         | Image/script source         |
| `alt`         | Alternative text            |
| `disabled`    | Disables input              |
| `placeholder` | Input hint text             |

---

## 1️⃣4️⃣ Block vs Inline Elements

### Visual Diagram

```
Block Elements (full width):
┌─────────────────────────────┐
│          <div>              │
├─────────────────────────────┤
│           <p>               │
└─────────────────────────────┘

Inline Elements (fit content):
┌──────┐ ┌──────────┐ ┌─────┐
│<span>│ │   <a>    │ │<img>│
└──────┘ └──────────┘ └─────┘
```

| Block Elements | Inline Elements |
| -------------- | --------------- |
| `<div>`        | `<span>`        |
| `<p>`          | `<a>`           |
| `<h1>`–`<h6>`  | `<img>`         |
| `<section>`    | `<strong>`      |
| `<ul>`, `<ol>` | `<em>`          |
| `<table>`      | `<button>`      |

---

## 1️⃣5️⃣ HTML5 Key Features

| Feature         | Description                               |
| --------------- | ----------------------------------------- |
| Semantic Tags   | `<header>`, `<footer>`, `<section>`, etc. |
| Audio/Video     | `<audio>`, `<video>` tags                 |
| Canvas          | Draw graphics with JS                     |
| Local Storage   | Store data in browser                     |
| Form Validation | `required`, `pattern`, `min`, `max`       |
| Geolocation     | Access user location                      |

```html
<!-- Audio -->
<audio controls>
  <source src="audio.mp3" type="audio/mpeg" />
</audio>

<!-- Video -->
<video width="400" controls>
  <source src="video.mp4" type="video/mp4" />
</video>

<!-- Form Validation -->
<input type="email" required placeholder="Enter email" />
<input type="number" min="1" max="100" />
```

---

## 🚀 Practical Roadmap (Full Stack Dev)

```
HTML Learning Path:
──────────────────
Phase 1 → Structure
  ✅ Tags, attributes, forms, tables

Phase 2 → Semantics
  ✅ Semantic tags, accessibility, SEO

Phase 3 → Integration
  ✅ Forms → Backend (POST/GET)
  ✅ DOM → JavaScript manipulation

Phase 4 → Practice Projects
  ✅ Portfolio page
  ✅ Login / Register page
  ✅ Blog layout
  ✅ Product card UI
```

---

## 📌 Quick Reference Cheatsheet

```html
<!-- Document -->
<!DOCTYPE html> <html> <head> <body>

<!-- Text -->
<h1>–<h6>  <p>  <b>  <i>  <strong>  <em>  <br>  <hr>

<!-- Links & Media -->
<a href="">  <img src="" alt="">  <video>  <audio>

<!-- Lists -->
<ul> <ol> <li>

<!-- Table -->
<table> <tr> <th> <td> <thead> <tbody>

<!-- Form -->
<form> <input> <textarea> <button> <select> <option>

<!-- Semantic -->
<header> <nav> <main> <section> <article> <aside> <footer>

<!-- Containers -->
<div>  <span>
```

---

_Happy coding! 🚀 Master the structure before the styling._