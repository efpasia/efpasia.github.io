---
layout: post
title: "Not All Code Architectures Survive the Agent"
date: 2026-04-05 23:00:00 +0800
categories: thoughts
---

Most of the debate around agentic coding is about tooling – which IDE, which model, which CLI to use, and so on. Much less attention goes to a more interesting question: *does the way we’ve been taught to structure code even make sense* when the thing writing it has limited long-term memory, a finite context window, and often has to make progress without holding the whole system in view?

This is an analysis of different code architectures – TDD, OOP, FP, MVC, MVVM, microservices, event-driven, CQRS, hexagonal, BDD, DDD – sorted by how well they actually work when an agent is doing the coding.

---

## Four Properties That Actually Matter

Before ranking anything, we need to define what **agent-friendly** even means. After enough sessions working with agents and thinking about this, I’ve landed on four properties that explain most outcomes.

### Can the agent reason about a piece of code locally?

A pure function that takes inputs and returns outputs can be verified by looking at it. An event handler that depends on twelve other subscribers firing in the correct order requires understanding the whole system. Current agents generally perform much better at **local reasoning** and degrade under **global reasoning**, because the context window is finite and everything outside it might as well not exist.

### Can the agent verify its own work mechanically?

TDD gives you a binary green/red signal. OOP done "well" requires the kind of taste that comes from years of reading and writing code – when to inherit vs compose, how deep to go with the hierarchy, what level of abstraction is appropriate, and so on. Agents don’t reliably exercise that kind of design judgment because they have pattern-matching across training data, and the training data is full of terrible OOP.

### Is control flow explicit or implicit?

MVC is explicit – a request hits a route, controller calls a service, service returns data. Event-driven is implicit – an event is published, and somewhere else something handles it, and the only way to know what that something is is to search every file for subscribers. Agents can follow explicit flow like a recipe. Implicit flow tends to degrade agent performance for the same reason it frustrates junior developers: too much important behavior is off-screen.

### What’s the ratio of actual logic to boilerplate?

A functional pipeline is almost pure signal. A full DDD implementation with aggregates, value objects, domain events, repositories, and bounded contexts can be 80% ceremony before you’ve written a line of meaningful logic. High ceremony raises the cost of getting changes right under limited context – every scaffolding line is a line where the agent can introduce a subtle bug, and a line that eats context window for no value.

Every paradigm scores differently on these four, and the ones that score well across all four are the ones that make agents genuinely good.

---

## S-Tier: Best of Them All

### TDD

This is the one. If you take nothing else from this post: **use TDD with your coding agents.**

An agent’s core weakness is epistemic – it generates code that looks correct but has no mechanism to know it’s actually correct. TDD converts the problem from random data generation of the most common outcomes to an iterative process. Write a test (which is just restating the spec in executable form), run it, see red, write the smallest implementation that passes, see green. The agent gets a specific mechanical signal at every step.

The only problem with this approach is the difficulty of zero-shot doing large problems; in these cases, agents tend to "hack" the tests. This is a strong signal of either a lack of context or the task’s high complexity, suggesting the agent, for some reason, can’t complete it.

But the real killer feature is that **the test suite is externalized memory**. An agent has zero recall of what it built three sessions ago. The tests from those sessions still exist and still run. Every new change runs against every existing test. The agent gets automatic regression detection for code it has no memory of writing.

TDD also creates something I’d call **confident composability**. The agent can build function B on top of function A without re-reading A’s implementation, because A’s tests guarantee its contract. As long as those tests pass, A works. The agent only needs to know A’s interface, not its internals. This property scales – the bigger the project gets, the more TDD helps, which is the opposite of most paradigms for agents.

Local reasoning ✓, mechanical verification ✓, explicit flow ✓, high signal ratio ✓. Four for four.

### Functional Programming

Pure functions have a property that’s almost uniquely valuable for agents: **local correctness becomes much more meaningful when there’s no hidden state.** If `transformOrder(order)` returns the right output for the right inputs, you’re done with that function. There’s no shared variable that some other function mutated underneath you, no spooky action or trigger at a distance. The agent doesn’t need to hold the entire codebase in context to verify one piece of it.

Immutability eliminates an entire class of bugs that agents are particularly bad at catching – like when function A modifies a shared variable, and function B reads it later assuming it’s unchanged. These bugs are invisible in the code of both A and B individually. You can only see them by understanding the system globally, which agents often struggle to do.

`parse → validate → transform → save` is also readable for agents. Each step is independently testable. The agent can work one function at a time, test it, and move on with confidence that nothing it did earlier will break.

The practical caveat: purely functional style doesn’t map onto every problem domain. Obviously, you need IO, you need state, you need side effects somewhere. The trick is to push FP as far as it goes – pure domain logic, immutable data transforms, composition pipelines – and contain the impure parts at the edges.

### BDD

> "Given a user with an expired subscription, When they try to access premium content, Then they should see an upgrade prompt."

That’s a prompt. *That is a prompt.* The translation from Gherkin to implementation is almost mechanical, and it’s exactly the kind of structured natural language → code task that LLMs were built for.

But BDD does something more important: it pushes the spec to be unambiguous before coding begins. The biggest source of agent errors is underspecified requirements and a lack of context. When the agent has to decide "what should happen when the user submits an empty form?" on its own, it guesses. BDD reduces this class of problems by requiring concrete scenarios.

The combination of **BDD specs and TDD execution** is the highest-leverage setup for agentic coding on new projects I’ve found.

---

## A-Tier: Strong Fits with Minor Friction

### Hexagonal Architecture (Ports & Adapters)

The core rule: **all dependencies flow inward** through defined interfaces. Domain logic has zero knowledge of databases, HTTP, or file systems. This means the agent can write the domain layer as pure functions – S-tier territory – and then wire up adapters independently.

Each adapter is a self-contained unit. Writing the PostgreSQL adapter requires knowing the port interface and knowing PostgreSQL. It doesn’t require understanding the HTTP adapter, the domain logic, or any other adapter. The agent’s context window only needs to hold the port interface and the specific technology it’s adapting to.

The friction keeping it from S-tier: the agent needs to understand the architecture upfront. If you don’t tell it "we’re using hexagonal architecture, the domain logic is in `/core`, ports are interfaces in `/ports`, adapters implement those interfaces in `/adapters`," it will cheerfully mix database queries into your domain logic. A good `CLAUDE.md` or system prompt makes this much easier to enforce.

### MVC

MVC becomes A-tier not due to its theoretical excellence, but rather due to the **massive volume of available training data**. All tutorials for frameworks, all answers to questions posted at Stack Overflow, and all bootcamp projects use MVC. As such, AI agents have been exposed to more MVC code than any human would be able to read in their lifetime.

The MVC pattern is mechanical and predictable enough that AI agents are reliable in following this pattern. The route exists somewhere within the structure (i.e., wherever you tell them), the controller will call the service, the service will interact with the model, and finally the view will render the response back to the user.

While MVC has less of a ceiling **than** Hexagonal Architecture – as MVC does allow business logic to reside within controllers when it should exist elsewhere – agents generally do a better job at navigating MVC compared to other patterns that may include an abstraction layer or more layers of indirection. Agents understand where things are. While the code generated by this process is rarely beautiful, it works well and is easily debugged. Any developer on your team will also be able to understand how to navigate this code without needing to reference information that was provided during a previous session.

As a result, if speed is most important for greenfield projects, using an agent to implement MVC is one of the best bets possible.

---

## B-Tier: Useful But With Real Trade-offs

### CQRS

CQRS has a clean conceptual separation: reads and writes use different models. The agent can follow this mechanically – command handlers go here, query handlers go here, never mix them.

The problem is that for a new project built zero-shot, CQRS is almost always premature. You’re doubling your surface area – two models, two pathways, synchronization between them – for benefits that often materialize under load patterns most new projects won’t see for months. An agent will faithfully implement the full CQRS machinery if you ask for it. It’ll produce a technically correct implementation that’s far more complex than what the project needs at that stage, eating context window and creating more surface area for bugs.

Good pattern. Rarely the right default for greenfield agentic development.

### OOP

Simple OOP – data classes, shallow composition, encapsulation – is perfectly fine for agents. A `User` class with properties and a couple of methods is trivially correct.

The problem is that **OOP done well requires design judgment**, and that’s the one thing agents don’t reliably exercise. When do you inherit vs compose? How deep should the hierarchy go? Is a `UserService` the right abstraction, or should it be `AuthenticationService` and `ProfileService`? These are judgment calls where the right answer depends on context that spans the entire codebase and into the future, requiring extensive human thought.

In my experience, agents tend toward over-inheritance, god objects, and multi-layer hierarchies. When you tell an agent to "use OOP best practices," what you often get back reflects the average OOP code in training data – and a lot of that code is bad.

The solution is exactly the same as for MVC: **constrain explicitly.** "Prefer composition over inheritance; maximum of one level of abstraction in the class hierarchy; no abstract factories." Without these guardrails, agents will default to sophisticated-looking, fragile patterns.

---

## C-Tier: Actively Fights Agent Strengths

### MVVM

MVVM adds two issues for agents that struggle with: **reactive data binding** and **implicit state updates**. When a ViewModel’s properties change, they magically trigger an update in the view. However, which binding triggered the update? How did the order of the bindings matter? Can there be a cycle of updates from two or more bindings?

These bugs are ones that have no apparent presence in your code until you interact with the UI at run time to use it in some manner. A developer may write MVVM code using an agent model to create a design that appears perfect (the ViewModel is clean, all of the binding statements appear to be valid, and so on) and then discover that the application will crash due to a subtle ordering issue in how reactive event handlers subscribe.

The root problem is **implicit control flow**. The agent can’t read a ViewModel and know what will happen when a property changes without understanding every binding in every View that references that property. That’s global reasoning, and it’s where agents consistently struggle.

### Event-Driven Architecture

Similar issue to MVVM, but at a larger scope. When an application publishes an event, you don’t know how many subscribers are listening (or even what type) based solely on the event-publishing code. To debug an event-driven system you have to be aware of all subscribers in the entire system. An agent writes a publisher and subscriber independently and correctly, yet they will fail due to events arriving out of sequence, or one handler making assumptions about state that other handlers haven’t yet established.

The debugging required for asynchronous, event-based distributed systems is equivalent to maintaining the entire event topology in memory; this is the exact opposite of the global understanding of complex logic that degrades as software becomes more complex, as developers move outside their immediate context.

Of course, if an agent has already built or is working on specific event handlers for existing systems – these are examples of using local reasoning about a bounded set of code. However, the greatest risk exists during initial design of a new event-driven system where an agent must create both the event topology and determine the appropriate ordering guarantees for those events concurrently.

---

## D-Tier: Fundamentally Misaligned With Zero-Shot Agentic Coding

### DDD

DDD is as much a **collaborative domain-modeling process** as it is a set of code-level patterns. The entire point is building a "ubiquitous language" between developers and domain experts through iterative conversation – event storming, bounded context mapping, aggregate root identification. These are inherently human activities that require deep domain knowledge and extended collaboration.

An AI agent has no domain expert to talk to. It doesn’t understand that "Account" means something different in the billing bounded context vs. the user identity bounded context. Without deep domain knowledge fed in through exhaustive prompting, agents applying DDD tend to produce superficial models – aggregates and value objects with the right naming conventions but wrong boundaries. That can be more harmful than no DDD at all, because it creates a false sense of rigor. The code looks like it was designed by someone who understood the domain.

If you have the domain knowledge and can feed it into the agent through a detailed spec – essentially doing the DDD work yourself and handing the agent the conclusions – it can implement the tactical patterns fine. But at that point, you’ve done the hard part. The agent’s work is purely mechanical.

The ceremony cost is brutal too. A full DDD implementation produces an enormous amount of boilerplate relative to actual business logic. Every line of boilerplate is a line eating context window and a line where the agent can introduce subtle structural errors.

### Microservices

Starting with microservices is often a poor default even for human teams, and it’s particularly costly for agents.

When you tell an agent to build a microservices architecture from scratch, here’s what it produces: multiple separate services, each needing their own deployment configuration, a service discovery mechanism, inter-service communication (HTTP? gRPC? message queues? – the agent will pick one and maybe mix in another), distributed tracing, eventual consistency handling, and an infrastructure layer that dwarfs the actual business logic.

The agent has no way to assess whether that complexity is warranted. The agent follows your instructions and creates exactly what you told it to create. As a result, you will be creating a system in which an error in any individual service can require information from each of all of the remaining services – and the amount of that information will exceed the size of a single agent session. In other words, debugging the system will become an archaeological process that involves multiple sessions, during which time there is nothing about what each of the other services does that is remembered by the agent.

Independent development and deployment (scaling) are the underlying principles of microservices. These principles are intended to allow development teams to work independently and at their own pace; they also provide fault isolation and enable teams to deploy new versions of applications independently of each other. Your new application has zero users; therefore none of these principles have been proven or demonstrated. Therefore, you are experiencing the full overhead of distributed systems on day one of operation – and you may never realize the potential benefits of using microservices.

---

These scores are heuristic judgments based on the four axes, not empirical benchmark results.

| Paradigm | Local Reasoning | Mechanical Verification | Explicit Flow | Signal Ratio | Grade |
|---|---|---|---|---|---|
| TDD | ★★★★★ | ★★★★★ | ★★★★★ | ★★★★★ | **S** |
| Functional Programming | ★★★★★ | ★★★★☆ | ★★★★★ | ★★★★★ | **S** |
| BDD | ★★★★★ | ★★★★★ | ★★★★☆ | ★★★★☆ | **S** |
| Hexagonal / Ports | ★★★★★ | ★★★★☆ | ★★★★★ | ★★★☆☆ | **A** |
| MVC | ★★★★☆ | ★★★☆☆ | ★★★★★ | ★★★★☆ | **A** |
| CQRS | ★★★★☆ | ★★★★☆ | ★★★★☆ | ★★☆☆☆ | **B** |
| OOP | ★★★☆☆ | ★★☆☆☆ | ★★★★☆ | ★★★☆☆ | **B** |
| MVVM | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ | ★★★☆☆ | **C** |
| Event-Driven | ★★☆☆☆ | ★★☆☆☆ | ★☆☆☆☆ | ★★★☆☆ | **C** |
| DDD | ★★★☆☆ | ★★☆☆☆ | ★★★☆☆ | ★☆☆☆☆ | **D** |
| Microservices | ★☆☆☆☆ | ★★★☆☆ | ★★☆☆☆ | ★☆☆☆☆ | **D** |

---

## The Ideal Agentic Stack

If I had a project today that was going to rely mostly on coding agents:

**Core functional programming with TDD.** Pure functions for domain logic and exhaustively tested. Each business rule is a function (with input parameters, output values) that can be written by the agent continuously with no problems in terms of testing.

**Behavior-driven development (BDD)** spec definitions at the periphery. Business-facing user behaviors are defined as Gherkin-based specifications before the agent writes specific implementation code. This helps reduce ambiguity of requirements and provides a clear objective target for the agent to implement from.

**A hexagonal architecture for structural design.** Logic is located in the center of the hexagon; ports define interface boundaries; adapters connect external integration implementations to those interfaces. The agent can continue to work with an individual adapter at a time and does not need to understand how other adapters interact.

**MVC or simple request/response for edge layers.** For HTTP layers and all related wiring patterns – use what the agent sees ten million times. Do not get creative here!

**Monolithic deployment.** A single repository, a single deployable package, a single environment/context that the agent understands. Deploy services later when data and need exist.

This configuration achieves the benefits of each layer while avoiding its drawbacks: best possible local reasoning, mechanical verification at every step, most explicit control flow, least amount of ceremony.

---

## The Takeaway

The pattern here isn’t complicated: paradigms that maximize **local reasoning** and **mechanical verifiability** tend to be agent-friendly; paradigms that require global context, design taste, or domain expertise tend to be agent-hostile.

While I am not opposed to Domain Driven Design (DDD), Event-Driven Architecture (EDA), and/or microservices as a whole – these are all good patterns employed by experienced teams working with large and complex systems – the reason they’re effective is that skilled people provide the necessary design judgment, domain knowledge, and global context required to support their use. At present, agents cannot provide those skills reliably. If you ask an agent to follow a paradigm based upon assumptions that it has those skills (judgment, knowledge, and so on), then you’ll be setting both your agent and yourself up for debugging.

You will also see that nothing above eliminates the necessity for human review. While architecture can limit the ways in which an agent fails – thus making errors less frequent and easier to identify – it cannot replace the thoroughness associated with reviews, observability, and validation. The best possible agent-friendly architecture in the world still requires a human to review its output before shipping.

An important aspect of the "agent-coding revolution" is that it is not about how much smarter the model becomes (it will become smarter), but rather about how software developers learn to decide which architectures enhance an agent’s capability and which create conflict. Optimize your architecture for the areas where an agent excels – i.e., local reasoning, mechanical verification, explicit control flow – and allow the agent to perform the tasks it performs best – i.e., generate vast amounts of correct code quickly while you remain focused on the decisions that truly require a human.

Ultimately, this is a significant shift in how one views themselves as a developer – and not merely "how do I better prompt my models?" – but instead "How do I structure my projects so that I’m minimizing opportunities for the agent to mess up?"
