# React & Frontend Basics

Backend and full-stack engineers are expected to understand what their APIs do to a user interface, and to make a small front-end change without breaking things. Interviewers ask about React and the browser to check that you can reason about rendering, data fetching and browser security, and that you design APIs with the client in mind. Full-stack, solutions and Forward Deployed Engineer roles ask most, because FDEs often build demos and internal tools end to end.

> Related: [API design & microservices](api-design-and-microservices.md) · [Node.js & TypeScript](nodejs-and-typescript.md) · [Security & compliance](security-and-compliance.md) · [Coding](../topics/02-coding/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What triggers a React component to re-render, and what is the difference between rendering and committing to the DOM?
- [ ] What is reconciliation, and why do list keys matter? What goes wrong when you use the array index as a key?
- [ ] Props vs. state: who owns each, and what does "lifting state up" solve?
- [ ] What do `useState` and `useEffect` do, and when does an effect run and clean up?
- [ ] What are the rules of hooks, and why do they exist?
- [ ] CSR vs. SSR vs. SSG: what does the browser receive first in each, and what is hydration?
- [ ] What is the same-origin policy, and what does CORS relax? Whose security does it protect?
- [ ] What is XSS, and how do stored, reflected and DOM-based variants differ?
- [ ] What is the difference between server state and client state, and why treat them differently?

## B. Working depth
- [ ] `useMemo`, `useCallback` and `React.memo`: what does each actually save, and when do they make no difference?
- [ ] What is a stale closure in `useEffect`, and how do dependency arrays cause or fix it?
- [ ] Why might you not need an effect? Give examples of effects that should be derived values or event handlers.
- [ ] What does TanStack Query give you over `fetch` inside `useEffect`: caching, deduplication, retries, background refetch?
- [ ] In TanStack Query, how do `staleTime`, `gcTime` and query keys work, and how do you invalidate after a mutation? *(verify)*
- [ ] How do you implement an optimistic update with rollback on failure, and when should you avoid one?
- [ ] Context vs. a state library (Redux, Zustand): what are the re-render implications of each?
- [ ] How does Vue's reactivity differ from React's rendering model, and how do the Composition API and hooks compare? *(verify)*
- [ ] What triggers a CORS preflight, what headers must the server return, and why can't credentials be used with a wildcard origin?
- [ ] What does a Content Security Policy do, and how do nonces help you avoid `unsafe-inline`?
- [ ] Storing tokens: `localStorage` vs. HttpOnly cookies, and how `SameSite` and CSRF protection fit in.
- [ ] What are the Core Web Vitals (LCP, INP, CLS), and what typically hurts each one? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] How would you choose between CSR, SSR, SSG and streaming for a logged-in dashboard vs. a public marketing page?
- [ ] What are React Server Components, what problem do they address, and what do they change for backend teams? *(verify)*
- [ ] What are the backend costs of SSR: server load, caching per user, time to first byte and cold starts?
- [ ] REST vs. GraphQL vs. a backend-for-frontend: how do you prevent over-fetching without creating a chatty or tightly coupled API?
- [ ] How should an API support a list view: cursor vs. offset pagination, filtering, sorting and partial responses?
- [ ] How would you design error responses so a front end can show field errors, retry safely and distinguish auth failures?
- [ ] Real-time updates: polling vs. WebSockets vs. Server-Sent Events. What are the trade-offs through load balancers and on mobile networks?
- [ ] How do bundle size and code splitting affect performance, and how do you enforce a budget in CI?
- [ ] CDN caching for HTML, APIs and assets: how do `Cache-Control`, `ETag` and hashed filenames work together?
- [ ] When would you choose React over Vue for a team, and when is a server-rendered page without a SPA the better choice?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] A page is slow only for some users, and LCP is poor in field data but fine in Lighthouse. How do you investigate?
- [ ] A list of 10,000 rows lags on every keystroke in a filter box. Find the cause and fix it.
- [ ] The browser console shows a CORS error, but the API works in Postman. Walk through the diagnosis and the correct fix.
- [ ] A security review finds user-supplied HTML rendered with `dangerouslySetInnerHTML`. What is the risk, and what do you change?
- [ ] Design the API and the front-end data layer for a paginated orders table with filters, inline editing and live status updates.
- [ ] Two quick searches return out of order, and the UI shows the wrong results. Why does it happen, and how do you prevent it?

## E. Hands-on checks (things to be able to DO)
- [ ] Build a small form and list in React with TypeScript, including loading, empty and error states.
- [ ] Use TanStack Query for a paginated list and a mutation with optimistic update and rollback.
- [ ] Use the React DevTools Profiler to find an unnecessary re-render, fix it and show the difference.
- [ ] Configure CORS and a strict Content Security Policy on a backend, and verify both in browser DevTools.
- [ ] Run Lighthouse on a page, improve one Core Web Vital and record the before and after.

---

**Good answers show:**
- A correct mental model: rendering is a function of state, and most performance fixes start from that.
- Separating server state from client state, and using a data-fetching library rather than hand-rolled effects.
- Browser security understood precisely, including what CORS does not protect and how CSP limits XSS damage.
- API design shaped by real screens, including pagination, error formats and caching.
- Measurement with real user data and profilers before adding memoisation or changing architecture.
- Honest scope: knowing the basics well as a backend engineer and knowing when to bring in a front-end specialist.

**Prove it:** Build a small full-stack app with a TypeScript API and a React front end: a paginated, filterable table with an inline edit using TanStack Query, optimistic updates and proper error states. Render the public landing page with SSR and the dashboard with CSR, add strict CORS and CSP headers, and record Core Web Vitals before and after one optimisation. Write up which API changes the front end pushed you to make.
