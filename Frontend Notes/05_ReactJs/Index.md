# React.js



### Topics to Learn

- Core of React (State or UI manipulation, JSX)
- Component Reusability
- Props
- How to Propagate Changes (Hooks)

### Additional Topics

- Routing (React doesn't have a built-in router)
- State management (Redux, Zustand, Context API)
- Class-based components (Legacy)
- BAAS App (Firebase, Supabase)

---
```
 05_ReactJs/
 ├── Index.md
 ├── Core_Fundamentals.md
 ├── Events_Forms.md
 ├── Side_Effects.md
 ├── Advance_React.md
 ├── Essential_Ecosystem.md
 ├── Performance_Optimization.md
 ├── BAAS_App.md
```

## What is React?
React is a JavaScript library for building user interfaces (UIs).
Created by: Meta Platforms.
Released: 2013.


"Instead of writing code for every possible UI state, you describe what the UI should look like for a given state, and React handles the updates efficiently."

---

### Why React Exists?
1. Component-Based Architecture
2. Declarative UI
3. Efficient Rendering (Virtual DOM)
4. Reusability
5. Large Ecosystem
6. Mobile Development (React Native)
7. SEO Friendly (SSR/SSG)

## Created with Vite or Create React App

- Vite

``` 
npm create vite@latest my_App -- --template react

cd my_App
npm install
npm run dev

``` 




- Create React App

``` 
npx create-react-app my_App
cd my_App
npm install
npm start
``` 

## Minimal Folder Structure (Vite/React)

A typical React project structure looks like this:

```text
my_App/
├── node_modules/     # Installed dependencies (auto-generated)
├── public/           # Static assets (e.g., favicon.ico, robots.txt)
├── src/              # Application source code
│   ├── assets/       # Images, fonts, and global CSS
│   ├── components/   # Reusable UI components
│   ├── App.jsx       # Root component of the application
│   ├── index.css     # Global styles for the app
│   └── main.jsx      # Entry point (mounts App to the DOM)
├── .gitignore        # Files and folders to ignore in Git
├── index.html        # Main HTML file (contains <div id="root">)
├── package.json      # Project dependencies and npm scripts
└── vite.config.js    # Vite configuration file
``` 
