---
sidebar_position: 3
---

# React Essentials

## Components

### What is a Component in React?
- A component is a reusable piece of code that defines part of your UI (like a button, header, form, or list).

- Components can be thought of as building blocks of a React app. They allow you to break down the UI into smaller, more manageable parts.

- Components are like functions that return a part of the UI (in the form of JSX).

### Types of Components

1. Functional Component

    - These are simpler components defined using functions.

    - They are used for components that don't need to manage complex logic or internal state (though with React hooks, they can now handle state too).

    - Example:
    ```jsx
    function Welcome() {
        return <h1>Hello, welcome to React!</h1>;
    }   
    ```

2. Class Components (Older way):

    - Class components were the main way of creating components in React before React Hooks were introduced.

    - They are defined using ES6 class syntax and can hold state and lifecycle methods.

    - Example:

    ```jsx
    class Welcome extends React.Component {
        render() {
            return <h1>Hello, welcome to React!</h1>;
        }
    }
    ```
    - **Note**: Nowadays, functional components are preferred over class components in most cases.

### JSX (JavaScript XML)

- JSX allows you to write HTML-like code inside your JavaScript, which React then transforms into actual HTML.

- JSX makes it easier to define UI elements and write cleaner code.

- Example of JSX:
```jsx
const element = <h1>Hello, world!</h1>;
```

- JSX is not mandatory in React, but it's commonly used because it’s more readable and concise.

### How Components are Used

- Once a component is created, it can be rendered into the DOM (browser screen) and reused in other parts of the app.

- Parent components can pass props (data) to child components.

- Example:
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return <Welcome name="John" />;
}
```

### Props (Properties)

- Props are the inputs or arguments that you pass to a component from its parent.

- Props allow you to pass data from one component to another (from parent to child).

- Props are read-only, which means the child component cannot change the props it receives from the parent.

- Example:
```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return <Greeting name="Alice" />;
}
```

### State

- State allows a component to maintain and manage dynamic data that can change over time.

- When the state of a component changes, it re-renders to reflect the new data in the UI.

- State is typically used in components that need to interact with users (e.g., forms, buttons).

- Example of using state in a functional component with hooks:
```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare a state variable named 'count'
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

### Event Handling in Components

- React uses event handlers to capture user interactions, like clicks, typing, and form submissions.

- Example
```jsx
function MyButton() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <button onClick={handleClick}>Click me</button>;
}
```

### Component Lifecycle (for Class Components)

- Each React component has a lifecycle, which includes:

    1. Mounting (when the component is created and inserted into the DOM).

    2. Updating (when the component’s state or props change).

    3. Unmounting (when the component is removed from the DOM).

- Class components have special methods to handle each of these lifecycle stages, like `componentDidMount()` and `componentDidUpdate()`

- Example (for class components):
```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    console.log('Component mounted!');
  }

  render() {
    return <h1>Hello World</h1>;
  }
}
```

- Note: With functional components, you use React hooks like `useEffect` to handle lifecycle events.

### React Fragments

- React Fragments are used to group multiple elements without introducing extra nodes in the DOM.

- You can use either React.Fragment or the shorthand <> </>.

- Fragments help keep your app’s DOM structure cleaner and more efficient.

- Example:
```jsx
function MyComponent() {
  return (
    <>
      <h1>Title</h1>
      <p>This is some text</p>
    </>
  );
}
```

