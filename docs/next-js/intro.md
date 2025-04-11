---
sidebar_position: 2
---

## **What is Next.js?**

**Next.js** is a **React framework** that makes building **modern, fast, and scalable web applications** easier.

It provides a lot of features out-of-the-box that you’d otherwise have to manually set up in a standard React app.

---

### ✅ **Why Use Next.js Instead of Just React?**

| Feature | React (Default) | Next.js |
|--------|----------------|---------|
| Routing | Manual (e.g., `react-router`) | Built-in File-based routing |
| Server-Side Rendering (SSR) | Manual setup needed | Built-in |
| Static Site Generation (SSG) | Not supported directly | Built-in |
| API Routes | Not available | Built-in |
| SEO-Friendly | Needs setup | Built-in SEO support |
| Image Optimization | Manual | Built-in `<Image />` component |
| Performance | Depends on setup | Optimized by default |

---

### 🛠️ **Core Features of Next.js**

1. ### **File-Based Routing**
   - Pages are created using the **`pages/`** directory.
   - Every `.js`, `.jsx`, `.ts`, or `.tsx` file in `pages/` becomes a route automatically.
   - Example:
     - `pages/index.js` → `/`
     - `pages/about.js` → `/about`

2. ### **Pre-rendering**
   - Next.js can render HTML **in advance**, improving performance and SEO.
   - Two main types:
     - **Static Generation (SSG)**: HTML is generated at **build time**.
     - **Server-Side Rendering (SSR)**: HTML is generated **on each request**.

3. ### **API Routes**
   - You can create backend API endpoints by adding files inside `pages/api/`.
   - Example: `pages/api/hello.js` → `/api/hello`

4. ### **Built-in CSS Support**
   - Supports CSS, Sass, CSS Modules, and also works well with Tailwind CSS.

5. ### **Fast Refresh**
   - See changes instantly during development without losing component state.

6. ### **Image Optimization**
   - `<Image />` component automatically optimizes images for faster loading.

7. ### **Built-in Head Management**
   - Use the `next/head` component to manage HTML `<head>` tags (like title, meta).

8. ### **Dynamic Routing**
   - Create dynamic pages using brackets:
     - `pages/post/[id].js` → `/post/123`, `/post/abc`, etc.

---

### 📦 **Next.js vs Create React App (CRA)**

| Aspect | Create React App | Next.js |
|--------|------------------|---------|
| Setup | Simple | Simple |
| Routing | Manual | Automatic |
| SSR | No | Yes |
| Static Generation | No | Yes |
| SEO | Weak | Strong |
| API Routes | No | Yes |
| Code Splitting | Partial | Automatic |
| Image Optimization | No | Yes |

---

### 🚀 Use Cases for Next.js

- Blogs and Content Sites (SEO-friendly)
- E-commerce platforms
- Dashboards with dynamic + static pages
- Hybrid apps (partially static, partially dynamic)
- Any production-grade React app

---

### 📘 Summary

> **Next.js = React + Extra Superpowers for Production-Ready Web Development**

It simplifies routing, improves SEO, handles SSR/SSG, and even lets you build full-stack apps with built-in API routes. It’s a go-to choice for modern React-based apps.

---

Let me know the next topic you'd like notes on!