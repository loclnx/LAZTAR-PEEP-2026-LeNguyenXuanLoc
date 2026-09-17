+++
title = "Day 02 - 16/09/2026 (ON-SITE) React and Next.js"
weight = 2
+++

### Day 02 Report

## A. Theory

## Part 1. React Fundamentals

### 1. What is React?

React is an open-source JavaScript library developed by Meta for building user interfaces, especially highly interactive web applications. React organizes the interface into reusable components and updates the UI when data changes.

### 2. What is a React component? How many types of components are there?

A component is an independent part of the interface that can receive input data and return the UI that should be displayed. Components divide an application into smaller parts that are easier to develop and maintain.

React commonly has two types of components:

- **Function Component:** Written as a JavaScript or TypeScript function. This is the most common approach today and supports Hooks.
- **Class Component:** Written as a class and uses lifecycle methods. This approach is mainly found in older React projects.

### 3. What is JSX?

JSX is a syntax extension for JavaScript that allows developers to write HTML-like structures inside JavaScript code. JSX is compiled into calls to `React.createElement` or an equivalent mechanism before running in the browser.

```jsx
const greeting = <h1>Hello React</h1>;
```

### 4. What are Props?

Props are data passed from a parent component to a child component. Props are read-only because a child component should not directly change the data received from its parent.

```jsx
function Welcome({ name }) {
  return <h1>Hello {name}</h1>;
}
```

### 5. What is State? How is State different from Props?

State is data managed internally by a component. When state changes, React asks the component to render again and update the interface.

| Characteristic              | Props                          | State                                 |
| --------------------------- | ------------------------------ | ------------------------------------- |
| Data source                 | From the parent component      | Managed by the component itself       |
| Can it be changed directly? | No                             | No, an update function should be used |
| Scope                       | Passed between components      | Usually belongs to one component      |
| Purpose                     | Configuration or data transfer | Stores changing UI data               |

### 6. What is the Virtual DOM? Why does React use it?

The Virtual DOM is an in-memory representation of the real DOM. When data changes, React creates a new Virtual DOM, compares it with the previous version, and updates only the necessary parts of the real DOM.

This reduces the number of direct DOM operations and makes UI updates more efficient and easier to control.

### 7. What are Hooks? Name some common React Hooks.

Hooks are special functions that allow Function Components to use state, lifecycle behavior, and other React features. Common Hooks include:

- `useState`: Manages state.
- `useEffect`: Performs side effects.
- `useContext`: Reads data from Context.
- `useReducer`: Manages state with complex logic.
- `useRef`: Stores a value between renders or references a DOM element.
- `useMemo`: Memoizes a computed result.
- `useCallback`: Memoizes a callback function.

### 8. What is `useState` used for?

`useState` declares and updates state in a Function Component.

```jsx
const [count, setCount] = useState(0);

function increase() {
  setCount(count + 1);
}
```

When `setCount` is called, React renders the component again with the new value.

### 9. What is `useEffect` used for?

`useEffect` handles side effects, which are tasks outside the render process such as calling an API, registering an event, updating the page title, or using a timer.

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

The dependency array determines when the effect runs again. An effect can also return a cleanup function to remove a timer or event listener.

### 10. What are the lifecycle stages of a React component?

A component lifecycle has three main stages:

1. **Mounting:** The component is created and added to the DOM.
2. **Updating:** The component is updated when its props or state change.
3. **Unmounting:** The component is removed from the DOM.

For Function Components, `useEffect` is commonly used to handle work after rendering and cleanup when the component is unmounted.

### 11. What is Client-Side Rendering (CSR)?

CSR is a rendering approach in which the server sends basic HTML and JavaScript to the browser. The browser loads the JavaScript, runs the application, and creates most of the interface on the client side.

CSR is suitable for highly interactive applications after they have loaded, but the initial load may be slower and SEO may require additional solutions.

### 12. What is React Router?

React Router is a routing library commonly used in React applications. It maps URLs to components and supports nested routes, dynamic routes, programmatic navigation, and protected routes.

For example, the URL `/products/10` can be mapped to a component that displays the product with ID `10`.

### 13. Does plain React provide Routing, SEO, and an API Server?

Plain React does not include Routing, server-side SEO, or an API Server by default:

- **Routing:** A library such as React Router can be used.
- **SEO:** Metadata libraries or another SSR solution can be added, but plain React mainly uses CSR.
- **API Server:** The application must call a separate backend API or another backend service.

Therefore, plain React is a UI library, not a full-stack framework.

### 14. What is the Context API? When should it be used?

The Context API shares data between multiple components without passing props through every intermediate component.

Context is useful for data used in many places, such as themes, languages, logged-in user information, or access permissions. It should not be used for every piece of state because changing a Context can cause many consuming components to render again.

### 15. What is an SPA (Single Page Application)?

An SPA initially loads one HTML page. JavaScript then changes the page content and navigates between screens without reloading the entire page.

An SPA provides fast and smooth navigation, but developers must consider the initial JavaScript load time, SEO, and browser state management.

## Part 2. Comparing React and Next.js

### 16. What is Next.js?

Next.js is a framework for building web applications with React, developed by Vercel. It provides routing, multiple rendering strategies, image optimization, metadata, API/Route Handlers, and deployment support.

### 17. What is the core difference between React and Next.js?

React is a library focused on building user interfaces. Next.js is a framework built on React that also provides architecture, folder conventions, routing, server rendering, performance optimization, and full-stack features.

In other words, React provides the component foundation, while Next.js provides a complete structure for building web applications.

### 18. How is routing different in React and Next.js?

In React, developers usually install and configure a library such as React Router and then define routes manually.

In Next.js, routing is based on the file and folder structure. Depending on the router, `app/page.tsx` or `pages/index.tsx` represents the `/` route. In many cases, routes do not need to be declared manually.

### 19. How does rendering differ between React and Next.js?

Plain React commonly uses CSR, where the interface is mainly created in the browser.

Next.js supports multiple rendering methods:

- CSR.
- SSR: Generates HTML on the server for each request.
- SSG: Generates static HTML during the build.
- ISR: Updates static pages periodically or when requested.

### 20. Why does Next.js provide better SEO support than plain React?

With plain React and CSR, the initial HTML often does not contain all page content, so a crawler must execute JavaScript to read the page. Next.js can generate HTML containing the content on the server or during the build.

Next.js also supports managing titles, descriptions, Open Graph images, and other metadata. Crawlers can therefore read content and metadata earlier, improving indexing and link sharing.

### 21. How does first-page-load performance differ between React and Next.js?

A React CSR application usually needs to load JavaScript before creating the interface, so the first load may take longer.

Next.js can send server-rendered HTML or a static HTML file before all JavaScript has finished loading. This can make content appear earlier. Actual performance still depends on bundle size, data, server performance, and application optimization.

### 22. How do React and Next.js project structures differ?

A React project usually allows developers to choose the folder structure and install routing, data-fetching, or build-configuration libraries themselves.

Next.js has clearer conventions, for example:

- `app/` for the App Router.
- `pages/` for the Pages Router.
- `public/` for static assets.
- `next.config.js` or `next.config.ts` for Next.js configuration.
- `layout.tsx`, `page.tsx`, and special files for specific features.

### 23. Does Next.js replace React? Why?

No. Next.js is built on React and uses React components, JSX, props, state, and Hooks. Next.js adds features and conventions for larger applications; it does not replace React knowledge.

### 24. When should plain React or Next.js be used?

Use plain React when:

- Building an internal SPA or dashboard.
- The application does not require strong SEO.
- You want freedom to choose libraries and project structure.
- The backend and frontend are deployed separately.

Use Next.js when:

- SEO and first-content display time are important.
- Building a public website, blog, e-commerce site, or landing page.
- You want file-based routing, SSR, SSG, ISR, and image optimization.
- You want to combine the UI with some server features in one project.

## Part 3. Next.js

### 25. What are the App Router and Pages Router in Next.js?

**Pages Router** is the traditional routing system that uses the `pages/` directory. It uses functions such as `getStaticProps` and `getServerSideProps` to fetch data.

**App Router** is the newer routing system that uses the `app/` directory. It supports React Server Components, nested layouts, loading UI, error UI, and Server Actions. For new projects, App Router is usually the recommended choice.

### 26. What is the difference between Server Components and Client Components?

**Server Components** are rendered on the server and do not send all component logic to the browser. They are suitable for fetching data and reducing client-side JavaScript.

**Client Components** run in the browser and require the `"use client"` directive. They are used when state, event handlers, Hooks such as `useState`, or browser APIs such as `window` and `localStorage` are needed.

Server Components reduce the client bundle, while Client Components are suitable for interactive interfaces.

### 27. What is SSR (Server-Side Rendering)?

SSR is a technique in which the server creates HTML for each request using current data and sends that HTML to the browser. SSR is suitable for content that changes frequently or depends on the request, cookies, or user information.

### 28. What is SSG (Static Site Generation)?

SSG generates static HTML files during the build. When a user visits the page, the server can return the static file very quickly.

SSG is suitable for content that changes infrequently, such as blogs, documentation, introductory pages, and landing pages.

### 29. What is ISR (Incremental Static Regeneration)?

ISR updates a static page after the application has been built without rebuilding the entire website. A page can be regenerated after a configured period or revalidated when an appropriate event occurs.

ISR combines the speed of SSG with the content-update capability of SSR.

### 30. How does file-based routing work in Next.js?

File-based routing maps the file and folder structure to URLs:

- `app/page.tsx` creates the `/` route.
- `app/about/page.tsx` creates the `/about` route.
- `app/blog/[slug]/page.tsx` creates the dynamic `/blog/:slug` route.
- `app/blog/layout.tsx` creates a shared layout for routes inside `blog`.

Routes are created by convention instead of being registered manually.

### 31. What is a Dynamic Route in Next.js?

A Dynamic Route is a route with one or more URL parts that can change. The folder or file name is placed inside square brackets.

For example, `app/products/[id]/page.tsx` can handle `/products/1` and `/products/2`. The `id` value is read from `params` to load the corresponding data.

### 32. What is `layout.tsx` used for in the App Router?

`layout.tsx` defines a shared interface for a route and its child routes, such as a navbar, sidebar, or footer. The layout remains when navigating between child pages, which avoids re-rendering the entire shared interface.

The root layout in `app/layout.tsx` usually contains the `html` and `body` elements, fonts, and providers used throughout the application.

### 33. What are API Routes (Route Handlers) in Next.js?

Route Handlers create HTTP endpoints inside the `app` directory, usually with a `route.ts` or `route.js` file.

```ts
export async function GET() {
  return Response.json({ message: "Hello" });
}
```

A Route Handler can process methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`. It can also connect to a database or handle server-side logic.

### 34. What are `getStaticProps` and `getServerSideProps`? When are they used?

These are functions from the **Pages Router**:

- `getStaticProps` fetches data during the build and is suitable for SSG. It can be combined with `revalidate` to use ISR.
- `getServerSideProps` fetches data on the server for every request and is suitable for SSR and frequently changing data.

These functions are not used in the `app/` directory of the App Router. The App Router fetches data in Server Components and uses newer caching and revalidation mechanisms.

### 35. How does `next/image` optimize images?

`next/image` is the Next.js image-optimization component. It supports:

- Resizing images to the required dimensions.
- Automatically using modern formats such as WebP or AVIF when appropriate.
- Lazy loading images outside the viewport.
- Reducing layout shift through `width`, `height`, or `fill`.
- Optimizing local images and remote images from configured domains.

When using images from an external source, the domain or `remotePatterns` must be configured in `next.config.js` or `next.config.ts`.

### 36. What is Middleware in Next.js?

Middleware is code that runs before a request is completed. It can check paths, authenticate users, redirect, rewrite URLs, and read or add cookies and headers.

Middleware is usually placed in a `middleware.ts` file at the project root. A `matcher` should be configured when the middleware only applies to specific routes.

### 37. How can pages be navigated in Next.js?

There are several ways to navigate:

- Use the `Link` component for internal links:

```tsx
<Link href="/about">About</Link>
```

- Use `useRouter` in a Client Component for programmatic navigation:

```tsx
const router = useRouter();
router.push('/about');
```

- Use `redirect` in a Server Component or server-side logic when a redirect must happen on the server.

### 38. How are Metadata and SEO handled in Next.js?

Next.js supports static metadata through `metadata` and dynamic metadata through `generateMetadata` in the App Router.

Metadata can include a `title`, `description`, `keywords`, Open Graph images, Twitter Cards, and a canonical URL. Page content should also use proper headings, meaningful `alt` text, clear links, and semantic HTML to improve SEO.

### 39. Does Next.js support TypeScript?

Yes. Next.js supports TypeScript directly. When a file is created or changed to `.ts` or `.tsx`, Next.js can create `tsconfig.json` and install the required types. TypeScript checks the types of props, state, API responses, and functions during development.

### 40. Which platforms can host a Next.js project?

Next.js can be deployed to many platforms, including:

- **Vercel:** Optimized for Next.js, with preview deployments and Git-based CI/CD.
- **Netlify:** Suitable for many frontend applications and supports Next.js.
- **AWS:** Can be deployed through Amplify, a dedicated server, containers, or other AWS services.
- **Docker:** Packages the application to run on any container-compatible server.
- **Railway, Render, DigitalOcean**, and other Node.js platforms.

Before deployment, check environment variables, the build command, remote-image configuration, APIs, and the rendering modes used by the application.

## Challenges Encountered

- **Wireframe design:** The challenge was arranging the wireframe layout to meet the requirements, clearly show the user flow, and remain easy to read. The content priorities, main feature positions, and layout consistency had to be decided before coding.
- **Choosing React or Next.js for SEO:** Next.js is more suitable for an SEO-focused website because it supports SSR and SSG, which render content as HTML earlier for crawlers. Next.js also provides built-in ways to manage metadata such as titles, descriptions, and Open Graph data. React can still support SEO, but it usually needs additional libraries or rendering solutions and more manual configuration.

## Landing Page

[View personal portfolio](https://loclnxportfolio.vercel.app/)

Link repo: [github.com/loclnx/Portfolio](https://github.com/loclnx/Portfolio)
