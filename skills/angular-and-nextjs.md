# Angular & Next.js

Angular remains common in large enterprise front ends, while Next.js is a frequent default for React products that need server rendering, so front-end interviews often test both the framework mechanics and the judgement to choose between them. Interviewers want to see that you can explain why a screen re-renders, where code runs and what a cache is holding. Senior Front-End, Full-Stack, UI Platform and Tech Lead roles ask most, often framed as "which stack would you choose for this product, and how would you keep it fast?". React fundamentals are covered in [React & frontend basics](react-and-frontend-basics.md).

> Related: [React & frontend basics](react-and-frontend-basics.md) · [Node.js & TypeScript](nodejs-and-typescript.md) · [Authentication & authorisation](authentication-and-authorization.md) · [Redis & caching](redis-and-caching.md) · [System design](../topics/03-system-design/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What makes up an Angular component, and how do standalone components differ from NgModule-based ones? *(verify)*
- [ ] Walk through the main Angular lifecycle hooks: `ngOnChanges`, `ngOnInit`, `ngAfterViewInit` and `ngOnDestroy`. What belongs in each?
- [ ] How does Angular's hierarchical dependency injection work (`providedIn: 'root'`, component `providers`, and `inject()`)?
- [ ] Observables vs. Promises: how do they differ, and when would you use `Subject`, `BehaviorSubject` or `ReplaySubject`?
- [ ] What are Angular signals (`signal`, `computed`, `effect`), and what problem do they solve? *(verify)*
- [ ] Explain default change detection vs. `OnPush`, and what Zone.js does in that process. *(verify)*
- [ ] App Router vs. Pages Router in Next.js: what do `layout`, `page`, `loading` and `error` files do? *(verify)*
- [ ] Define CSR, SSR, SSG and ISR. What does the user see, and when is the HTML generated in each?
- [ ] Server Components vs. Client Components: what does `'use client'` actually mark, and what can each kind of component do? *(verify)*
- [ ] What is hydration, and what commonly causes a hydration mismatch?

## B. Working depth
- [ ] `switchMap`, `mergeMap`, `concatMap` and `exhaustMap`: which would you use for typeahead, parallel downloads, ordered saves and a submit button?
- [ ] How do RxJS subscription leaks happen in Angular, and how do the `async` pipe and `takeUntilDestroyed` prevent them? *(verify)*
- [ ] When would you use signals and when RxJS in the same feature, and how do `toSignal` and `toObservable` connect them? *(verify)*
- [ ] Reactive vs. template-driven forms: when would you use each, and how do typed forms and custom validators fit in?
- [ ] Angular routing and HTTP: lazy-loaded routes, functional guards, resolvers and interceptors. Which concerns belong where? *(verify)*
- [ ] Component communication: `input()` and `output()` vs. decorators, content projection, and `@for` with `track`. Why does `track` matter? *(verify)*
- [ ] How do you test Angular code with `TestBed`, component harnesses, `HttpTestingController` and marble tests?
- [ ] In the Next.js App Router, when do you fetch in a Server Component, use a Server Action or write a Route Handler? *(verify)*
- [ ] Describe Next.js's caching layers (request memoisation, data cache, full route cache and client router cache) and how `revalidatePath` and `revalidateTag` affect them. *(verify)*
- [ ] Dynamic routes and `generateStaticParams`: what makes a route render dynamically, for example `cookies()` or `headers()`? *(verify)*
- [ ] How do streaming, `Suspense` and `loading.tsx` change what the user sees, and where would you place the boundaries?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] What does zoneless Angular change, what breaks when you remove Zone.js, and how would you migrate a large application? *(verify)*
- [ ] Angular SSR with hydration and `@defer`: what do they improve, and what new problems do they add for browser-only code? *(verify)*
- [ ] State management at scale: plain services with signals vs. NgRx. What would make you adopt a library, and how do you enforce module boundaries in a large workspace? *(verify)*
- [ ] How do you diagnose and improve Core Web Vitals (LCP, INP, CLS) in each framework, using lazy loading, `@defer`, `next/dynamic` and client bundle boundaries? *(verify)*
- [ ] What security risks come with the server/client boundary: secrets in props, Server Actions as public endpoints, and authorisation checks in the wrong layer? *(verify)*
- [ ] Self-hosting Next.js vs. a managed platform: what changes for shared cache storage, image optimisation, multi-instance revalidation and cost? *(verify)*
- [ ] How can caching leak personalised data through a CDN or shared cache, and what controls would you use in a multi-tenant app?
- [ ] Next.js is a React framework, while Angular is a full framework and Vue is a progressive one. How do you compare them fairly, including Nuxt where relevant? *(verify)*
- [ ] How would you choose between Angular, React, Vue and Next.js based on team size, hiring, conventions, SEO, upgrade burden and how long the product must live?
- [ ] Compare the upgrade experience of `ng update` with major Next.js changes. How do you budget for keeping up? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] An Angular dashboard with 5,000 rows stutters on every keystroke. Profile it and fix it, considering `OnPush`, `track`, signals and virtual scrolling.
- [ ] A typeahead shows stale results out of order and keeps making requests after the user leaves the page. Explain the bugs and fix them with RxJS.
- [ ] An online catalogue has one million product pages, with prices that change hourly and a personalised basket. Choose a rendering and caching approach for each part in Next.js. *(verify)*
- [ ] After a release, some Next.js users briefly see another user's account name. How do you respond, find the cause and prevent it happening again?
- [ ] A Next.js page takes two seconds to first byte because of sequential data fetches. How do you measure and fix it with parallel fetching, caching and streaming?
- [ ] A team proposes rewriting a large Angular application in Next.js to improve SEO. How do you assess the proposal, and what alternatives would you offer?
- [ ] One organisation needs a marketing site, a customer portal and an internal admin tool. Would you choose Angular, React, Vue, Next.js or a mix? Write the decision record.

## E. Hands-on checks (things to be able to DO)
- [ ] Build a lazy-loaded Angular feature using standalone components, a reactive form, an HTTP interceptor and `OnPush`.
- [ ] Convert a `BehaviorSubject`-based service to signals, then compare the template code and the amount of change detection work.
- [ ] Write a typeahead with `debounceTime`, `distinctUntilChanged` and `switchMap`, and cover it with a marble test.
- [ ] Build a Next.js app with static, revalidated, dynamic and streamed routes, then explain the route output from `next build`. *(verify)*
- [ ] Implement a Server Action with input validation, an authorisation check and `revalidateTag`, then prove that the cache updates correctly. *(verify)*
- [ ] Move the non-interactive parts of a client-rendered page into Server Components, and measure bundle size and Core Web Vitals before and after.

---

**Good answers show:**
- A precise explanation of what triggers an update in Angular, and how signals and `OnPush` narrow the work.
- RxJS operators chosen for their concurrency behaviour, with subscriptions that are clearly cleaned up.
- A clear mental map of where Next.js code runs, what crosses the server/client boundary and why that matters for security.
- The ability to say exactly which cache served a response, and how and when it will be invalidated.
- Framework choices justified by team, product and lifespan, with fair credit given to the alternatives.
- Performance work driven by profilers and field data, with awareness that framework defaults change between releases.

**Prove it:** Build the same small product catalogue with search, a detail page and an authenticated basket twice, once in Angular with signals and SSR and once in the Next.js App Router. Measure bundle size, Core Web Vitals and build time, then write a one-page decision record stating which you would choose for a small product team and for a large enterprise team.
