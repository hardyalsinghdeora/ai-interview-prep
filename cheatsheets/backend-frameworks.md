# Backend Frameworks Cheatsheet

Covers key terms for Spring Boot, Django, FastAPI, Flask, Express, NestJS, Laravel and ASP.NET Core, plus object-relational mappers and a framework comparison.

> Related: [Java and Spring Boot skill](../skills/java-and-spring-boot.md) · [Python skill](../skills/python.md) · [Node.js and TypeScript skill](../skills/nodejs-and-typescript.md) · [PHP and Laravel skill](../skills/php-and-laravel.md) · [API design and microservices skill](../skills/api-design-and-microservices.md) · [api-design.md](api-design.md) · [databases-and-sql.md](databases-and-sql.md) · [programming-languages.md](programming-languages.md) · [oop-and-design-patterns.md](oop-and-design-patterns.md) · [testing.md](testing.md) · [Django, FastAPI & Flask skill](../skills/django-fastapi-and-flask.md) · [.NET & C# skill](../skills/dotnet-and-csharp.md)

## Framework comparison

| Framework | Language | Style | Request model | Data layer | Known for |
|---|---|---|---|---|---|
| Spring Boot | Java, Kotlin | Opinionated, full-featured | Thread per request; reactive and virtual threads optional | Spring Data, usually with JPA | Enterprise systems, large ecosystem |
| Django | Python | Batteries included | WSGI; ASGI supported | Django ORM | Admin site, rapid development |
| FastAPI | Python | Lightweight, type-driven | ASGI, asynchronous | None built in, often SQLAlchemy | APIs with automatic documentation |
| Flask | Python | Microframework | WSGI, synchronous | None built in, often SQLAlchemy | Small services, flexibility |
| Express | JavaScript, TypeScript | Minimal, unopinionated | Event loop | None built in | Simplicity, huge middleware ecosystem |
| NestJS | TypeScript | Opinionated, modular | Event loop, on Express or Fastify | Integrates TypeORM, Prisma and others | Structure for large Node.js applications |
| Laravel | PHP | Batteries included | Fresh process state per request; Octane optional | Eloquent | Developer productivity, full tooling |
| ASP.NET Core | C# | Full-featured | Asynchronous on a thread pool | Entity Framework Core | Performance, Microsoft ecosystem |

## Shared terms

| Term | What it means |
|---|---|
| **Middleware** | Code wrapped around each request and response, handling cross-cutting work such as logging or authentication; order matters. |
| **Routing** | Maps an HTTP method and path to the code that handles the request. |
| **MVC** (Model-View-Controller) | Separates data, presentation and request handling, so each can change more independently. |
| **DTO** (Data Transfer Object) | A plain object shaping data at the boundary, which keeps internal models out of public APIs. |
| **Serialisation** | Converts objects to formats such as JSON and back; a frequent source of bugs and data leaks. |
| **WSGI** (Web Server Gateway Interface) | The synchronous Python standard connecting web servers to applications. |
| **ASGI** (Asynchronous Server Gateway Interface) | The asynchronous Python standard, which also supports long-lived connections such as WebSockets. |

## Spring Boot

| Term | What it means |
|---|---|
| **Spring Boot** | An opinionated layer over Spring with embedded servers and sensible defaults; version 3 onwards needs Java 17 or later *(verify)*. |
| **Auto-configuration** | Sets up beans based on the classpath and properties, and backs off when custom beans are defined. |
| **Starter** | A dependency bundle such as `spring-boot-starter-web`, which brings compatible libraries for one capability. |
| **Bean** | An object created and managed by the Spring container; singleton scope by default, so avoid mutable state. |
| **`ApplicationContext`** | Spring's container, which creates beans, wires dependencies and manages their lifecycles. |
| **`@SpringBootApplication`** | Combines `@Configuration`, `@EnableAutoConfiguration` and `@ComponentScan` on the main class. |
| **Stereotype annotations** | `@Component`, `@Service`, `@Repository` and `@Controller` mark classes for scanning and show their role. |
| **`@RestController`** | Combines `@Controller` and `@ResponseBody`, so return values are written directly as the response body. |
| **`@Transactional`** | Wraps a method in a transaction through a proxy; calls within the same class bypass it. |
| **Transaction rollback default** | `@Transactional` rolls back on unchecked exceptions but not checked ones unless configured, a common surprise. |
| **Actuator** | Production endpoints for health, metrics and diagnostics; expose them carefully, as they can reveal sensitive details. |
| **Profile** | Named configuration such as `dev` or `prod`, which activates different settings and beans per environment. |
| **Spring WebFlux** | The non-blocking reactive web stack; it helps only if the whole call path avoids blocking. |

## Django

| Term | What it means |
|---|---|
| **Django** | A full-featured Python framework including ORM, admin, authentication and migrations, which speeds up data-driven applications. |
| **Project and app** | A project is the whole site and its settings; an app is a reusable feature module. |
| **MTV** (Model-Template-View) | Django's version of MVC, where the view holds request logic and the template handles presentation. |
| **QuerySet** | A lazy, chainable database query that runs only when its results are needed. |
| **`select_related` and `prefetch_related`** | The first uses a SQL join, the second a separate query; both prevent N+1 queries. |
| **Django migrations** | Versioned schema changes generated with `makemigrations` and applied with `migrate`. |
| **Django admin** | An automatically generated interface for managing data, which saves building internal tools early on. |
| **DRF** (Django REST Framework) | The common add-on for building APIs, with serializers, viewsets, authentication and permissions. |
| **Signals** | Hooks that run on events such as saving a model; convenient, but they hide control flow. |

## FastAPI

| Term | What it means |
|---|---|
| **FastAPI** | A Python API framework using type hints for validation and documentation, built for asynchronous workloads. |
| **Pydantic** | Validates and serialises data from type hints, which rejects bad requests before business logic runs. |
| **Starlette** | The ASGI toolkit underneath FastAPI, providing routing, middleware and WebSockets. |
| **Uvicorn** | A popular ASGI server that runs FastAPI applications. |
| **`Depends`** | FastAPI's dependency injection, used for shared logic such as database sessions or current user checks. |
| **Automatic API docs** | Generated OpenAPI documentation, served by default at `/docs`, which stays in step with the code. |
| **`async def` vs `def`** | `async def` runs on the event loop, so blocking calls stall everyone; `def` routes use a thread pool. |
| **`BackgroundTasks`** | Runs work in-process after responding; not durable, so use a task queue for important jobs. |

## Flask

| Term | What it means |
|---|---|
| **Flask** | A small Python framework built on Werkzeug and Jinja2, leaving database and structure choices to the team. |
| **Blueprint** | Groups related routes into a module, which keeps larger applications organised. |
| **Application factory** | A `create_app` function that builds the app, which simplifies configuration and testing. |
| **Application and request context** | Make `current_app`, `request` and `g` available during a request, without passing them everywhere. |
| **Gunicorn** | A common production WSGI server; Flask's built-in server is for development only. |

## Express

| Term | What it means |
|---|---|
| **Express** | A minimal Node.js web framework built around middleware, which leaves structure largely to the team. |
| **`next()`** | Passes control to the next middleware; if neither this nor a response happens, the request hangs. |
| **Router** | A mountable group of routes and middleware, which splits an application into modules. |
| **Error-handling middleware** | Middleware with four arguments, `(err, req, res, next)`, which centralises error responses; register it last. |
| **Async error handling** | Express 5 forwards rejected promises to error handlers, whereas Express 4 needs them passed manually *(verify)*. |
| **Blocking the event loop** | CPU-heavy synchronous work stalls every request in a Node.js process, so offload it to workers. |

## NestJS

| Term | What it means |
|---|---|
| **NestJS** | A TypeScript framework with modules, decorators and dependency injection, bringing consistent structure to Node.js applications. |
| **Nest module** | A class marked `@Module` that groups related controllers and providers and controls what is exported. |
| **Nest controller** | Handles routes and delegates business logic to providers. |
| **Provider** | A class marked `@Injectable`, such as a service, which Nest's container can inject. |
| **Guard** | Decides whether a request may continue, which is the usual place for authorisation. |
| **Interceptor** | Wraps handler execution, useful for logging, caching and transforming responses. |
| **Pipe** | Validates or transforms input before it reaches the handler. |
| **Exception filter** | Catches thrown exceptions and turns them into consistent responses. |

```text
Nest request order: middleware -> guards -> interceptors -> pipes -> handler -> interceptors
Exceptions thrown along the way go to exception filters.
```

## Laravel

| Term | What it means |
|---|---|
| **Laravel** | A full-featured PHP framework with routing, ORM, queues, authentication and strong tooling for fast delivery. |
| **Eloquent** | Laravel's Active Record ORM, where each model class maps to a table. |
| **Artisan** | Laravel's command-line tool for generating code, running migrations and managing tasks. |
| **Blade** | Laravel's templating engine, which compiles views to plain PHP. |
| **Service container** | Laravel's dependency injection system, which builds classes and resolves their dependencies. |
| **Service provider** | The central place to register services and bootstrap an application's features. |
| **Laravel facade** | A static-looking interface to a container service, such as `Cache::get()`; convenient, though it hides dependencies. |
| **Route model binding** | Automatically turns a route parameter into a model instance, which removes repetitive lookup code. |
| **Laravel Octane** | Keeps the application in memory between requests for speed; state can leak across requests if careless. |

## ASP.NET Core

| Term | What it means |
|---|---|
| **ASP.NET Core** | Microsoft's cross-platform, open-source web framework for C#, known for strong performance and first-class dependency injection. |
| **Kestrel** | The default cross-platform web server, which often sits behind a reverse proxy. |
| **Middleware pipeline** | Ordered components configured at startup that each handle a request; incorrect order breaks features such as authentication. |
| **Minimal API** | Defines endpoints with concise lambda handlers and no controllers, which suits small services. |
| **Controller-based API** | Groups endpoints in classes with `[ApiController]`, offering conventions that suit larger APIs. |
| **Service lifetimes** | Singleton lasts for the app, Scoped for a request and Transient per injection; mixing them badly causes bugs. |
| **`appsettings.json`** | The main configuration file, layered with environment-specific files and environment variables. |
| **`BackgroundService`** | A base class for long-running work hosted inside the application process. |
| **Action filter** | Runs code around controller actions, handling cross-cutting concerns with access to model binding results. |

## ORMs

| Term | What it means |
|---|---|
| **ORM** (Object-Relational Mapping) | Maps database rows to objects, which reduces boilerplate but can hide inefficient SQL. |
| **JPA** (Jakarta Persistence API) | The Java persistence standard, formerly Java Persistence API; it defines the interface rather than an implementation. |
| **Hibernate** | The most widely used JPA implementation, adding its own caching and querying features. |
| **Persistence context** | Hibernate's tracked set of loaded entities for a session, which guarantees one object per row. |
| **Dirty checking** | Detects changes to tracked entities and writes them at flush, with no explicit save call. |
| **EF Core** (Entity Framework Core) | .NET's main ORM; `DbContext` tracks changes and acts as a unit of work. |
| **LINQ** (Language Integrated Query) | C# query syntax that EF Core translates into SQL, giving queries compile-time checking. |
| **SQLAlchemy** | Python's leading database toolkit, offering both a SQL expression layer and a Data Mapper ORM. |
| **Prisma** | A TypeScript ORM generating a type-safe client from a schema file, popular for its developer experience. |
| **TypeORM** | A decorator-based TypeScript ORM supporting both Active Record and Data Mapper styles. |
| **N+1 query problem** | One query loads a list, then one more runs per row; fix with joins or batched loading. |
| **Lazy vs eager loading** | Lazy fetches relations on first access, while eager fetches upfront; lazy is risky inside loops. |
| **Unit of Work** | Tracks changes during a business operation and commits them together, which keeps writes consistent. |
| **Optimistic locking** | Uses a version column to detect conflicting updates at write time, without holding database locks. |
| **Schema migration tools** | Flyway and Liquibase for Java, Alembic for SQLAlchemy; they apply versioned schema changes reliably. |

| Active Record vs Data Mapper | Active Record | Data Mapper |
|---|---|---|
| Persistence logic | On the model, such as `user.save()` | In a separate mapper or session |
| Coupling to the database | High | Lower |
| Best for | Simple create-read-update-delete applications | Complex domains |
| Examples | Eloquent, Django ORM | Hibernate, SQLAlchemy ORM, EF Core |
