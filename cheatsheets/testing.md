# Testing Cheatsheet

Covers test types and strategy, test doubles, unit and integration tools, contract and browser testing, load testing, coverage, flaky tests, TDD and BDD.

> Related: [Java and Spring Boot skill](../skills/java-and-spring-boot.md) · [Python skill](../skills/python.md) · [Node.js and TypeScript skill](../skills/nodejs-and-typescript.md) · [Docker and CI/CD skill](../skills/docker-and-ci-cd.md) · [Coding](../topics/02-coding/README.md) · [ci-cd-tools.md](ci-cd-tools.md) · [ci-cd-and-iac.md](ci-cd-and-iac.md) · [programming-languages.md](programming-languages.md) · [Testing & quality skill](../skills/testing-and-quality.md)

## Test types

| Term | What it means |
|---|---|
| **Unit test** | Checks one small piece of logic in isolation; fast and precise, so these form the bulk of a suite. |
| **Integration test** | Checks that components work together, such as code and a real database; slower but finds wiring problems. |
| **Component test** | Exercises one service or user interface component through its interface, with external dependencies replaced. |
| **Contract test** | Checks that a consumer and provider agree on an interface, without deploying both together. |
| **E2E test** (End-to-End test) | Drives a full user journey through the deployed system; realistic but slow and more fragile. |
| **Smoke test** | A small, quick check that the most important paths work, often run straight after deployment. |
| **Regression test** | Confirms that previously working behaviour still works; every fixed bug should ideally gain one. |
| **UAT** (User Acceptance Testing) | Business users or customers confirm the system meets their needs before release. |
| **Exploratory testing** | Skilled people investigate without a fixed script, finding problems automated checks would not anticipate. |
| **Snapshot test** | Compares output with a stored copy; cheap to write, but easily approved without proper review. |
| **Property-based test** | Generates many inputs and checks a rule always holds, which finds edge cases nobody listed. |
| **Mutation testing** | Introduces small code changes and checks that tests fail, which measures test strength rather than execution. |
| **Load test** | Applies expected traffic to confirm that latency and error targets hold. |
| **Stress test** | Pushes beyond expected traffic to find the breaking point and how the system fails. |
| **Spike test** | Applies a sudden traffic jump to check scaling and recovery. |
| **Soak test** | Runs steady load for many hours to expose memory leaks and slow degradation. |

## Test strategy

| Term | What it means |
|---|---|
| **Test pyramid** | Many unit tests, fewer integration tests and very few end-to-end tests, which keeps suites fast and reliable. |
| **Ice cream cone** | The inverted anti-pattern, with mostly manual and end-to-end tests, leading to slow and unreliable feedback. |
| **Testing trophy** | A front-end-oriented alternative that favours integration tests, because they best match how users exercise software. |
| **AAA** (Arrange, Act, Assert) | A test layout: set up, perform one action, then check the result, which keeps tests readable. |
| **Test isolation** | Each test sets up its own state and does not depend on others, so tests can run in any order. |
| **Test fixture** | The fixed setup a test relies on, such as prepared data or objects. |

| Level | Speed | Confidence per test | Cost to maintain | Share of suite |
|---|---|---|---|---|
| Unit | Milliseconds | Low to medium | Low | Largest |
| Integration | Seconds | Medium to high | Medium | Moderate |
| End-to-end | Seconds to minutes | High | High | Smallest |

## Mocks, stubs and fakes

| Term | What it means |
|---|---|
| **Test double** | The general term for any object standing in for a real dependency during a test. |
| **Dummy** | An object passed only to satisfy a signature and never actually used. |
| **Stub** | Returns canned answers to calls, which controls the indirect inputs to the code under test. |
| **Spy** | Records how it was called, often wrapping a real object, so calls can be checked afterwards. |
| **Mock** | Programmed with expected calls and used to verify interactions; overuse ties tests to implementation details. |
| **Fake** | A working but simplified implementation, such as an in-memory repository; not suitable for production. |

| Stub vs mock vs fake | Stub | Mock | Fake |
|---|---|---|---|
| Has real logic | No | No | Yes, simplified |
| What the test checks | Resulting state | Calls made | Resulting state |
| Typical use | Force a response or error | Check an email was sent | Replace a repository |

## JUnit and Java

| Term | What it means |
|---|---|
| **JUnit** | The standard Java testing framework; annotations here are from the Jupiter API introduced in JUnit 5, and newer majors exist *(verify)*. |
| **`@Test`** | Marks a method as a test case for the JUnit runner to discover and execute. |
| **`@BeforeEach`** | Runs setup before every test method, giving each test fresh state; `@AfterEach` handles cleanup. |
| **`@ParameterizedTest`** | Runs one test with several inputs, supplied by annotations such as `@CsvSource`, reducing duplicated test code. |
| **Mockito** | The popular Java mocking library, using `when(...)` to stub calls and `verify(...)` to check interactions. |
| **AssertJ** | A fluent assertion library that makes checks and failure messages easier to read. |
| **Surefire and Failsafe** | Maven plugins: Surefire runs unit tests and Failsafe runs integration tests, usually in separate build phases. |

## pytest

| Term | What it means |
|---|---|
| **pytest** | The most common Python test framework, using plain `assert` statements with detailed failure output. |
| **pytest fixture** | A function providing setup through a test's arguments, with a chosen scope; `yield` allows teardown afterwards. |
| **`conftest.py`** | A file whose fixtures are available to tests in its directory tree, without any import. |
| **`@pytest.mark.parametrize`** | Runs one test with multiple input sets, each reported as a separate case. |
| **`monkeypatch`** | A built-in fixture that temporarily changes attributes or environment variables, then restores them automatically. |
| **Marker** | A label such as `@pytest.mark.slow`, used to select or skip groups of tests. |

```python
import pytest

@pytest.fixture
def client():
    c = make_client()
    yield c
    c.close()

@pytest.mark.parametrize("value,expected", [(1, 2), (2, 4)])
def test_double(client, value, expected):
    assert client.double(value) == expected
```

## Jest

| Term | What it means |
|---|---|
| **Jest** | A popular JavaScript test framework bundling a runner, assertions, mocking and coverage. |
| **Matcher** | An assertion method such as `toBe` or `toEqual`; `toEqual` compares structure rather than identity. |
| **`jest.fn()`** | Creates a mock function that records calls and can return chosen values. |
| **`jest.mock()`** | Replaces a whole module with a mock, which isolates code from real dependencies. |
| **`jest.spyOn()`** | Watches an existing method, optionally replacing its behaviour, and can be restored afterwards. |
| **Fake timers** | Replaces real timers with controllable ones, so time-based code is tested without waiting. |
| **RTL** (React Testing Library) | Tests components through what users see, usually alongside Jest, rather than through internal details. |
| **Vitest** | A Vite-based test runner with a largely Jest-compatible interface, common in Vite projects. |

## Testcontainers, Pact and WireMock

| Term | What it means |
|---|---|
| **Testcontainers** | A library that starts throwaway Docker containers from test code, so tests use real databases and brokers. |
| **Wait strategy** | Tells Testcontainers when a container is really ready, preventing failures from connecting too early. |
| **Ryuk** | A helper container that removes test containers afterwards, even when a test run crashes. |
| **WireMock** | Runs a fake HTTP server with programmed responses, which simulates third-party APIs and failures. |
| **Pact** | A contract testing tool where tests on the consumer side generate the contract that the provider verifies. |
| **Consumer-driven contract** | The consumer states what it needs from a provider, so the provider can change anything else safely. |
| **Provider verification** | Replays contract interactions against the real provider, confirming that its responses still satisfy consumers. |
| **Pact Broker** | A shared service storing contracts and verification results, connecting consumer and provider pipelines. |
| **`can-i-deploy`** | A Pact Broker check showing whether a version is compatible with what is deployed, used as a release gate. |

## Browser testing

| Term | What it means |
|---|---|
| **Playwright** | Microsoft's browser automation framework for Chromium, Firefox and WebKit, with several language bindings. |
| **Locator** | Playwright's way of finding elements, retried at use; role and text locators are more resilient than CSS paths. |
| **Auto-waiting** | Playwright checks an element is ready before acting, which removes most manual sleeps. |
| **Browser context** | An isolated, cheap browser profile, giving each test its own cookies and storage. |
| **Trace Viewer** | Replays a recorded run with actions, screenshots and network calls, which helps debug CI failures. |
| **Cypress** | A JavaScript end-to-end framework running alongside the application in the browser, known for its interactive runner. |
| **`cy.intercept()`** | Stubs or watches network requests in Cypress, which makes tests quicker and more deterministic. |
| **Selenium** | The long-established browser automation project, with many language bindings and wide browser support. |
| **WebDriver** | The W3C standard interface for controlling browsers, on which Selenium is built. |
| **Selenium Grid** | Runs tests across many browsers and machines in parallel. |
| **Page Object Model** | Wraps each page's elements and actions in a class, so user interface changes need updating in one place. |

| Feature | Playwright | Cypress | Selenium |
|---|---|---|---|
| Languages | JS/TS, Python, Java, .NET | JS/TS | Java, Python, C#, JS, Ruby and others |
| Browsers | Chromium, Firefox, WebKit | Chromium family, Firefox, WebKit experimental *(verify)* | All major browsers |
| Waiting | Automatic | Automatic retries | Mostly explicit |
| Parallel runs | Built in | Mainly through its paid cloud *(verify)* | Through Grid |
| Multiple tabs | Yes | Limited | Yes |

## Load testing tools

| Term | What it means |
|---|---|
| **k6** | A Grafana Labs load testing tool, scripted in JavaScript, which fits easily into CI pipelines. |
| **VU** (Virtual User) | A simulated client running a script repeatedly; the main way of expressing concurrency in k6. |
| **k6 threshold** | A pass or fail criterion such as 95th percentile latency, which can fail the run in CI. |
| **Think time** | A pause between actions that mimics real users; without it, load is unrealistically aggressive. |
| **Apache JMeter** | A long-standing Java load testing tool, with a graphical test designer; run without the interface for real load. |
| **Thread Group** | JMeter's set of virtual users, defining how many, the ramp-up and duration. |
| **Sampler** | A JMeter element that sends a request, such as HTTP or JDBC. |
| **Listener** | A JMeter element that collects and displays results; heavy listeners can distort a load test. |
| **Open vs closed model** | Open sets arrival rate regardless of response time; closed loops fixed users, which can hide overload. |

```javascript
import http from 'k6/http';

export const options = {
  vus: 50,
  duration: '2m',
  thresholds: { http_req_duration: ['p(95)<300'] },
};

export default function () {
  http.get('https://test.example.com/health');
}
```

## Coverage

| Term | What it means |
|---|---|
| **Code coverage** | The share of code that tests execute; shows what is untested, but not whether assertions are any good. |
| **Line coverage** | The percentage of lines executed, which is the simplest and weakest common measure. |
| **Branch coverage** | The percentage of decision outcomes executed, which reveals untested conditions that line coverage misses. |
| **Mutation score** | The percentage of introduced code changes that tests catch, a stronger signal of test quality. |
| **JaCoCo** (Java Code Coverage) | The usual Java coverage library, whose reports feed build gates and tools such as SonarQube. |
| **Coverage.py** | The standard Python coverage tool, often run through the `pytest-cov` plugin. |
| **Istanbul** | The JavaScript coverage instrumentation behind `nyc`, and one of the coverage providers for Jest. |

## Flaky tests

| Term | What it means |
|---|---|
| **Flaky test** | A test that passes and fails without code changes, which teaches teams to ignore real failures. |
| **Common flaky causes** | Timing assumptions, shared state, test order, real networks, unseeded randomness and time zones. |
| **Test quarantine** | Moves a known flaky test out of the blocking suite until fixed; needs an owner, or it becomes permanent. |
| **Automatic retry** | Reruns failures to reduce noise; useful short term, but it can hide real intermittent bugs. |
| **Deterministic control** | Injecting the clock, random seeds and external responses, so a test behaves identically each run. |

## TDD and BDD

| Term | What it means |
|---|---|
| **TDD** (Test-Driven Development) | Writing a failing test before the code, which drives design in small steps and leaves a safety net. |
| **Red, green, refactor** | The TDD cycle: write a failing test, make it pass simply, then improve the code. |
| **BDD** (Behaviour-Driven Development) | Describes behaviour in shared business language, which aligns developers, testers and domain experts. |
| **Gherkin** | The structured plain-text language for BDD scenarios, using keywords such as `Given`, `When` and `Then`. |
| **Cucumber** | A popular tool that links Gherkin scenarios to step code and runs them as tests. |
| **ATDD** (Acceptance Test-Driven Development) | The team agrees acceptance tests before building, which makes "done" clear from the start. |

| TDD vs BDD | TDD | BDD |
|---|---|---|
| Focus | Code design and correctness | Shared understanding of behaviour |
| Audience | Developers | Developers, testers, business |
| Language | Programming language | Natural language, such as Gherkin |
| Typical scope | Units | Features and scenarios |

```gherkin
Scenario: Expired card is declined
  Given a basket containing one item
  When the customer pays with an expired card
  Then the payment is declined
```
