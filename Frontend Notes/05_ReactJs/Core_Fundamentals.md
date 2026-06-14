# Core Fundamentals of React

To build solid applications in React, you must first understand the core principles that dictate how React works under the hood. There are some main concepts that define the React philosophy.

---

## 1. Declarative UI

In traditional Vanilla JavaScript (Imperative), you tell the browser *exactly how* to do something step-by-step (e.g., query this element, create a new span, add text to it, append it to the body).

In React, you tell the library **what you want the UI to look like** based on the current state. React takes care of the "how" by communicating with the DOM efficiently.

* **Analogy:** Imperative is giving someone turn-by-turn directions to the store. Declarative is giving them an address and letting them figure out the best route.

## 2. Component-Based Architecture

React applications are built out of components. Think of a component as an independent, reusable piece of code that returns a piece of the user interface.

* Components allow you to split complex interfaces into smaller, manageable chunks (e.g., a `Navbar`, a `Button`, a `Card`).
* Every React app has a "root" component (usually `App.jsx`) which contains all the other sub-components acting like a giant tree.

## 3. JSX (JavaScript XML)

JSX is the syntax extension that allows you to write HTML-like markup directly inside your JavaScript files.

* It feels like writing HTML, but it's actually closer to JavaScript. Everything written in JSX gets compiled down into standard JavaScript function calls (`React.createElement()`) behind the scenes.
* Because you are in Javascript, you can inject variables, arrays, and functions directly into your HTML blocks using curly braces `{}`.

## 4. Props (Properties)

Props are used to pass data from a parent component to a child component. They are read-only, meaning a child component cannot modify the props it receives from its parent.

```jsx
// Child Component
function Student(props) {
    return (
        <div className="card">
            <p>Name: {props.name}</p>
            <p>Age: {props.age}</p>
        </div>
    );
}

// Parent Component
function App() {
    return (
        <>
            <Student name="John" age="20" />
            <Student name="Jane" age="22" />
        </>
    );
}
```

## 5. State

State is a built-in React object that allows a component to "remember" information about itself. When the state of a component changes, React automatically re-renders the component to reflect the new data.

```jsx
import { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);

    return (
        <>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
        </>
    );
}
```

## 6. Unidirectional Data Flow

In React, data flows in **one direction**: downwards from parent to child.

* Parents pass data (`props`) down to their children.
* Children **cannot** pass data natively back up to their parents or safely modify the data given to them.
* If a child component needs to trigger a change, the parent must pass down a function as a prop, and the child will call that function. This makes debugging much easier because you always know where exactly data originates.

## 7. The Virtual DOM

Directly updating the browser's actual DOM is slow and resource-heavy. React solves this speed issue using the Virtual DOM.

1. React keeps a lightweight, Javascript copy of the actual DOM (The Virtual DOM).
2. When data (state) changes, React creates a new Virtual DOM and compares it to the previous one (a process called **Diffing**).
3. Once React identifies what exactly changed, it selectively and rapidly updates only those specific pieces in the real DOM (a process called **Reconciliation**).

## 8. Hooks & Component Lifecycle

Hooks are functions that let you "hook into" React state and lifecycle features from function components. Before hooks, you had to use class components to manage state or lifecycle methods.

* **`useState`**: The most common hook. It lets you add state to a function component.
* **`useEffect`**: Lets you perform side effects (like data fetching, subscriptions, or manually changing the DOM) in a function component. It replaces traditional lifecycle methods (`componentDidMount`, `componentDidUpdate`).
* **`useContext`**: Lets you subscribe to React context without introducing nesting (solves prop-drilling).
* **`useReducer`**: An alternative to `useState` for managing more complex state logic.
* **`useMemo` / `useCallback`**: Performance optimization hooks to memoize expensive calculations or function definitions.
