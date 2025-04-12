---
slug: publishing-npm-package
title: Build and Publish a Custom React + Tailwind CSS Component Library (with Webpack & Babel)
authors: [sahil]
tags: [npm, react, javascript, babel, webpack]
---

If you’ve ever wanted to build your **own reusable React component library** with **Tailwind CSS**, but didn’t want the overhead of TypeScript or complex setups—this post is for you!

<!-- truncate -->

In this guide, we’ll build a **simple, fully customizable component library** using:

- **JavaScript**
- **Tailwind CSS 3**
- **Webpack + Babel**
- **Ready to publish to npm**

We'll even create a sample `Typography` component to get started.

---

## Goal

We’re going to build a package like:

```
@sahilphondekar/react-tailwind-library
```

Ready to use in any React app with:

```bash
npm install @sahilphondekar/react-tailwind-library
```

---

## Final Folder Structure

Here’s a sneak peek of what we’ll end up with:

```
react-tailwind-library/
├── dist/
├── src/
│   ├── components/
│   │   └── Typography/
│   │       ├── Typography.jsx
│   │       └── index.js
│   ├── index.js
│   └── styles/
│       └── tailwind.css
├── .babelrc
├── .gitignore
├── package.json
├── postcss.config.js
├── tailwind.config.js
├── webpack.config.js
└── README.md
```

---

## 🔧 Step 1: Initialize the Project

```bash
mkdir react-tailwind-library
cd react-tailwind-library
npm init -y
```

Edit your `package.json`:

```json
{
  "name": "@sahilphondekar/react-tailwind-library",
  "main": "dist/index.js",
  "peerDependencies": {
    "react": ">=16.8.0",
    "react-dom": ">=16.8.0"
  },
  "scripts": {
    "build": "webpack"
  }
}
```

---

## Step 2: Install Dependencies

### Runtime Dependencies

```bash
npm install react react-dom
```

### Dev + Build Tools

```bash
npm install -D webpack webpack-cli babel-loader \
@babel/core @babel/preset-env @babel/preset-react \
tailwindcss@3 postcss postcss-loader autoprefixer \
css-loader style-loader
```

---

## Step 3: Babel Setup

Create a `.babelrc` file:

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

---

## Step 4: Webpack Config

Create a `webpack.config.js`:

```js
const path = require("path");

module.exports = {
  mode: "production",
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "index.js",
    library: "@sahilphondekar/react-tailwind-library",
    libraryTarget: "umd",
    clean: true,
  },
  externals: {
    react: "react",
    "react-dom": "react-dom"
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: "babel-loader"
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"]
      }
    ]
  },
  resolve: {
    extensions: [".js", ".jsx"]
  }
};
```

---

## Step 5: Tailwind CSS Setup

Initialize Tailwind:

```bash
npx tailwindcss init
```

### `tailwind.config.js`

```js
module.exports = {
  content: ["./src/**/*.{js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### `postcss.config.js`

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

### `src/styles/tailwind.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## Step 6: Create the Typography Component

### `src/components/Typography/Typography.jsx`

```jsx
import React from "react";
import "../../styles/tailwind.css";

const Typography = ({ variant = "body", children, className = "" }) => {
  const styles = {
    h1: "text-4xl font-bold",
    h2: "text-3xl font-semibold",
    h3: "text-2xl font-semibold",
    h4: "text-xl font-medium",
    h5: "text-lg font-medium",
    h6: "text-base font-medium",
    body: "text-base",
    caption: "text-sm text-gray-500",
  };

  const Tag = ["h1", "h2", "h3", "h4", "h5", "h6"].includes(variant)
    ? variant
    : "p";

  return <Tag className={`${styles[variant]} ${className}`}>{children}</Tag>;
};

export default Typography;
```

### `src/components/Typography/index.js`

```js
export { default } from "./Typography";
```

---

## Step 7: Entry Point

### `src/index.js`

```js
import Typography from "./components/Typography";

export { Typography };
```

---

## Step 8: Build & Publish

### Build the package

```bash
npm run build
```

### Publish to npm

```bash
npm publish --access public
```

> Make sure you’re logged into npm with `npm login` and have a unique package name.

---

## Step 9: Test it in a React App

Install the package:

```bash
npm install @sahilphondekar/react-tailwind-library
```

Use it:

```jsx
import { Typography } from "@sahilphondekar/react-tailwind-library";

export default function App() {
  return (
    <div>
      <Typography variant="h1">Hello UI Kit!</Typography>
    </div>
  );
}
```