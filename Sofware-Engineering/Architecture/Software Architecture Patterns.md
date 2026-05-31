# Software Architecture Patterns

> **Source:** [Software Architecture Patterns](https://blog.bytebytego.com/p/software-architecture-patterns)

Architectural patterns offer a systematic approach to addressing recurring design issues. They are reusable approaches to building software that capture core design structures of systems and software elements, allowing them to be reused across different projects and scenarios.

---

## 1. Client-Server Pattern

Client-server architecture is a widely used model for network communication. A **client** (user or application) sends requests to a **server**, and the server responds with the requested data or service. This can run on a single machine or across networked machines.

| Role | Responsibility |
|------|----------------|
| **Client** | Initiates communication; sends requests specifying desired data or services |
| **Server** | Listens for requests, processes them, and returns responses; manages resources and data |

---

## 2. Layered Architecture

Layered architecture breaks complex systems into distinct layers, each with a specific set of functionality. This organizes code and makes the system easier to maintain and modify.

### Presentation Layer
Displays information to the user and collects input (UI and user-facing components).

- **User Interface** — Buttons, forms, menus; look and feel
- **User Input Handling** — Captures and validates user input
- **Data Presentation** — Formats and presents data meaningfully (charts, graphs, etc.)

### Business Logic Layer
Implements the business rules of the application.

- **Business Rules Implementation** — Encapsulates domain rules and processes
- **Data Processing** — Calculations, algorithms, and transformations
- **Workflow Management** — Sequences steps and actions for tasks/processes

### Data Access Layer
Facilitates data persistence and retrieval; interacts with databases or external data sources.

- **Data Retrieval** — Queries and fetches data via APIs
- **Data Persistence** — Saves changes (transactions, concurrency, integrity)
- **Data Mapping** — Maps database records to application objects/entities

---

## 3. Pipes and Filters Pattern

The pipe and filter pattern processes data by separating tasks into independent components. It suits systems handling large volumes of data.

| Component | Role |
|-----------|------|
| **Data source** | Starting point; receives input and delivers it downstream (file, DB, external system) |
| **Pipe** | Connectors that transfer and buffer data between components |
| **Filter** | Self-contained processing units that transform data; independent and reusable |
| **Data sink** | Endpoint; receives processed output (file, DB, external system) |

---

## 4. Domain-Driven Design (DDD)

DDD is not a typical architectural pattern—it is a **design approach** that emphasizes understanding and modeling the domain (problem space). It prioritizes business logic and domain knowledge.

### Key Concepts

- **Bounded Contexts** — Distinct areas encapsulating a specific domain/subdomain, each with its own language, concepts, and rules; enables independent team work
- **Aggregates** — Clusters of related entities and value objects treated as a single unit; define consistency boundaries and enforce business rules
- **Domain Services** — Encapsulate domain logic that does not fit a single entity or value object; separate domain model from technical implementation

### Advantages

- **Alignment with business requirements** — Domain experts involved throughout; software reflects stakeholder needs
- **Improved communication** — Shared domain model and vocabulary between developers and domain experts
- **Modular, maintainable architecture** — Clear separation of concerns; easier to understand, modify, and extend

---

## 5. Monolithic Architecture

The application is built as a **single, cohesive unit**—all code and dependencies packaged together and typically deployed on one server.

### Characteristics

- **Single, self-contained unit** — Components, modules, and dependencies are tightly coupled
- **Unified deployment** — One deployment unit; simpler deployment process

### Advantages

- **Simplicity** — Fewer moving parts; simpler development, testing, and deployment
- **Ease of maintenance and debugging** — Everything in one place; easier to identify and fix issues
- **Straightforward development** — Build the application as a whole without coordinating many components

### Disadvantages

- **Limited scalability** — Performance bounded by single server; vertical scaling is costly
- **Difficulty adopting new technologies** — Tight coupling means updates can break the whole app
- **Tight coupling** — Changes in one area can have unintended effects elsewhere; brittle over time

---

## 6. Microservices Architecture

Applications are built as a **collection of small, independent services** that communicate over a network. Each service focuses on a specific business capability and can be developed, deployed, and scaled independently.

### Advantages

- **Improved scalability** — Scale services independently for traffic spikes
- **Increased flexibility** — Modify or add services without impacting others; faster feature delivery
- **Technology diversity** — Different tools/languages per service as needed

### Challenges

- **Service communication** — Discovery, protocols, and APIs at scale
- **Data management** — Per-service data stores increase complexity and synchronization needs
- **Distributed system complexity** — Latency, partial failures, distributed transactions

### Best Practices

1. **Design around business capabilities** — Use DDD bounded contexts for service boundaries
2. **Loose coupling, high cohesion** — Clear boundaries and well-defined interfaces
3. **Containerization** — e.g. Docker for packaging and deploying each service
4. **Monitoring and management** — Detect and resolve issues quickly
5. **CI/CD** — Automate testing and deployment
6. **Resilience patterns** — Circuit breakers, retries, timeouts, fallbacks
7. **12-Factor App** — Scalable, maintainable microservices practices

---

## 7. Event-Driven Architecture (EDA)

Components communicate through **events** rather than direct request-response. Enables fast, efficient communication across services.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Events** | Notifications of something of interest (user actions, data updates, errors) |
| **Event sources (producers)** | Generate and publish events to a broker/bus |
| **Event consumers (subscribers)** | Subscribe to events and react with actions or further processing |
| **Event broker (event bus)** | Central hub routing events from producers to consumers; decouples them |

### Advantages

- **Decoupling** — Components depend on events, not each other directly
- **Scalability** — Parallel processing; workload distributed across consumers
- **Asynchronous processing** — Non-blocking; better resource utilization

### Challenges

- **Complexity management** — Harder tracing/debugging; needs monitoring, logging, tracing
- **Event ordering** — Async processing risks out-of-order handling; may need sequencing, timestamps, or event sourcing
- **Error handling and recovery** — Failures may be delayed; replays or compensating actions needed
- **Event schema evolution** — Versioning, backward compatibility, and migration strategies

---

## 8. Stream-Based Architecture

Focuses on **continuous processing of data** as it flows through the system. Data streams are sequences of records (events/messages) produced, transmitted, and consumed in real time or near real time.

### Key Principles

- **Event-driven processing** — Process data as generated, not in batches; minimal latency
- **Continuous data flow** — Each record processed on arrival; supports real-time analytics and decisions

### Advantages

- **Real-time processing** — Immediate insights (fraud detection, recommendations, IoT)
- **Scalability and elasticity**
  - *Scalability* — Handle increased load by adding resources (often manual)
  - *Elasticity* — Resources auto-scale up/down with demand
- **Flexibility and adaptability** — Easy integration of new sources; logic can change without disrupting the system
- **Fault tolerance and resilience** — Replication, checkpointing, exactly-once guarantees; recovery from crashes or network issues

---

## 9. Serverless Architecture

A cloud execution model that **abstracts server management**. Developers focus on code and deploy individual functions; the provider manages allocation and provisioning.

### Key Concepts

- **Event-driven** — Functions triggered by HTTP requests, DB changes, file uploads, scheduled tasks, etc.
- **Auto-scaling** — Instances adjust automatically from few requests to thousands per second
- **Pay-per-use** — Billing on actual execution time/resources, not pre-allocated servers

---

## Summary

| Pattern | One-line description |
|---------|----------------------|
| **Client-Server** | Client requests; server responds with data or services |
| **Layered** | Distinct layers (presentation, business logic, data access) with specific responsibilities |
| **Pipes and Filters** | Independent components linked by pipes; data flows source → filters → sink |
| **DDD** | Deep domain understanding drives design; bounded contexts, aggregates, domain services |
| **Monolithic** | Single cohesive deployable unit |
| **Microservices** | Small independent services communicating over a network |
| **Event-Driven** | Components communicate via events through a broker/bus |
| **Stream-Based** | Continuous real-time processing of flowing data |
| **Serverless** | Event-triggered functions; provider manages infrastructure; pay per use |
