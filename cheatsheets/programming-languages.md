# Programming Languages Cheatsheet

The terms interviewers commonly use for Node.js and TypeScript, Go, Python, Java and PHP, with a side-by-side comparison of their runtimes.

> Related: [cs-fundamentals.md](cs-fundamentals.md) · [data-structures-and-algorithms.md](data-structures-and-algorithms.md) · [api-design.md](api-design.md) · [Coding topic](../topics/02-coding/README.md) · [Node.js and TypeScript skill](../skills/nodejs-and-typescript.md) · [Go skill](../skills/go.md) · [Python skill](../skills/python.md) · [Java and Spring Boot skill](../skills/java-and-spring-boot.md) · [PHP and Laravel skill](../skills/php-and-laravel.md)

## Shared concepts and comparison

| Term | What it means |
|---|---|
| **Static typing** | Types are checked before running. Catches many errors early and improves tooling, with some extra annotation effort. |
| **Dynamic typing** | Types are checked at runtime. Faster to write, but type mistakes surface later unless tests or analysers catch them. |
| **Structural typing** | Compatibility depends on an object's shape, not its declared name. Used by TypeScript and, for interfaces, Go. |
| **Nominal typing** | Compatibility depends on explicitly declared types. Used by Java, making relationships explicit at the cost of flexibility. |
| **Generics** | Code parameterised by type, such as `List<String>`. Provides reuse with type safety, avoiding casts or duplicated code. |
| **Type erasure** | Generic or static type information is removed before runtime. Java generics and TypeScript types work this way, limiting runtime type checks. |
| **JIT compilation** (Just-In-Time) | Compiles frequently run code to machine code during execution. Fast long-running performance, with warm-up time. |
| **AOT compilation** (Ahead-Of-Time) | Compiles to native machine code before running, as Go does. Fast startup and simple deployment. |
| **Closure** | Function that remembers variables from its enclosing scope. Powerful for callbacks, but can keep memory alive longer than expected. |

| Aspect | Node.js and TypeScript | Go | Python | Java | PHP |
|---|---|---|---|---|---|
| Execution | V8 JIT | Native binary | CPython bytecode interpreter | JVM bytecode plus JIT | Zend Engine, OPcache, optional JIT |
| Typing | Dynamic JavaScript; static, structural TypeScript | Static; structural interfaces | Dynamic; optional type hints | Static, nominal | Dynamic; optional type declarations |
| Concurrency | Event loop, async/await | Goroutines, channels | Threads, `asyncio` | Threads, virtual threads, executors | Per-request workers; Fibers for libraries |
| CPU parallelism | Worker threads, processes | Goroutines across cores | `multiprocessing`; free-threaded build *(verify)* | Threads | Multiple worker processes |
| Memory management | Generational GC | Concurrent mark-sweep GC | Reference counting plus cycle GC | Generational and region-based GCs | Reference counting plus cycle GC |
| Generics | Compile-time, erased | Type parameters | Type hints only | Compile-time, erased | Not native; static analyser annotations |
| Errors | Exceptions, rejected Promises | Returned `error` values | Exceptions | Checked and unchecked exceptions | Exceptions |
| Packages | npm, pnpm, Yarn | Go modules | pip, Poetry, uv | Maven, Gradle | Composer |
| Common use | I/O-heavy APIs, full stack | Infrastructure, network services | Data, AI, scripting, web | Enterprise back ends | Web applications |

## Node.js

| Term | What it means |
|---|---|
| **V8** | Google's JavaScript engine that compiles JavaScript to machine code. Provides Node's execution speed and generational garbage collection. |
| **libuv** | C library providing Node's event loop and asynchronous I/O. Abstracts `epoll`, `kqueue` and IOCP across operating systems. |
| **Event loop** (Node.js) | Single main thread cycling through phases that run ready callbacks. Handles many connections, but blocking code stalls them all. |
| **Microtask queue** | Runs Promise callbacks after the current operation, before the loop moves on. Endless microtasks can starve I/O. |
| **`process.nextTick`** | Queues a callback ahead of Promise microtasks. Overuse can starve the event loop. |
| **`setImmediate`** | Runs a callback in the check phase, after polling for I/O. Useful for yielding during long work. |
| **Promise** | Object representing a future result. `async`/`await` is cleaner syntax over it; unhandled rejections can terminate a process. |
| **libuv thread pool** | Background threads for file system, DNS lookup and some crypto work, four by default. Saturation can delay these operations *(verify)*. |
| **Worker threads** | Run JavaScript in parallel threads with separate event loops. Suit CPU-heavy tasks that would otherwise block the main thread. |
| **Cluster module** | Forks several Node processes sharing a server port. Uses multiple CPU cores, though containers often replace it with more replicas. |
| **Stream** | Processes data in chunks rather than loading it all. Saves memory and supports backpressure through `pipeline`. |
| **CommonJS** | Node's original synchronous `require` module system. Still common, but differs in loading behaviour from standard modules. |
| **ESM** (ECMAScript Modules) | Standard `import` and `export` system. Allows static analysis and top-level `await`. |

```text
Event loop phases: timers -> pending callbacks -> idle, prepare -> poll -> check -> close callbacks
Between callbacks: process.nextTick queue, then Promise microtasks
```

## TypeScript

| Term | What it means |
|---|---|
| **`any`** | Turns off type checking for a value. Convenient for migration, but removes the safety TypeScript exists for. |
| **`unknown`** | Accepts any value but must be narrowed before use. The safe alternative to `any` for untrusted data. |
| **`never`** | Type with no possible values. Used for functions that never return and exhaustive checks. |
| **Union type** | A value may be one of several types. Models alternatives, with narrowing needed before type-specific use. |
| **Intersection type** | A value satisfies several types at once, written with `&`. Combines object shapes. |
| **Type narrowing** | Compiler refines a type after checks such as `typeof` or `in`. Lets union values be used safely. |
| **Discriminated union** | Union members share a literal field such as `kind`. Enables exhaustive checks and clear state modelling. |
| **Generic constraint** | Limits a type parameter using `extends`, such as `T extends { id: string }`. Allows safe property use inside generic code. |
| **Utility types** | Built-in helpers such as `Partial`, `Pick`, `Omit` and `Record`. Derive new types without duplication. |
| **Declaration file** (`.d.ts`) | Holds type information without implementation. Adds types for JavaScript libraries. |
| **Runtime validation** | TypeScript types do not exist at runtime, so external data needs checks with libraries such as Zod. |

## Go

| Term | What it means |
|---|---|
| **Goroutine** | Lightweight function managed by the Go runtime, starting with a small stack. Thousands or millions can run cheaply. |
| **GMP scheduler** (Goroutine, Machine, Processor) | Multiplexes goroutines onto operating system threads through logical processors. Gives cheap concurrency, with work stealing across cores. |
| **`GOMAXPROCS`** | Maximum threads executing Go code simultaneously, by default based on available CPUs *(verify)*. Affects throughput in CPU-limited containers. |
| **Channel** | Typed conduit for passing values between goroutines. Enables communicating instead of directly sharing memory. |
| **Unbuffered channel** | Send and receive must meet, so each synchronises the other. Use when handover itself matters. |
| **Buffered channel** | Holds up to a set number of values before senders block. Decouples bursts and naturally limits queued work. |
| **`select`** | Waits on several channel operations, proceeding with one that is ready. Used for timeouts, cancellation and fan-in. |
| **`sync.WaitGroup`** | Waits until a set of goroutines finishes. The basic tool for joining concurrent work. |
| **`context.Context`** | Carries cancellation, deadlines and request-scoped values across calls. Standard for stopping work and preventing goroutine leaks. |
| **Goroutine leak** | Goroutine blocked forever, often on a channel. Memory grows silently, so every goroutine needs a clear exit path. |
| **`defer`** | Schedules a call to run when the function returns, in last-in-first-out order. Reliable cleanup for files and locks. |
| **`panic` and `recover`** | Panic unwinds the stack for unrecoverable problems; `recover` inside `defer` stops it. Normal failures should return errors. |
| **Implicit interface** | A type satisfies an interface by having its methods, with no declaration. Lets consumers define small interfaces. |
| **Slice** | View over an array with pointer, length and capacity. Shared backing arrays make `append` and sub-slices a common bug source. |
| **Escape analysis** | Compiler decides whether a value lives on the stack or heap. Fewer escapes mean less garbage collection work. |
| **Go GC** (Garbage Collector) | Concurrent, non-generational, tri-colour mark-sweep collector. Designed for short pauses, trading some throughput for latency. |
| **Race detector** | Tool enabled with `go test -race` that reports data races at runtime. Only finds races that actually execute. |
| **Zero value** | Variables default to a usable empty value, such as 0 or `nil`. Reduces initialisation code, though writing to a `nil` map panics. |

## Python

| Term | What it means |
|---|---|
| **CPython** | The reference and most common Python implementation. Interprets bytecode, and most Python runtime behaviour assumes it. |
| **GIL** (Global Interpreter Lock) | Lets one thread at a time execute Python bytecode per interpreter. Limits CPU parallelism; threads still help when waiting on I/O. |
| **Free-threaded Python** | Optional CPython build without the GIL, from PEP 703 (Python Enhancement Proposal). Not yet the default and package support varies *(verify)*. |
| **`threading`** | Standard module for operating system threads. Best for I/O waits, since the GIL limits CPU-bound work in standard builds. |
| **`multiprocessing`** | Runs separate interpreter processes, each with its own GIL. Enables CPU parallelism, with higher memory and data transfer costs. |
| **`asyncio`** | Single-threaded event loop running coroutines with `async` and `await`. Suits many concurrent network calls; blocking calls stall it. |
| **Python memory management** | Reference counting frees most objects immediately, and a cycle collector handles reference cycles. Explains generally predictable cleanup in CPython. |
| **Decorator** | Function wrapping another function or class, applied with `@`. Used for logging, caching and access checks. |
| **Generator** | Function using `yield` to produce values lazily. Saves memory when processing large or endless sequences. |
| **Context manager** | Object used with `with` to set up and clean up resources. Ensures files and locks are released despite exceptions. |
| **Duck typing** | Objects are judged by available methods, not declared type. Flexible, with mistakes surfacing only at runtime. |
| **Type hints** | Optional annotations checked by tools such as mypy. Ignored by the interpreter unless libraries such as Pydantic use them. |
| **Mutable default argument** | A default such as `[]` is created once and shared across calls. A classic bug; use `None` and create inside. |
| **Dunder method** (Double Underscore) | Special method such as `__init__` or `__eq__`. Connects classes to language features and operators. |
| **MRO** (Method Resolution Order) | Order Python searches classes for an attribute, using C3 linearisation. Essential for understanding multiple inheritance and `super()`. |
| **WSGI and ASGI** (Web Server Gateway Interface, Asynchronous Server Gateway Interface) | Python web server standards. WSGI is synchronous; ASGI adds async, WebSockets and long-lived connections. |

## Java

| Term | What it means |
|---|---|
| **JVM** (Java Virtual Machine) | Runs Java bytecode, managing memory and threads. Provides portability, and runs other languages such as Kotlin. |
| **JDK and JRE** (Java Development Kit, Java Runtime Environment) | The JDK includes compiler and tools; the JRE is what runs programs. Modern deployments often build minimal runtimes. |
| **Bytecode** | Portable instructions produced by `javac`. The JVM interprets and JIT-compiles them for each platform. |
| **HotSpot JIT** (Just-In-Time) | Profiles running code, then compiles frequently used methods to optimised machine code. Explains JVM warm-up and later high performance. |
| **JVM heap** (Java Virtual Machine) | Shared memory holding objects, sized by `-Xms` and `-Xmx`. Exhaustion causes `OutOfMemoryError`. |
| **Young generation** | Heap area for new objects, split into Eden and survivor spaces. Collected frequently, because most objects die young. |
| **Old generation** | Heap area for long-lived, promoted objects. Collected less often, and pressure here often causes long pauses. |
| **Metaspace** | Native memory storing class metadata, replacing PermGen since Java 8. Classloader leaks can make it grow until failure. |
| **G1 GC** (Garbage-First Garbage Collector) | Region-based collector targeting a pause time goal. Default on most server configurations *(verify)*. |
| **ZGC** (Z Garbage Collector) | Mostly concurrent collector aiming for very short pauses on large heaps. Trades some throughput and memory for low latency. |
| **JMM** (Java Memory Model) | Rules for how threads see shared memory. Defines "happens-before", needed to write correct concurrent code. |
| **`volatile`** (Java) | Guarantees visibility and ordering of reads and writes across threads. Does not make compound actions such as `count++` atomic. |
| **`synchronized`** | Acquires an object's intrinsic lock around a block or method. Gives mutual exclusion and visibility guarantees. |
| **`ExecutorService`** | Manages a thread pool and runs submitted tasks. Bounding the queue and pool prevents resource exhaustion. |
| **Virtual threads** | Lightweight threads scheduled by the JVM, final in Java 21. Let blocking-style code scale to many concurrent tasks *(verify)*. |
| **Bounded wildcards** (PECS: Producer Extends, Consumer Super) | Use `? extends T` when reading and `? super T` when writing. Makes generic APIs flexible but safe. |
| **Checked exception** | Must be declared or caught, enforced at compile time. Makes failure explicit, though it can add boilerplate. |
| **`ConcurrentHashMap`** | Thread-safe map using fine-grained locking and atomic operations. Scales far better than wrapping a map in one lock. |
| **`equals` and `hashCode` contract** | Equal objects must have equal hash codes. Breaking this makes hash-based collections lose entries. |

## PHP

| Term | What it means |
|---|---|
| **Shared-nothing request model** | Each request starts with fresh state, with everything discarded afterwards. Limits leaks and simplifies scaling, but state must live elsewhere. |
| **PHP-FPM** (PHP: Hypertext Preprocessor FastCGI Process Manager) | Manages a pool of worker processes behind a web server. Each worker handles one request, so pool size caps concurrency. |
| **Zend Engine** | The core runtime that compiles PHP to opcodes and executes them. Underlies the performance features of modern PHP. |
| **OPcache** (Opcode Cache) | Stores compiled opcodes in shared memory, so scripts are not recompiled each request. Essential for production performance. |
| **PHP JIT** (PHP: Hypertext Preprocessor Just-In-Time) | Added in PHP 8.0, compiling to machine code. Helps CPU-heavy work more than typical database-bound web requests. |
| **Composer** | PHP's dependency manager, also generating class autoloaders. Lock files give reproducible installs. |
| **PSR** (PHP Standards Recommendation) | Community standards such as PSR-4 autoloading and PSR-12 style. Help frameworks and libraries interoperate. |
| **Strict types** | `declare(strict_types=1)` stops scalar type coercion for calls made in that file. Catches silent type conversion bugs. |
| **`==` vs `===`** | `==` converts types before comparing; `===` also requires identical types. Prefer `===` to avoid surprising matches. |
| **Trait** | Reusable group of methods included in classes with `use`. Shares behaviour around single inheritance. |
| **Fibers** | Interruptible functions added in PHP 8.1, used by async libraries. They provide cooperative concurrency, not parallel execution. |
| **PHP generics via annotations** (PHP: Hypertext Preprocessor) | PHP has no native generics, so PHPDoc annotations are checked by PHPStan or Psalm. Types are not enforced at runtime *(verify)*. |
| **PHP garbage collection** (PHP: Hypertext Preprocessor) | Reference counting with a cycle collector. Rarely a concern per request, but matters in long-running workers. |
| **Long-running PHP runtimes** (PHP: Hypertext Preprocessor) | Tools such as Swoole, RoadRunner and FrankenPHP keep applications in memory. Faster startup, but state can leak across requests *(verify)*. |
