# Testing and quality

Testing questions reveal how you think about risk: what could break, which test would catch it soonest and what that test costs to run and maintain. Interviewers want more than a recital of the test pyramid, so expect questions on flaky suites, slow pipelines, mocks that hide real bugs and how to release safely with limited test time. Backend, full-stack and platform engineering loops ask most, while tech leads and engineering managers are asked to set a test strategy and defend its cost.

> Related: [Docker and CI/CD](docker-and-ci-cd.md) · [Git and code review](git-and-code-review.md) · [Java and Spring Boot](java-and-spring-boot.md) · [Python](python.md) · [Node.js and TypeScript](nodejs-and-typescript.md) · [Observability and reliability](observability-and-reliability.md) · [Debugging & troubleshooting](../topics/12-debugging-troubleshooting/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] What does the test pyramid recommend and why? When does a different shape, such as the testing trophy, make more sense?
- [ ] Unit vs. integration vs. end-to-end tests: what does each prove, what does each cost, and where do you draw the line for "unit"?
- [ ] Dummy, stub, spy, mock and fake: what is the difference, and when would you use each?
- [ ] What makes a unit test good? Why should it check behaviour rather than implementation details?
- [ ] Line coverage vs. branch coverage: what does 80% coverage tell you, and what does it not tell you?
- [ ] What is a flaky test, what commonly causes one, and why can it be worse than having no test?
- [ ] JUnit: explain the test lifecycle annotations, `@ParameterizedTest`, `@Nested` and `@ExtendWith`. What is each one for? *(verify)*
- [ ] pytest: how do fixtures and their scopes work, what belongs in `conftest.py`, and when would you use `@pytest.mark.parametrize`?
- [ ] Jest: `jest.fn` vs. `jest.spyOn` vs. `jest.mock`. How do module mocks get hoisted, and how do you stop mocks leaking between tests? *(verify)*
- [ ] Load vs. stress vs. spike vs. soak testing: what question does each one answer?

## B. Working depth
- [ ] In Spring Boot, when would you use `@SpringBootTest` rather than a slice test such as `@WebMvcTest` or `@DataJpaTest`? What does each cost? *(verify)*
- [ ] Why would you test against a real database with Testcontainers instead of an in-memory one such as H2? What are the costs in speed and CI setup? *(verify)*
- [ ] How do you isolate database state between integration tests, and what changes when tests run in parallel?
- [ ] What goes wrong when a test suite relies heavily on mocks? What does "don't mock what you don't own" mean in practice?
- [ ] How do you test code that depends on time, randomness or eventual consistency without using fixed sleeps?
- [ ] What gap does contract testing fill? Walk through a consumer-driven Pact workflow: consumer test, pact file, broker, provider verification, provider states and `can-i-deploy`.
- [ ] What does Pact not protect you from? How do you handle asynchronous messages and consumers you do not control? *(verify)*
- [ ] Playwright vs. Cypress vs. Selenium: how do their architectures differ, and how would you choose one for a new project? *(verify)*
- [ ] How do you write end-to-end tests that survive UI changes? Cover selectors, auto-waiting, page objects and creating test data through APIs.
- [ ] k6 vs. JMeter: compare scripting, resource use, protocol support and CI integration. When would you pick each? *(verify)*
- [ ] Open vs. closed load models: how does the choice affect your results, and what is coordinated omission?
- [ ] SonarQube: what are quality gates, and how do bugs, vulnerabilities, security hotspots and code smells differ? Why focus the gate on new code? *(verify)*

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] What goes wrong when a team enforces a hard coverage target, and what would you gate on instead?
- [ ] What does mutation testing with tools such as PIT or Stryker tell you that coverage does not? Is it worth its run time? *(verify)*
- [ ] Across 30 microservices, how would you balance contract tests, integration tests and shared end-to-end environments, and who owns what?
- [ ] Testing in production: compare canary releases, feature flags, synthetic monitoring and shadow traffic. How do you stop tests touching real money or real users?
- [ ] Design a flaky test programme: detection, quarantine, ownership and retry policy. When do retries hide a real race condition?
- [ ] A CI pipeline takes 45 minutes. How do you bring it under 10 using sharding, parallelism, test selection and caching, and what risk does each add?
- [ ] How do you make a load test credible when staging differs from production in data volume, topology and third-party dependencies?
- [ ] (Lead) Developer-owned quality vs. a dedicated QA team: how would you choose, and what does each model need in order to work?
- [ ] (EM) How do you justify the cost of test infrastructure to leadership, and which measures show that it is paying off?

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] You inherit a legacy service with no tests and must change its pricing logic next week. What do you test first, and how?
- [ ] An end-to-end suite of 800 Selenium tests fails 20% of the time, and engineers ignore it. What do you do this month, and what do you do this year?
- [ ] A breaking API change reached production even though every team's tests passed. Design a contract testing rollout across six consuming teams.
- [ ] Traffic is expected to reach 10 times normal for a seasonal sale. Plan the load tests, the targets, the environment and the pass or fail criteria.
- [ ] Design the test strategy for a payment flow that uses a third-party provider, webhooks and retries.
- [ ] A bug appears only in production, and every test passed. How do you investigate it, and what do you change in your test approach afterwards?

## E. Hands-on checks (things to be able to DO)
- [ ] Write a parameterised test with a mocked dependency in JUnit, then the same kind of test in pytest and in Jest.
- [ ] Write an integration test using Testcontainers that runs your real schema migrations against PostgreSQL.
- [ ] Set up a Pact consumer test and provider verification in CI, with `can-i-deploy` blocking a breaking change.
- [ ] Write a Playwright test that reuses a logged-in session, records a trace on failure and passes 50 repeated runs.
- [ ] Write a k6 script with ramping load and thresholds on p95 latency and error rate that fails a CI job when they are breached.
- [ ] Reproduce a flaky test on purpose, using a race condition or shared state, then fix it and explain the root cause.
- [ ] Add SonarQube to a pipeline with a quality gate on new code, and fail a build with a deliberately poor change. *(verify)*

---

**Good answers show:**
- Starting from risk: what would hurt users or the business most, and the cheapest test that would catch it.
- Clear trade-offs between confidence, speed and maintenance cost, rather than one fixed ratio of test types.
- Tests that check behaviour and survive refactoring, with test doubles used only where they earn their place.
- Flaky tests treated as defects with owners, not as noise to retry until the build turns green.
- Load test results tied to service level objectives, with percentiles, a realistic workload and known limits of the environment.
- An understanding that testing continues after release, through canaries, feature flags, monitoring and fast rollback.

**Prove it:** Take a small REST service with PostgreSQL and a web front end, then add unit tests, Testcontainers integration tests, a Pact contract with a second service, two Playwright journeys and a k6 load test with thresholds, all running in CI. Introduce three deliberate bugs, record which layer catches each one, and write a short note on what you would remove if the pipeline had to run in half the time.
