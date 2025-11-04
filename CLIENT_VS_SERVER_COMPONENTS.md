# Client vs Server Components in Next.js 13+

## Overview

In Next.js 13+ (App Router), components are **Server Components by default**. You only need to add `'use client'` when you need client-side features.

---

## 🔴 Server Components (Default)

**What they are:**

- Components that render **only on the server**
- HTML is generated on the server and sent to the browser
- **No JavaScript** is sent to the browser for these components
- Cannot use React hooks or browser APIs

**When to use:**

- Fetching data from databases/APIs
- Accessing backend resources
- Large dependencies that shouldn't be in the client bundle
- Static content
- SEO-friendly content

**Example from your codebase:**

```tsx
// app/api/health/route.ts - This is a Server Component (API Route)
import { NextResponse } from "next/server";

export async function GET() {
  // This runs ONLY on the server
  const response = await fetch("http://localhost:3001/api/health");
  const data = await response.json();
  return NextResponse.json(data);
}
```

**What a Server Component CAN do:**
✅ Fetch data directly from databases
✅ Use `async/await` at the top level
✅ Access file system or server-only APIs
✅ Keep sensitive information (API keys, tokens) on the server
✅ Reduce bundle size (no client-side JavaScript)

**What a Server Component CANNOT do:**
❌ Use React hooks (`useState`, `useEffect`, etc.)
❌ Use browser APIs (`localStorage`, `window`, `document`)
❌ Handle user interactions (`onClick`, `onChange`)
❌ Use React Context
❌ Use custom hooks

---

## 🟢 Client Components

**What they are:**

- Components that render **on both server AND client**
- Include JavaScript that runs in the browser
- Can use React hooks and browser APIs
- Interactive and dynamic

**When to use:**

- Components with interactivity (buttons, forms, inputs)
- Components using React hooks (`useState`, `useEffect`, etc.)
- Components using browser APIs (`localStorage`, `window`, etc.)
- Components handling user events (`onClick`, `onChange`, etc.)
- Components using React Context

**Example from your codebase:**

```tsx
// app/login/page.tsx - This SHOULD be a Client Component
"use client"; // ← Add this at the top!

import { useState, useEffect } from "react";
import { useAuth } from "@/context/AuthContext";
import { useRouter } from "next/navigation";

export default function LoginPage() {
  // ✅ These require 'use client':
  const [formData, setFormData] = useState({ email: "", password: "" });
  const { login, isAuthenticated } = useAuth(); // Custom hook
  const router = useRouter(); // Next.js client hook

  useEffect(() => {
    if (isAuthenticated) {
      router.push("/");
    }
  }, [isAuthenticated, router]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    await login(formData.email, formData.password);
  };

  return <form onSubmit={handleSubmit}>{/* Interactive form elements */}</form>;
}
```

**What a Client Component CAN do:**
✅ Use React hooks (`useState`, `useEffect`, `useContext`)
✅ Use browser APIs (`localStorage`, `window`, `document`)
✅ Handle user interactions (`onClick`, `onChange`, `onSubmit`)
✅ Use React Context
✅ Use custom hooks
✅ Be interactive and dynamic

**What a Client Component CANNOT do:**
❌ Fetch data directly from databases (must use API routes)
❌ Access file system
❌ Keep secrets (everything is sent to the browser)

---

## 📊 Comparison Table

| Feature                        | Server Component | Client Component       |
| ------------------------------ | ---------------- | ---------------------- |
| **Where it runs**              | Server only      | Server + Browser       |
| **JavaScript sent to browser** | None             | Yes                    |
| **React Hooks**                | ❌ No            | ✅ Yes                 |
| **Browser APIs**               | ❌ No            | ✅ Yes                 |
| **User Interactions**          | ❌ No            | ✅ Yes                 |
| **Fetch data from DB**         | ✅ Yes           | ❌ No (use API routes) |
| **Async/Await**                | ✅ Yes           | ⚠️ Only in functions   |
| **Bundle Size**                | Small            | Larger                 |
| **SEO**                        | ✅ Better        | ⚠️ Less optimal        |
| **Initial Load**               | ✅ Faster        | ⚠️ Slower              |
| **Interactivity**              | ❌ None          | ✅ Full                |

---

## 🎯 How to Decide

### Use Server Component (default) when:

- Displaying static content
- Fetching data from database/API
- Rendering markdown/blog posts
- SEO is important
- You want faster initial load

### Use Client Component (`'use client'`) when:

- Component needs interactivity (buttons, forms)
- Using React hooks (`useState`, `useEffect`)
- Using browser APIs (`localStorage`, `window`)
- Handling user events (`onClick`, `onChange`)
- Using React Context or custom hooks

---

## 🔄 Component Boundary

**Important Rule:** Once you add `'use client'` to a file, ALL components imported into that file become Client Components too, even if they don't have `'use client'` themselves.

```tsx
// Parent.tsx
"use client"; // ← Makes this AND all children Client Components

import ChildComponent from "./Child";
import GrandChildComponent from "./GrandChild";

export default function Parent() {
  return (
    <>
      <ChildComponent /> {/* Client Component */}
      <GrandChildComponent /> {/* Client Component */}
    </>
  );
}
```

**To use Server Components inside a Client Component:**

You need to pass Server Components as **children** or **props**:

```tsx
// Client Component
"use client";
import { ServerComponent } from "./ServerComponent";

export default function ClientWrapper({ children }) {
  return (
    <div>
      {children} {/* This can be a Server Component */}
    </div>
  );
}
```

---

## 📝 Your Codebase Examples

### ✅ Should be Client Components (need `'use client'`):

1. **`app/login/page.tsx`**

   - Uses `useState`, `useEffect`
   - Uses `useAuth()` hook
   - Uses `useRouter()` hook
   - Handles form submission

2. **`context/AuthContext.tsx`**

   - Uses `useState`, `useEffect`, `useContext`
   - Uses `localStorage` (browser API)
   - Uses `useRouter()` hook

3. **`app/layout.tsx`** (if it uses AuthProvider)

   - Imports `AuthProvider` which is a Client Component

4. **`app/page.tsx`** (home page - currently deleted)
   - Uses `useState`, `useEffect`
   - Uses `useAuth()`, `useRouter()` hooks
   - Handles user interactions

### ✅ Already Server Components (don't need `'use client'`):

1. **`app/api/health/route.ts`**
   - API route (always runs on server)
   - Uses server-side fetch

---

## 🚨 Common Mistakes

### ❌ Wrong: Using hooks without `'use client'`

```tsx
// This will ERROR
import { useState } from "react";

export default function MyComponent() {
  const [count, setCount] = useState(0); // ❌ Error!
  return <div>{count}</div>;
}
```

### ✅ Correct: Add `'use client'`

```tsx
"use client"; // ← Add this!

import { useState } from "react";

export default function MyComponent() {
  const [count, setCount] = useState(0); // ✅ Works!
  return <div>{count}</div>;
}
```

---

## 🎓 Best Practices

1. **Default to Server Components** - Only use `'use client'` when you need client features
2. **Keep Client Components small** - Extract logic to Server Components when possible
3. **Use Server Components for data fetching** - Faster and more secure
4. **Use Client Components for interactivity** - Only when you need it
5. **Pass Server Components as children** - To mix Server and Client Components

---

## 📚 Summary

- **Server Components** = Default, run on server, no JavaScript, fast, SEO-friendly
- **Client Components** = Add `'use client'`, run in browser, interactive, use hooks
- **Rule of thumb**: If you use hooks, browser APIs, or event handlers → you need `'use client'`
