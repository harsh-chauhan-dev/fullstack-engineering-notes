## Event Handling

Event handling lets React respond to user actions like:

- Click
- Typing
- Hover
- Form Submit
- KeyPress
- KeyUp
- KeyDown
- MouseEnter
- MouseLeave

So many more events are there.

- Example of event handling:

```jsx
import { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);
 
    function message(){
        alert("Hello World");
    }

    return (
        <>
            <p>Count: {count}</p>
            <button onClick={message}>Say Hello</button>
        </>
    );
}
```

- onClick (React Event Handler)
- {message} function reference
- Don't write message() in onClick


### Arrow Function Event Handling

```jsx
import { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);
 
    return (
        <>
            <p>Count: {count}</p>
            <button onClick={() => alert("Button Clicked")}>Show Alert</button>
        </>
    );
}
```

- Passing Parameters
- Running quick logic

### Passing Parameters

```jsx
import { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);
 
    function greet(name){
        alert("Hello " + name);
    }

    return (
        <>
            <p>Count: {count}</p>
            <button onClick={() => greet("Harsh")}>Greet</button>
        </>
    );
}
```

### Event object

```jsx
import { useState } from 'react';

function App() {
 
    function handleEvent(event){
        console.log(event);
    }
    return (
        <>
            <button onClick={handleEvent}>Show Event</button>
        </>
    );
}
```
- Prevent default
- Get value
- Get target element


### Event Table

| React Event | Use Case |
| :--- | :--- |
| `onClick` | Button click |
| `onChange` | Input change |
| `onSubmit` | Form submit |
| `onMouseOver` / `onMouseEnter` | Hover / Mouse enter |
| `onKeyDown` | Key press down |
| `onKeyUp` | Key release |
| `onMouseDown` | Mouse button down |
| `onMouseUp` | Mouse button up |
| `onMouseLeave` | Mouse leave |

## Forms

Forms are used to collect data from users.

- Example of form:

```jsx
import { useState } from 'react';

function App() {
 
    function handleSubmit(event){
        event.preventDefault();
        console.log("Form Submitted");
    }

    const [name, setName] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    const handleNameChange = (e) => setName(e.target.value);
    const handleEmailChange = (e) => setEmail(e.target.value);
    const handlePasswordChange = (e) => setPassword(e.target.value);

    return (
        <>
            <form onSubmit={handleSubmit}>
                <input type="text" placeholder='Enter your name' onChange={handleNameChange} value={name} />
                <input type="email" placeholder='Enter your email' onChange={handleEmailChange} value={email} />
                <input type="password" placeholder='Enter your password' onChange={handlePasswordChange} value={password} />
                <button type="submit">Submit</button>
            </form>
        </>
    );
}
```