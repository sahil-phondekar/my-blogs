---
slug: testing-npm-package
title: How to Use `npm link` to Test NPM Packages Locally Before Publishing
authors: [sahil]
tags: [npm, react, javascript, vite]
---

When building your own NPM package (like a React component library), it's essential to test it *in a real project* before publishing to the registry. This is where `npm link` comes in handy.

In this post, we’ll walk through how to use `npm link` to test your local package in another project.

<!-- truncate -->

## What is `npm link`?

`npm link` creates a **symbolic link** between your local NPM package and another project. It allows your test app to use your local package as if it were installed from the registry.

## Step-by-Step Guide

### 1. Go to Your Package Directory

```bash
cd ~/Projects/my-component-library
```

Run the following to make it globally available:

```bash
npm link
```

> This registers your package globally on your machine.

### 2. Go to Your Test Project

Now, open your test project (where you want to use your library):

```bash
cd ~/Projects/my-app
```

Link the package:

```bash
npm link my-component-library
```

This creates a symbolic link from `node_modules/my-component-library` to your local package source folder.

### 3. Use the Package in Your App

Now you can use your package in the test app just like any other dependency:

```js
import { Button } from "my-component-library";

function App() {
  return <Button>Test Button</Button>;
}
```

## Unlinking Later

When you're done testing, clean up the link:

```bash
cd ~/Projects/my-app
npm unlink my-component-library
```

Then globally remove the link:

```bash
cd ~/Projects/my-component-library
npm unlink
```

## Conclusion

Using `npm link` is an efficient way to test your NPM packages locally. It mimics the install process while allowing rapid development and testing without publishing to the registry.
