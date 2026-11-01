# OOP, design patterns and low-level design

Low-level design rounds test whether you can turn a loose problem into clean, working classes: clear responsibilities, sensible boundaries and code that copes with the next requirement. Interviewers rarely want pattern definitions on their own, so expect to explain why a pattern fits, what it costs and when simpler code would be better. SDE and senior backend loops ask most, often through a timed exercise such as a parking lot or an LRU cache, while tech leads are also asked how design standards hold up across a growing codebase.

> Related: [Java and Spring Boot](java-and-spring-boot.md) · [Python](python.md) · [System design](system-design.md) · [Testing and quality](testing-and-quality.md) · [Coding](../topics/02-coding/README.md) · [CS fundamentals](../topics/13-cs-fundamentals/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Explain encapsulation, abstraction, inheritance and polymorphism using one small example. Which is most often misunderstood?
- [ ] Abstract class vs. interface: how do you choose, and how have default methods changed that choice in Java? *(verify)*
- [ ] Method overloading vs. overriding: which is resolved at compile time, which at run time, and what bugs can each cause?
- [ ] Walk through the SOLID principles, giving a short example of a violation for each one.
- [ ] Why is the square-extends-rectangle example a Liskov Substitution violation, and how would you model it instead?
- [ ] Why do people say to favour composition over inheritance? Give a case where inheritance is still the better choice.
- [ ] What do cohesion and coupling mean, and how would you spot poor examples of each in a pull request?
- [ ] Dependency inversion vs. dependency injection vs. an inversion of control container: how are they related?
- [ ] Name the three Gang of Four categories (creational, structural, behavioural) and give two patterns you would actually use from each.
- [ ] Why does immutability make a design safer, and what does it take to make a class properly immutable?

## B. Working depth
- [ ] Factory Method vs. Abstract Factory vs. Builder: what problem does each solve, and when is a plain constructor enough?
- [ ] Why is Singleton often considered an anti-pattern? How would you get a single shared instance without its downsides?
- [ ] Strategy vs. State: the class structures look similar, so how do you tell which one you need?
- [ ] Decorator vs. Proxy vs. Adapter: how do their intents differ, and where have you seen each in a common framework?
- [ ] Observer: what can go wrong with ordering, memory leaks, exceptions and slow subscribers?
- [ ] Template Method vs. Strategy: what is the inheritance vs. composition trade-off here?
- [ ] When would you use Command or Chain of Responsibility? Give a real example, such as undo support or request middleware.
- [ ] Constructor vs. setter vs. field injection: which do you prefer, and how does the choice affect testing and object validity?
- [ ] When is manual wiring enough, and when does a DI container such as Spring earn its complexity?
- [ ] Explain the dependency rule in clean architecture. What belongs in entities, use cases, adapters and frameworks?
- [ ] How do clean, hexagonal and layered architecture differ in practice? Where would a database transaction live in each?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] When does applying SOLID or patterns too eagerly make code worse? How do you decide when an abstraction has earned its place?
- [ ] What is the fragile base class problem, and how does it affect a public API or shared library?
- [ ] Which classic patterns become unnecessary with first-class functions, and how would you implement Strategy or Command without classes?
- [ ] How do sealed types and pattern matching change the case for the Visitor pattern? *(verify)*
- [ ] How would you design without inheritance in Go, or with duck typing and `typing.Protocol` in Python? What do you gain and lose?
- [ ] How does concurrency affect your design? Cover shared mutable state, safe publication, double-checked locking and immutable objects.
- [ ] What problems do DI containers introduce, such as circular dependencies, failures at startup and hard-to-trace wiring? How do you keep them under control?
- [ ] When is clean architecture not worth its extra layers? What would you do for a small CRUD service?
- [ ] (Lead) How do you keep module boundaries and design standards intact in a growing codebase, using reviews and automated architecture checks?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] Design a parking lot with several vehicle types, floors, tickets and pricing rules. How do you add a new pricing rule or handle two cars claiming one spot?
- [ ] Design a rate limiter class supporting token bucket and sliding window algorithms. How do you make it thread-safe, and what changes when it runs across many servers?
- [ ] Design an LRU cache with O(1) `get` and `put`. How would you add expiry times and safe concurrent access, and when would you use LFU instead?
- [ ] Design an elevator system with several cars. Model the states and requests, and keep the scheduling algorithm replaceable.
- [ ] Design a notification system supporting email, SMS and push, with user preferences, templates, retries and duplicate prevention.
- [ ] You inherit a 2,000-line class with a long `switch` on order type. How do you refactor it safely, one step at a time?
- [ ] Design a vending machine or a movie ticket booking system, explaining how you prevent invalid transitions or double booking.

## E. Hands-on checks (things to be able to DO)
- [ ] Code an LRU cache in 25 minutes without a built-in linked map, with tests for eviction order and edge cases.
- [ ] Implement a token bucket rate limiter with an injected clock, so the tests run without any sleeps.
- [ ] Draw a class diagram for the parking lot exercise in five minutes, then implement its core flow and explain each responsibility.
- [ ] Refactor a conditional-heavy class to use Strategy or State, keeping the tests green after every step.
- [ ] Build a small service with hexagonal structure and wire it by hand, then swap a PostgreSQL adapter for an in-memory one.
- [ ] Review a short piece of code, find its SOLID violations and say which ones are actually worth fixing.

---

**Good answers show:**
- Requirements clarified first, including scope, scale, concurrency and likely changes, before any class is named.
- Responsibilities and relationships driven by the problem, with patterns named afterwards rather than chosen in advance.
- Trade-offs stated plainly, including when the simplest option is the right one.
- Working code with sensible names, edge cases handled and a clear testing approach.
- Extension shown concretely: how the next requirement would change the design, and how few classes it would touch.
- Awareness of concurrency, failure handling and where the in-process design stops working.

**Prove it:** Build a small notification library with email and SMS channels behind one interface, user preferences, templates, retries with backoff, and duplicate prevention, with dependencies injected and a fake clock and fake providers in the tests. Then add a push channel and write a short note on which files changed and what you would design differently.
