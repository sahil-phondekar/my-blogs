---
slug: publishing-npm-package-vite
title: Creating a Customizable React Component Library with Vite and Pure CSS
authors: [sahil]
tags: [npm, react, javascript, vite]
---

Building a reusable React component library can streamline UI development across multiple projects. In this blog, we'll create a **customizable React component library** using:

- **Vite** (for fast builds)
- **Pure CSS** (for styling)
- **CSS Modules** (for scoped styles)

<!-- truncate -->

## Step 1: Project Setup

### 1. Initialize the Project
```bash
mkdir react-component-library  
cd react-component-library  
npm init -y  
```

### 2. Install Dependencies
```bash
npm install react react-dom  
npm install vite @vitejs/plugin-react --save-dev  
```

### 3. Configure Vite (`vite.config.js`)
```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
    plugins: [react({
        include: '**/*.{jsx,tsx}',
    })],
    build: {
        lib: {
            entry: 'src/index.js',
            name: 'ReactComponentLibrary',
            fileName: (format) => `react-component-library.${format}.js`,
            formats: ['es']
        },
        rollupOptions: {
            external: ['react', 'react-dom'],
            output: {
                globals: {
                    react: 'React',
                    'react-dom': 'ReactDOM'
                }
            }
        },
        cssCodeSplit: false
    }
});
```

## Step 2: Create a Reusable Button Component

### 1. Folder Structure
```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.module.css
│   │   └── index.js
│   └── index.js
└── index.js
```

### 2. Button Component (`Button.jsx`)
```jsx
import React from "react";
import styles from "./Button.module.css";

const Button = ({
                    children,
                    variant = "primary",
                    size = "medium",
                    onClick,
                    className = "",
                    disabled = false,
                }) => {
    const buttonClasses = [
        styles.btn,
        styles[variant],
        styles[size],
        disabled ? styles.disabled : "",
        className,
    ]
        .filter(Boolean)
        .join(" ");

    return (
        <button className={buttonClasses} onClick={onClick} disabled={disabled}>
            {children}
        </button>
    );
};

export default Button;
```

### 3. CSS Module (`Button.module.css`)
```css
.btn {
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-family: inherit;
    font-weight: 500;
    transition: all 0.2s ease;
    display: inline-flex;
    align-items: center;
    justify-content: center;
}

/* Variants */
.primary {
    background-color: var(--primary-color, #4a6bff);
    color: white;
}

.primary:hover {
    background-color: var(--primary-hover, #3a5bef);
}

.secondary {
    background-color: var(--secondary-color, #f0f2f5);
    color: var(--text-color, #333);
}

.secondary:hover {
    background-color: var(--secondary-hover, #e0e2e5);
}

/* Sizes */
.small {
    padding: 6px 12px;
    font-size: 12px;
}

.medium {
    padding: 8px 16px;
    font-size: 14px;
}

.large {
    padding: 12px 24px;
    font-size: 16px;
}

/* Disabled state */
.disabled {
    opacity: 0.6;
    cursor: not-allowed;
}
```

---

## Step 3: Bundle & Publish to npm

### 1. Export Components (`src/index.js`)
```javascript
export { default as Button } from './components/Button';
```

### 2. Build the Library
```bash
npm run build
```

### 3. Publish to npm
```bash
npm login
npm publish --access public
```

---

## Step 4: Using the Library in a React App**

### 1. Install the Library**
```bash
npm install @sahilphondekar/react-component-library
```

### 2. Import CSS in Your Root Component
```jsx
import "@sahilphondekar/react-component-library/dist/style.css";
```

### 3. Use the Button Component
```jsx
import { Button } from '@sahilphondekar/react-component-library';

function App() {
  return (
    <Button variant="primary" size="large" onClick={() => alert('Clicked!')}>
      Click Me
    </Button>
  );
}
```

## Customization with CSS Variables
Override default styles in your app's CSS:
```css
:root {
  --primary-color: #ff6b6b;
  --primary-hover: #ff5252;
  --secondary-color: #f8f9fa;
}
```

## GitHub & npm Links
- **GitHub:** [github.com/sahil-phondekar/react-component-library](https://github.com/sahil-phondekar/react-component-library)
- **npm:** [npmjs.com/package/@sahilphondekar/react-component-library](https://www.npmjs.com/package/@sahilphondekar/react-component-library)

## Conclusion
You’ve now built a **customizable React component library** with:  
✔ **Vite** for fast builds  
✔ **CSS Modules** for scoped styling  
✔ **npm publish** for reusability

Extend it by adding more components (Card, Input, Modal) and themes! 🚀

**Happy coding!** 🎉