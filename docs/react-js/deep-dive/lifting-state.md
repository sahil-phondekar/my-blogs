---
sidebar_position: 3
---

# Lifting State Up in React

## What is "Lifting State Up"?
- **Lifting state up** means moving the state from a child component to a common parent component so that **multiple child components** can share and access that state.
- This is needed when **two or more components** need to communicate or share the same data.

## Why Lift State Up?
- React components only have access to their **own state**. If you want sibling components to share data or update each other, you need to **lift the state** to their **common parent**.
- The parent manages the state, and then passes it down to the children via **props**.

## Example: Lifting State Up

### Without Lifting State (Not Shared)
Imagine we have two child components, `ComponentA` and `ComponentB`, and we want them to share some data (like a counter). Without lifting state, each child has its own independent state.

```js
import React, { useState } from 'react';

function ComponentA() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Component A Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase A</button>
    </div>
  );
}

function ComponentB() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Component B Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase B</button>
    </div>
  );
}

export default function App() {
  return (
    <div>
      <ComponentA />
      <ComponentB />
    </div>
  );
}
```

In this example, the counts are separate for `ComponentA` and `ComponentB`.

### With Lifting State Up (Shared State)

Now, we lift the state up to the **common parent** (`App`), so both components can access and update the same state.

```js
import React, { useState } from 'react';

function ComponentA({ count, setCount }) {
  return (
    <div>
      <p>Component A Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase A</button>
    </div>
  );
}

function ComponentB({ count, setCount }) {
  return (
    <div>
      <p>Component B Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase B</button>
    </div>
  );
}

export default function App() {
  const [count, setCount] = useState(0);  // State is now in the parent (App)

  return (
    <div>
      <ComponentA count={count} setCount={setCount} />
      <ComponentB count={count} setCount={setCount} />
    </div>
  );
}
```

### Key Changes:
- **State in Parent (`App`)**: The state (`count`) and the function to update it (`setCount`) are now managed in the `App` component.
- **Pass State and Setter to Children**: The `count` and `setCount` are passed down as **props** to `ComponentA` and `ComponentB`.
- **Both Components Use Shared State**: Now, both `ComponentA` and `ComponentB` can modify and display the same `count`.

## Why Use This Approach?
- **Sharing Data Between Components**: This is useful when sibling components need to **sync** or share data.
- **Centralized State Management**: The state is managed in one place, making it easier to update and track.
