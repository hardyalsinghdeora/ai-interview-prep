# Frontend Frameworks Cheatsheet

Covers key terms for React, Next.js, Angular and Vue, plus rendering modes, state management, browser performance and a framework comparison.

> Related: [React and frontend basics skill](../skills/react-and-frontend-basics.md) · [Node.js and TypeScript skill](../skills/nodejs-and-typescript.md) · [networking-and-web.md](networking-and-web.md) · [programming-languages.md](programming-languages.md) · [backend-frameworks.md](backend-frameworks.md) · [testing.md](testing.md) · [Angular & Next.js skill](../skills/angular-and-nextjs.md)

## Framework comparison

| Feature | React | Next.js | Angular | Vue |
|---|---|---|---|---|
| Type | User interface library | Full-stack React framework | Full framework | Progressive framework |
| Language | JavaScript or TypeScript with JSX | JavaScript or TypeScript with JSX | TypeScript with HTML templates | JavaScript or TypeScript with templates |
| Updates driven by | Re-render, then reconcile | Same as React | Signals and change detection | Tracked reactive dependencies |
| Routing | Separate library | Built in, file-based | Built in | Official Vue Router |
| Shared state | Context or libraries | Context or libraries, plus server data | Services, Signals, NgRx | Pinia |
| Server rendering | Through a framework | Built in | `@angular/ssr` | Nuxt |
| Stewardship *(verify)* | Began at Meta, now foundation-governed | Vercel | Google | Independent, community-funded |

## React

| Term | What it means |
|---|---|
| **Component** | A reusable function returning user interface markup, which is the basic building block of React applications. |
| **JSX** (JavaScript XML) | HTML-like syntax in JavaScript, compiled to function calls that describe the user interface. |
| **Props** | Read-only inputs passed from parent to child, which make data flow one-way and predictable. |
| **State** | Data owned by a component that triggers a re-render when it changes. |
| **Virtual DOM** (Virtual Document Object Model) | A lightweight description of the interface, compared across renders so only needed browser updates happen. |
| **Reconciliation** | React's comparison of old and new trees, which works out the minimum changes to apply. |
| **Key** | A stable identity for a list item; using array indexes can attach state to the wrong row. |
| **Fiber** | React's internal engine, which splits rendering into units that can be paused and prioritised. |
| **Hook** | A function such as `useState` that gives function components access to React features. |
| **Rules of Hooks** | Call hooks only at the top level of components or custom hooks, so their call order stays consistent. |
| **`useEffect`** | Synchronises a component with external systems after rendering; wrong dependencies cause stale values or loops. |
| **`useMemo` and `useCallback`** | Cache a computed value and a function between renders; useful only when the recalculation or re-render really matters. |
| **`useRef`** | Holds a mutable value without causing a re-render, often used to access a DOM element. |
| **Custom hook** | A function reusing stateful logic across components, without sharing the state itself. |
| **`React.memo`** | Skips re-rendering a component when its props have not changed, which helps with expensive subtrees. |
| **Suspense** | Shows fallback content while children wait for code or data, simplifying loading states. |
| **Concurrent rendering** | Lets React interrupt rendering to keep urgent updates responsive, used through features such as `useTransition`. |
| **Error boundary** | Catches rendering errors in a subtree and shows a fallback; still written as a class component *(verify)*. |
| **Controlled component** | A form input whose value is held in React state, making validation and resets simple. |
| **StrictMode** | A development-only check that runs some functions twice, which exposes impure rendering and missing cleanup. |
| **React Compiler** | A build tool that adds memoisation automatically, reducing manual `useMemo` use; check current support *(verify)*. |

## Next.js

| Term | What it means |
|---|---|
| **Next.js** | A React framework adding routing, server rendering and build tooling; defaults change across major versions *(verify)*. |
| **App Router** | The newer `app/` directory routing, built on React Server Components with nested layouts. |
| **Pages Router** | The original `pages/` directory routing, still found in many existing applications. |
| **`layout` file** | Shared interface wrapping child routes that persists across navigation, avoiding repeated rendering. |
| **`'use client'`** | Marks the boundary where code becomes client-side, which brings browser interactivity and adds JavaScript to the bundle. |
| **Server Action** | A server-side function called directly from components or forms, reducing hand-written endpoints for mutations *(verify)*. |
| **Route handler** | A `route.ts` file defining HTTP endpoints inside the App Router. |
| **Next.js middleware** | Code that runs before a request completes, for redirects or checks; renamed `proxy` in version 16 *(verify)*. |
| **`next/image`** | An image component that resizes, lazy loads and serves modern formats, improving load time and layout stability. |
| **`revalidate`** | Sets how long cached data stays fresh before regeneration, which is how Next.js applies incremental static regeneration. |

## Angular

| Term | What it means |
|---|---|
| **Angular** | Google's TypeScript framework including routing, forms, HTTP and tooling, which gives large teams consistent conventions. |
| **Standalone component** | A component that declares its own imports with no `NgModule`, the default in recent versions *(verify)*. |
| **`NgModule`** | The older way to group declarations and dependencies, still common in existing codebases. |
| **Angular service** | A class holding shared logic or data, usually provided as a singleton through dependency injection. |
| **Hierarchical injector** | Angular's tree of dependency injection providers, which decides which instance a component receives. |
| **Change detection** | How Angular finds changed data and updates the DOM; strategy choice strongly affects performance. |
| **`OnPush`** | A change detection strategy that checks a component only on specific triggers, reducing unnecessary work. |
| **Zone.js** | Patches asynchronous browser APIs to tell Angular when to check for changes; being replaced by zoneless mode *(verify)*. |
| **Signal** | A reactive value that tracks its readers, enabling precise updates without checking whole component trees. |
| **RxJS** (Reactive Extensions for JavaScript) | A library of observable streams and operators, used widely in Angular for asynchronous data. |
| **Pipe** (Angular) | Transforms values in templates, such as formatting dates; pure pipes rerun only when inputs change. |
| **Built-in control flow** | The `@if`, `@for` and `@switch` template syntax, replacing `*ngIf` and `*ngFor` *(verify)*. |

## Vue

| Term | What it means |
|---|---|
| **Vue** | A progressive framework with templates and fine-grained reactivity, which can be adopted gradually or used for full applications. |
| **SFC** (Single-File Component) | A `.vue` file combining template, script and style, which keeps related code together. |
| **Options API** | Organises components by options such as `data` and `methods`; simple, though logic can scatter as components grow. |
| **Composition API** | Organises logic with imported functions such as `ref`, which improves reuse and TypeScript support. |
| **`<script setup>`** | Compile-time shorthand for using the Composition API, with less boilerplate. |
| **`ref` and `reactive`** | `ref` wraps any value, read through `.value` in scripts; `reactive` makes an object deeply reactive. |
| **`computed`** | A cached value derived from reactive data, recalculated only when its dependencies change. |
| **`watch`** | Runs a side effect when reactive data changes, such as fetching data after a filter changes. |
| **`v-model`** | Two-way binding between a form input or component and state, which removes event handling boilerplate. |
| **Nuxt** | The Vue meta-framework adding file-based routing, server rendering and static generation, similar to Next.js. |

## Rendering modes

| Term | What it means |
|---|---|
| **CSR** (Client-Side Rendering) | The browser downloads JavaScript and builds the page; simple to host, but slower first load and weaker search visibility. |
| **SSR** (Server-Side Rendering) | The server builds HTML per request, giving fresh content and fast first paint at higher server cost. |
| **SSG** (Static Site Generation) | HTML is built ahead of time and served from a CDN, which is fast and cheap but needs rebuilds. |
| **ISR** (Incremental Static Regeneration) | Regenerates individual static pages after deployment, combining static speed with reasonably fresh content. |
| **Hydration** | Client JavaScript attaches event handlers to server-rendered HTML; until finished, pages may look ready but not respond. |
| **Hydration mismatch** | Server and client render different output, often from dates or random values, causing warnings or incorrect content. |
| **Streaming SSR** (Streaming Server-Side Rendering) | Sends HTML in chunks as it becomes ready, so slow data does not block the whole page. |
| **Islands architecture** | Mostly static HTML with isolated interactive parts that hydrate separately, which reduces JavaScript sent to browsers. |
| **RSC** (React Server Components) | Components that run only on the server and send no component code to the browser, shrinking bundles. |
| **Edge rendering** | Runs rendering at locations close to users, which reduces latency if data is also nearby. |

| Mode | HTML created | First paint | Freshness | Typical use |
|---|---|---|---|---|
| CSR | In the browser | Slower | Live after load | Dashboards behind login |
| SSR | Per request on server | Fast | Always fresh | Personalised or changing pages |
| SSG | At build time | Fastest | Until rebuild | Documentation, marketing |
| ISR | Build time, then in background | Fast | Fresh within a set window | Large catalogues, content sites |

## State management

| Term | What it means |
|---|---|
| **Local state** | Data used by one component and kept there; the right default before reaching for shared state. |
| **Lifting state up** | Moves state to the nearest common parent, so sibling components stay in sync. |
| **Prop drilling** | Passing props through layers that do not use them, which makes refactoring tedious. |
| **Context** | React's way to pass values down without props; all consumers re-render when the value changes. |
| **Server state** | Remote data the client only borrows; it needs caching and refetching, unlike ordinary client state. |
| **TanStack Query** | A library for fetching, caching and refreshing server state, which removes a lot of manual loading code. |
| **Redux** | A predictable global store where actions pass through pure reducers to produce new state. |
| **Redux Toolkit** | The officially recommended way to write Redux, with far less boilerplate. |
| **Selector** | A function that reads derived data from a store, limiting re-renders to relevant changes. |
| **Zustand** | A small hook-based store for React with little boilerplate, a common lighter alternative to Redux. |
| **NgRx** | A Redux-style state library for Angular, useful for large applications but adds ceremony. |
| **Pinia** | The official Vue store, with simple actions and strong TypeScript support. |

| Kind of state | Example | Usual home |
|---|---|---|
| Local | Open dropdown | Component state |
| Shared client | Theme, basket | Context or a store |
| Server | Product list | TanStack Query or framework data loading |
| URL | Filters, page number | Router query parameters |

## Browser performance

| Term | What it means |
|---|---|
| **Core Web Vitals** | Google's user experience metrics for loading, responsiveness and visual stability, measured from real users. |
| **LCP** (Largest Contentful Paint) | Time until the largest visible content appears; slow servers and large images are common causes. |
| **INP** (Interaction to Next Paint) | Responsiveness across a visit's interactions; long JavaScript tasks are the common cause of poor scores. |
| **CLS** (Cumulative Layout Shift) | How much visible content unexpectedly moves; reserve space for images and adverts to reduce it. |
| **TTFB** (Time to First Byte) | Time until the first response byte arrives, which reflects server and network delay. |
| **FCP** (First Contentful Paint) | Time until the first text or image appears, an early signal that the page is loading. |
| **Lighthouse** | A browser audit tool giving lab scores and recommendations; lab results can differ from real-user data. |
| **Critical rendering path** | The browser's steps from HTML, CSS and JavaScript to pixels; blocking resources delay first display. |
| **Layout (reflow)** | Calculating element sizes and positions; expensive because one change can affect much of the page. |
| **Paint (repaint)** | Drawing pixels for elements, cheaper than layout when only appearance such as colour changes. |
| **Compositing** | Combining layers, which is why animating `transform` and `opacity` is usually smoothest. |
| **Long task** | Main-thread work lasting over 50 milliseconds, which blocks input handling and hurts responsiveness. |
| **Code splitting** | Divides JavaScript into chunks loaded on demand, which reduces the initial download. |
| **Lazy loading** | Delays images, components or routes until needed, which saves bandwidth and start-up time. |
| **Tree shaking** | Removes unused exports at build time; relies on ES modules and code without side effects. |
| **`defer` and `async`** | Script attributes that avoid blocking HTML parsing; `defer` preserves order, while `async` runs when ready. |
| **Debounce** | Runs a function after activity stops, which suits search boxes. |
| **Throttle** | Runs a function at most once per interval, which suits scroll and resize handlers. |
| **List virtualisation** | Renders only visible rows of long lists, which keeps DOM size and memory low. |
| **Web Worker** | Runs JavaScript on a background thread, which keeps heavy work away from the main thread. |
| **Service Worker** | A script that intercepts network requests, enabling offline support and custom caching. |

| Core Web Vital | Measures | Good target *(verify)* |
|---|---|---|
| LCP | Loading | 2.5 seconds or less |
| INP | Responsiveness | 200 milliseconds or less |
| CLS | Visual stability | 0.1 or less |
