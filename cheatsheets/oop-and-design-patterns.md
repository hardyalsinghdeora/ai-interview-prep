# OOP and Design Patterns Cheatsheet

Covers object-oriented principles, SOLID, the Gang of Four patterns, dependency injection, clean and hexagonal architecture, Domain-Driven Design and a low-level design checklist.

> Related: [Java and Spring Boot skill](../skills/java-and-spring-boot.md) · [API design and microservices skill](../skills/api-design-and-microservices.md) · [System design](../topics/03-system-design/README.md) · [Decomposition](../topics/04-decomposition/README.md) · [programming-languages.md](programming-languages.md) · [system-design.md](system-design.md) · [testing.md](testing.md) · [OOP, design patterns & LLD skill](../skills/oop-design-patterns-and-lld.md)

## OOP basics

| Term | What it means |
|---|---|
| **OOP** (Object-Oriented Programming) | Organises software as objects combining data and behaviour, which helps model domains and manage change. |
| **Class** | A template defining the data and behaviour of a kind of object. |
| **Object** | A runtime instance of a class, with its own state. |
| **Encapsulation** | Keeps data private behind methods, so internals can change and rules cannot be bypassed. |
| **Abstraction** | Exposes only what callers need and hides the details, which reduces what they must understand. |
| **Inheritance** | A subclass reuses and extends a parent class; powerful, but it creates tight coupling. |
| **Polymorphism** | One interface with many implementations, so callers can use new types without changing. |
| **Interface** | A contract of methods without committing to an implementation, which lets implementations be swapped. |
| **Abstract class** | A partly implemented class that cannot be instantiated, used to share code across related subclasses. |
| **Association** | A general relationship in which one object uses or refers to another. |
| **Aggregation** | A "has-a" relationship where the part can outlive the whole, such as a team and its members. |
| **Composition** | A "has-a" relationship where the whole owns the part's lifetime, such as an order and its lines. |
| **Composition over inheritance** | Prefer combining objects to subclassing, which keeps behaviour swappable and avoids fragile hierarchies. |
| **Coupling** | How much one module depends on another; lower coupling means changes spread less. |
| **Cohesion** | How closely a module's responsibilities belong together; higher cohesion makes code easier to understand. |

| Overloading vs overriding | Overloading | Overriding |
|---|---|---|
| Signature | Same name, different parameters | Same signature as the parent |
| Where | Usually same class | Subclass |
| Resolved | Compile time | Runtime |
| Polymorphism type | Static | Dynamic |

| Interface vs abstract class | Interface | Abstract class |
|---|---|---|
| Main purpose | Define a capability | Share a base implementation |
| Instance state | Generally none | Allowed |
| Multiple per class | Usually yes | Usually one parent |
| Use when | Unrelated types share a contract | Related types share code |

## SOLID and related principles

| Term | What it means |
|---|---|
| **SRP** (Single Responsibility Principle) | A class should have one reason to change, so unrelated concerns do not break each other. |
| **OCP** (Open-Closed Principle) | Open for extension and closed for modification, so new behaviour avoids risky edits to working code. |
| **LSP** (Liskov Substitution Principle) | Subtypes must work wherever their base type is expected, without callers seeing surprises. |
| **ISP** (Interface Segregation Principle) | Prefer small, focused interfaces, so clients do not depend on methods they never use. |
| **DIP** (Dependency Inversion Principle) | High-level and low-level code both depend on abstractions, which isolates business rules from technical details. |
| **DRY** (Don't Repeat Yourself) | Keep each piece of knowledge in one place; merging merely similar code can be worse. |
| **KISS** (Keep It Simple, Stupid) | Prefer the simplest design that works, since complexity carries a long-term cost. |
| **YAGNI** (You Aren't Gonna Need It) | Do not build for imagined future needs, which avoids paying for unused flexibility. |
| **Law of Demeter** | Talk only to immediate collaborators, avoiding chains like `a.getB().getC()` that spread structural knowledge. |

## Creational patterns

| Term | What it means |
|---|---|
| **Factory Method** | A method, overridable by subclasses, that decides which class to create, decoupling callers from concrete types. |
| **Abstract Factory** | Creates families of related objects, such as themed widgets, keeping each family consistent. |
| **Builder** | Constructs a complex object step by step, avoiding constructors with long parameter lists. |
| **Prototype** | Creates new objects by copying an existing one, useful when setup is expensive. |
| **Singleton** | Ensures one instance with global access; it hides dependencies and hampers testing, so often better avoided. |

## Structural patterns

| Term | What it means |
|---|---|
| **Adapter** | Converts one interface into another, so incompatible code such as a third-party library can fit. |
| **Bridge** | Separates an abstraction from its implementation, so each can vary independently and subclasses do not multiply. |
| **Composite** | Treats single objects and groups uniformly through a tree, such as files and folders. |
| **Decorator** | Wraps an object to add behaviour at runtime, such as logging, without changing the class. |
| **Facade** | Provides one simple interface over a complicated subsystem, which reduces what callers must know. |
| **Flyweight** | Shares common state among many similar objects, which saves memory at large scale. |
| **Proxy** | A stand-in that controls access to an object, for lazy loading, caching or security checks. |

| Lookalikes | Interface offered | Purpose |
|---|---|---|
| Adapter | Different from the wrapped object | Make incompatible code fit |
| Decorator | Same as the wrapped object | Add behaviour |
| Proxy | Same as the wrapped object | Control access |
| Facade | New, simpler, over many objects | Hide complexity |

## Behavioral patterns

| Term | What it means |
|---|---|
| **Chain of Responsibility** | Passes a request along handlers until one deals with it, as in web middleware. |
| **Command** | Wraps a request as an object, enabling queues, logging and undo. |
| **Interpreter** | Represents a small language's grammar as classes, so its expressions can be evaluated. |
| **Iterator** | Walks through a collection without exposing how it is stored. |
| **Mediator** | Routes communication through a central object, so components do not depend on one another. |
| **Memento** | Captures and restores an object's state without breaking encapsulation, as used for undo. |
| **Observer** | Notifies subscribers automatically when a subject changes, which decouples event sources from reactions. |
| **State** | Changes an object's behaviour by swapping internal state objects, replacing large conditional blocks. |
| **Strategy** | Makes algorithms interchangeable behind one interface, such as selecting among pricing rules at runtime. |
| **Template Method** | A base class fixes an algorithm's outline, and subclasses fill in particular steps. |
| **Visitor** | Adds operations to an object structure without changing its classes; adding new element types becomes harder. |

| Strategy vs state | Strategy | State |
|---|---|---|
| Who chooses | Usually the client | The object or its states |
| Do variants know each other | No | Often, to trigger transitions |
| Models | Interchangeable algorithms | Lifecycle stages |

## Dependency injection

| Term | What it means |
|---|---|
| **IoC** (Inversion of Control) | A framework or caller controls the flow and wiring rather than application code, which reduces coupling. |
| **DI** (Dependency Injection) | Objects receive dependencies from outside rather than creating them, which makes swapping and testing easy. |
| **DI container** (Dependency Injection container) | Framework component that builds objects and supplies their dependencies, such as Spring's application context. |
| **Constructor injection** | Dependencies are passed at construction, so they are explicit, can be final and are always present. |
| **Setter injection** | Dependencies are set through methods after creation, which suits optional dependencies only. |
| **Field injection** | A framework sets private fields directly; concise, but it hides dependencies and complicates testing. |
| **Service Locator** | Code asks a central registry for dependencies; usually discouraged because needs are hidden from the signature. |

```java
class OrderService {
    private final PaymentGateway gateway;           // depends on an interface

    OrderService(PaymentGateway gateway) {          // constructor injection
        this.gateway = gateway;
    }
}

// A test can pass a fake without any framework
new OrderService(new FakePaymentGateway());
```

## Clean and hexagonal architecture

| Term | What it means |
|---|---|
| **Layered architecture** | Organises code into presentation, business and data layers; simple, though business logic often ends up depending on the database. |
| **Clean architecture** | Robert C. Martin's concentric design with business rules at the centre, independent of frameworks, databases and interfaces. |
| **Dependency rule** | Source code dependencies point only inwards, so inner business rules know nothing of outer details. |
| **Use case** | Application-specific logic coordinating domain objects to fulfil one user goal. |
| **Hexagonal architecture** | Alistair Cockburn's ports and adapters style, which isolates the application core from all outside technology. |
| **Port** | An interface owned by the core that defines how the outside world interacts with it. |
| **Adapter** (hexagonal) | Connects a port to specific technology, such as a REST controller or SQL repository. |
| **Driving adapter** | Calls into the application, such as an HTTP controller or test; also called primary. |
| **Driven adapter** | Called by the application, such as a database or email client; also called secondary. |

| Comparison | Layered | Clean or hexagonal |
|---|---|---|
| Core depends on | Often the data layer | Nothing external |
| Test business logic | Usually needs data access or heavy mocks | With simple fakes of ports |
| Replace a database | Often invasive | Replace an adapter |
| Upfront cost | Low | Higher, more interfaces and mapping |
| Best for | Simple applications, create-read-update-delete work | Complex, long-lived domains |

## DDD terms

| Term | What it means |
|---|---|
| **DDD** (Domain-Driven Design) | Eric Evans's approach of shaping software around a shared domain model, valuable mainly for complex business areas. |
| **Ubiquitous language** | One shared vocabulary for experts and developers in a context, used in code and conversation. |
| **Bounded context** | A boundary within which a model and its terms have one meaning; often a guide to service boundaries. |
| **Context map** | Shows how bounded contexts relate and which teams or systems depend on which. |
| **Core subdomain** | The area giving competitive advantage, so it deserves the best modelling effort; generic subdomains can often be bought. |
| **Anti-corruption layer** | Translates between another system's model and one's own, which keeps legacy concepts from leaking in. |
| **Entity** | A domain object with lasting identity, even as its attributes change, such as a customer. |
| **Value object** | An immutable object defined by its values, with no identity, such as money or an address. |
| **Aggregate** | A cluster of objects treated as one consistency boundary, generally changed in a single transaction. |
| **Aggregate root** | The sole entry point to an aggregate, which enforces its rules; outside code references only this. |
| **Domain event** | A record of something significant that happened in the business, named in the past tense. |
| **Repository** | A collection-like interface for loading and saving aggregates, hiding persistence from the domain. |
| **Domain service** | Holds domain logic that belongs to no single entity or value object, such as a transfer between accounts. |

| Entity vs value object | Entity | Value object |
|---|---|---|
| Equality | By identity | By values |
| Mutability | Can change over time | Immutable, replace to change |
| Example | Order, customer | Money, date range |

## Low-level design checklist

| Term | What it means |
|---|---|
| **Clarify requirements** | Confirm core use cases, actors, constraints and what is explicitly out of scope before drawing anything. |
| **Find the entities** | Pick out nouns as possible classes and verbs as behaviour, then remove anything that adds nothing. |
| **Assign responsibilities** | Give each class one clear job, and place behaviour with the data it uses. |
| **Define relationships** | Decide on association, composition or inheritance, along with multiplicity, favouring composition where in doubt. |
| **Define interfaces** | Depend on abstractions at points likely to vary, such as payments, notifications or storage. |
| **Choose patterns** | Apply a pattern only to a concrete problem, such as Strategy for varying pricing rules. |
| **Handle state** | Model lifecycle states and valid transitions explicitly, so invalid changes are rejected. |
| **Handle concurrency** | Identify shared data and protect it with locks, atomic operations or immutability, for example when booking one seat. |
| **Handle failure and edge cases** | Cover invalid input, partial failure, retries and empty or duplicate cases. |
| **Walk through and test** | Trace a key use case through the classes, then confirm the design is testable and extensible. |
