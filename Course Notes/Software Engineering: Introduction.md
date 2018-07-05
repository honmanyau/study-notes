# Software Engineering: Introduction

## Table of Contents

* [How to Code: Complex Data](#how-to-code-complex-data)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [Software Engineering: Introduction](https://www.edx.org/course/software-engineering-introduction-ubcx-softeng1x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### Introduction

#### Software Engineering: Introduction

* A programming language is a means for a software engineering to formalise her/his ideas in a format that a computer can understand
* There is an error in the example given regarding syntax vs. semantics:

  ```
  foo == foo // "Always true"
  ```

  Given that TypeScript is used in this course, this can't be further from the truth. Consider this:

  ```
  let foo = NaN;

  foo == foo // false
  ```
* Immutability becomes more important in multi-threaded environments

#### Introduction to Concurrency and Asynchronous Development: Part 1

* Concurrency and asynchrony are two techniques for increasing the perceived performance of modern applications
* Fweee, nice analogy for concurrency (ferris wheel vs. roller coaster)

#### Introduction to Concurrency and Asynchronous Development: Part 2

* Four main parts to JavaScript w.r.t. async:
    * Call stack—LIFO structure for handling when methods called
    * Web APIs (or equivalent in Node.js)—async code doesn't get run on the call stack, it gets run in this separate environment. The callback passed to the Web API is placed back onto the callback queue once it's done with certain task
    * Event loop—check whether or not the call stack is empty, when it is, check the callback queue for a new task (if not empty) that can be placed onto the call stack to execute
    * Callback queue—similar to call stack but a FIFO structure

#### Introduction to Concurrency and Asynchronous Development: Part 3

### Process

#### Why Process?

A structure for the diverse set of activities involved in build modern softwares:

* What we are building?
* Who we are building for?
* How are the teams going to coordinate with each other and get things done?
* When things need to be done?

Typically, a process involves the following stakeholders (and for better or worse, everyone has their own idea of how things should be built!):

* Developers—building the products
* QA—validating the products
* DevOps—make sure that product can be deployed, maintained and works in practice
* Managers
* Users
* Support

Documentation:

* Specification
* Development
* Validation
* Deployment/Evolution

#### Waterfall

Stepwise, distinct process with explicit hand-off from one stage to another. Never going back (in practice that's difficult) to the previous step:

1. Requirement
2. Design
3. Implementation—writing code according to design documents
4. Verification
5. Maintenance

Downside—one would not know if a system is useful until the very end of the process.

#### Spiral

Address some of the shortcomings in the waterfall model by iterating faster.

1. Planning—figuring out requirements
2. Risk analysis—what can potentially go wrong?
3. Engineering—traditional software development
4. Validation—not just QA, checking prototype with customers as well to see if the software being built is actually in line with what the customer want

Iterate by spiralling out of the graph in cycle. The most important and core parts are built first, and additional functions can be incrementally implemented if required.

Primary shortcomings—effectively documenting risk analysis non-trivial; waiting for the whole cycle to complete, before feedbacks, which could be a longer than ideal.

#### Extreme Programming

Agile methodologies optimises the feedback loop between customers and development team. Allows for much higher flexibility and development velocity than traditional methods.

The more important points from the Agile Manifesto:

* Favour individual interactions over processes and tools—valuing stakeholder inputs
* Value working software over extensive documentation
* Favour customer interaction over contract negotiation
* Favour agility over planning so that customer requirements can also be more accurately addressed

Avoids building the wrong things and allows development teams to be more flexible and experiment.

#### XP

One of the first influential agile development methodologies that focuses on having buildable system at all times. It follows five key principles:

1. Communication—stakeholders talking to each other
2. Simplicity—build the baseline to see if it addresses customer requirements first and iteratively improve from there
3. Feedback—both ways between development and customers
4. Courage—give developers the freedom to experiment and learn more about the system and make it better, even if the outcome isn't implemented
5. Respect—everyone in the process is important

#### TDD

Emerged from agile movement as expectation of quality became more and more important. With emphasis on automated tests—the system should be buildable any any time and all tests should be satisified.

And... WRITE TESTS FIRST.

#### Scrum

Product backlog that represents all issues (features/bugs). Sprint that's usually 1-3 weeks long containing only selected features or bugs from the backlog.

Don't have specific engineering practices, teams figure out what works best.

Usually three high-level stakeholder roles:

* Product owner—acts as proxy for clients or clients themselves, evaluate team output matches expectations
* Scrum master—manages the process (not necessarily the team)
* Team—typically multi-discipline, 5-8 people

1. Figure out what needs to chosen, plan and make sure that it's achievable, in conjunction with product owner, then turn into a sprint backlog
2. Standup meeting. Everyone goes over their progress **every day**, what they are working on and what's stopping them. Scrum master makes sure that everyone is delivering what they're supposed to and figure out how problems can be resolved
  * What did you do yesterday?
  * What will you do today?
  * What obstacles are in your way?
3. Ceremony—demo to product owners and feedback occurs here. Reflection
4. Plan the next sprint

### Specification

A collection of documents that is an abstraction of what the system is intended to be like at the end and one of the most important documentation for connecting the customer and the engineering team.

#### Requirements

* Functional—what should the system do
* Non-functional—properties a system should have. Importance varies depending on the stakeholder in question but important for all of them to be involved as implement one thing usually comes at the expense of another. Some examples:
  * Usability
  * Performance
  * Scalability
  * Reliability
  * Complexity
  * Testability

#### Requirements Properties

* Complete
* Consistent
* Precise
* Concise

#### Requirements Elicitation

Formalising requirement is iterative. Requirements elicitation is the first step.

1. Elicitation—figuring out what should be built
2. Analysis—looking at requirements gathered and making sure that they are consistency, make sure there are not concerns from both development team and customer
3. Reification—formalising requirements as documents that will be used during the development process
4. Validation—make sure that things match customer expectations. Iterate from Step 1 if not.

#### Additional Requirements

* Design constraints: constraints inside an organisation that may influence how software is built; e.g. budget, regulatory requirements, internal processes... etc.
* Environmental constraints: software isn't built in isolation and needs to work with external resources
* Preferences: what the customer thinks is more important than others

#### Validating Requirements

* A way to evaluate features added are implemented and function correctly
* Make sure that everyone is on the same page
* Could be a flag for additional time spent in the analysis and reification stage

What can be measured and how they can be measured? These need to be explicit, quantifiable, and actionable.

For example: "should be performant"

* Time (how long it takes things to happen)
  * Throughput: the number of customer/transactions that can be handled
    * Peak
    * Off-peak
  * Response. For example, should respond within 150 ms
* Space
  * Memory. For example: should never use more than 1GB of disk space
  * Disk For example: should never use more than 100 MB of memory

#### User Stories

* Who are features for
* What are the values
* What's involved in creating the feature
* Costs—must be implementable within a single iteration
* How long it takes to build

Five main parts:

1. Role Goal Benefit (role can be non-human entities, too). Typical example: As a <Role> I want <Goal> so that <Benefit / Expected outcome> (reference: [Agile Digest, User Story](https://agiledigest.com/agile-digest-tutorial/user-story/))
2. Limitations
3. Definition of Done
4. Engineering Tasks (how the features interacts with the system or subssytems???)
5. Effort Estimate

More Clarification on engineering notes from the quiz:

> Engineering notes should discuss at a technical level any notes to bear in mind or steps to take; they should not describe desirable traits or make estimates on time.

#### INVEST Guidelines

Mnemonic for writing effective user stories.

* Independence—user stories should be independent from each other as much as possible; not least because so that they are compatible with a product backlog as much as possible and so that the right set of user stories can be selected for the next iteration
* Negotiation—extra detail may be added through negotiation by considering that they are WIP
* Valuable—make sure that a user story is providing meaningful value, as things may change after each iteration
* Estimable—should provide a concrete, meaningful cost
* Small—easier to reason about, negotiate, estimate costs for, independent, develop
* Testable—makes sure that things are testable so that everyone can evaluate and agree to what is done when done

#### User Story and INVEST Example

Example:

1. RGB: As a **prof** I want to **create repositories** so that **students can work**
2. Limitations: need repository names, student IDs
3. DoD: runnable as a single command; automated test cases—programatically verifiable
4. Engineering Notes: This is an existing system, so should use GitHub manager
5. Cost: 1.5 units

#### Decomposing User Stories: Part 1

* Role: as a player
* Goal: move Mario
* Benefit: Dodge/attack enemies
* Limitations: keyboard input
* Engineering notes:
* Notes: use existing `Level` class
* Cost: +1 unit for key control; +1 unit for movement handling; +1 unit for collision

* DoD: key input controls Mario's movements; Mario should be able to move through the level; when Mario ~~attacks/hits~~ collides, enemy should be ~~hurt~~ hit

**INVEST**

* Independent? No.
* Negotiable? Yes. There is a lot of flexibility in how things can be implemented.
* Valuable? Yes. Otherwise game cannot be played
* Estimable? It is estimable, but it can be reasoned about.
* Small? Nooooooope.
* Testable? Yes.

#### Decomposing User Stories: Part 2

* Not a linear process—iterative.
* Methodology varies based on team.

To decompose a user story:

1. Find entities
2. Link entities
3. Bind actions (to entities)
4. Prototype (make sure that it matches the story that will unfold)
5. Formalise (writing down designs in a formal location)
6. Implementation

Example from DoD above:

* Entities: key, Mario, Level, enemies

Is anything missing? Figures

* Sketch entities out and link them with a graph

* Binding actions—usually verbs in the user stories, for example, "controls", "move", "collisions", "hits", and bind them to the graph

#### Decomposing User Stories: Part 3-5

Much of part 3-5 are explained with diagrams and it's better to refer to the videos instead.

### Why Test?

1. **Choose** what needs to be tested
2. **Create** tests
3. **Execute** tests
4. **Examine** results
5. **Evaluate** the testing process and ask whether or not things have been tested enough for the feature to be confidently shipped

Aspects of testing:

* White box testing
* Black box testing
* Assertions
* Testability

#### Terminology

* Test case
* Test suite
* CUT (Code Under Test)/SUT (System under Test)

* White box—designing tests by examining the internals of the code and how they work
* Black box—according to specification, usually involves **not** looking at how things work

#### Properties of Tests

* Fast
* Reliable—failure corresponds to defects in the system
* Isolate—a failure should quickly indicate what part of the code is the problem
* Simulate users

#### Kinds of Tests

* Fast/Slow
* Isolated/Non-isolated

* Unit test (fast, isolated)—checking a single, small part of the code to see if something is wrong
* Acceptance test (Slow, Isolated, usually manual)—performed by the person receiving a product to see whether or not things are working according to specification
* Smoke/Integration test (fast ish, less isolated than unit tests but much more isolated than integration tests)—check whether or not a collection of smaller features is behaving as intended
* System test—test a broader part of the system (usually integration tested) and usually with synthetic data

#### Red Green Refactor

* Make sure that a test fails before implementation
* Fail -> Pass -> Refactor + Extend

#### Introduction to Coverage

Two broad categories of coverage.

1. Flow independent
  * Cheap to compute
  * Easy tor reason about
  * Makes sure that each piece can be executed successfully
  * Block coverage
  * Line coverage
  * Statement coverage (similar to line coverage, but consider `for (let i = 0; i < 10; i++)`)

2. Flow-dependent
  * Making sure that different pieces of the code can execute together
  * Harder to reason about
  * More expensive to compute
  * But "more insights on how stringently we have executed our system"???
  * Branch coverage
  * Path coverage
  * MCC coverage

Coverage-guided testing is iterative:
1. Identify
2. Design
3. Write and execute
4. Repeat

> Coverage does let us know if any exceptions are raised during execution, but cannot tell us that our program actually did the right thing. A method may do the wrong thing (e.g. return the wrong value) but would achieve the same level of code coverage that a correctly-implemented method would.

#### Coverage Wrap-Up

* Coverage is **not** about correctness of behaviours.
* Writing high-quality tests is more important than writing low-quality tests simply with the goal of higher coverage in mind

#### Equivalence Class Partitioning

Less data available for analysis in Black Box Testing, only according to specification. It's more appropriate for other stakeholders to write who are not the original developers.

* Focuses on **what** the system is doing, not **how**

#### Input Partitioning

#### Output Partitioning

#### Boundary Value Analysis

#### Assertions

Assertion allows ones to reason about whether or not **execution** is correct.

Four Phase Test:

1. Set up test spectra
2. Run tests
3. A series of assertions that check the behaviour
4. After after/after each method for cleaning up test fixture

Given When Then:

* Strong emphasis on readability

1. Given a state of a program
2. Apply some test behaviours
3. Assert that the behaviour is correct

#### Introduction to Testability

1. Reach
2. Trigger
3. Propagate
4. Observe
5. Interpret

#### Controllability

#### Observability

#### Isolateability

Umm... seems to be a CS spelling. Isolability...? Also, why isn't observability spelt "obesrveability" instead, too. The same goes for "automatability" which comes later. Urgh, OCD triggers.

#### Automatability

#### Testability Wrap-Up

When tests fail, it is important to consider whether or not the defects are actually in the tests.

### High Level Design

#### Abstraction

#### Structured Programming

#### Decomposition

* Decomposition—taking a highly level description and successively add more and more details to it; however, leaf nodes usually end up conflicting with each other and may not match the best design in practice
* Bottom up—the reverse of top-down, inconsistencies may still occur for the opposite reason

#### Encapsulation

#### Information Hiding

#### Views

* Structural—class diagrams, deployment diagrams
* Behaviour—sequence diagrams, state machine diagrams

#### Deployment Diagrams

"Overlay class diagrams to add additional information about where modules and classes will exist at runtime", useful in distributed and cloud-based applications.

#### State Machine Diagrams

Illustrate the core states that a system could be in and how it transitions from one state to another.

#### High Level API Design

Balance between utility and technical debt.

* An API method should do one thing and do it well
* Should never expose internal implementation details
* As small as possible
* Usability

#### Low Level API Design

* Avoid long list of parameters
* Return descriptive objects
* Avoid exceptional returns
* Handle exceptional circumstances. Example:
  ```JavaScript
  throw new Error({ code, message, info });
  ```
* Favour immutability
* Favour private classes, fields and methods

#### API Design Process

* What is the goal?
  * Languages
  * Protocols
  * Platform
  * Formats
* Who is the customer?
  * Versioning
  * Licensing
  * Authentication

#### API Usability

* Visibility
* Model
* Mapping
* Feedback

#### REST Development: Part 3

> Statelessness is a core architectural constraint for all REST based systems.

> ... it makes REST based servers much easier to scale, and enables caching and server switching in a way that wouldn't otherwise.


#### Coupling: Part 1

How strongly connected program elements are to one another.

#### Coupling: Part 2

Data coupling -> stamp coupling -> stamp coupling -> global coupling -> content coupling

#### Cohesion: Part 2

The degree to which members of a class serve a unifying task or concept.

Functional cohesion -> sequential cohesion -> communication cohesion -> procedural cohesion -> temporal cohesion -> logical cohesion -> coincidental cohesion

#### Design Guidance and Symptoms

* Rigidity
* Fragility
* Immobility
* Viscosity
* Needless complexity
* Repetition
* Opacity

#### Single Responsibility Principle

A software module should do one thing and do it well.

Design patterns that help:

* Strategy patter—small, independent algorithms
* Command pattern—abstract actions an object takes from its internal Implementation
* State pattern—abstract away how an object behaves in different states that can change dynamically at runtime

#### Open/Closed Principle

System should be open to extension but closed to modification.

#### Liskov Substitution Principle

Any object in a program should be interchangeable with any other that has the same parent type.

#### Interface Segregation Principle

Clients should not be forced to depend on interfaces that they do not use.

#### Dependency Inversion Principle

Classes should depend on abstractions, not on implementation.

### Low Level Design

#### Introduction to Low Level Design

* Encapsulate what varies
* Design to interfaces
* Favour composition over inheritance

#### Design Patterns

A set of guidelines, based on problems that others have encountered before, that helps software engineers to design better systems by avoiding evolutionary problems as much as possible. They also function as a shared vocabulary between developers.

* Creational—helps to build objects in an extensible way
* Structural—helps structure systems in a way to avoid evolutionary problems
* Behavioural—helps to add new behaviours at runtime

#### Singleton

* Creational pattern
* Simplest design pattern

* Constructor is private
* Accessed through a public method `getInstance()`

* Singleton is effectively just a global variable
* They rely on a static `getInstance()` method, its make a system harder to evolve
* They spread dependencies to singleton classes throughout the system
* Singleton makes testing harder

#### Strategy

Provides a mechanism for encapsulating algorithms to allow for facile future extension.

* Classic example of O in SOLID
* Using polymorphism to hide concrete types behind more abstract interfaces

#### State

Abstracts away the need for state management inside a client. (Like Redux?)

* Often helpful to state with a state diagram
* Flexible way to manage state transition
* Localises state decisions within individual state classes

#### Facade

Simplifies client access to subsystems by reducing their apparent complexity through providing high-level support for tasks that depend on the subsystems.

* It is important that a facade does not block direct access to the subsystems
* Encourages weak coupling between clients and subsystems
* Violate the single responsibility principle as a trade off

#### Decorator

Allows the layering of "arbitrary combinations of behaviours to individual instances of objects."

* Well understood example of composition over inheritance
* A new object is created by instantiating an instantiated component wrapped with `Decorator`
* Removes the need to generate combination of subtypes
* An example of Single Responsibility principle
* Supports the Open/Closed principle

* Might want to consider the strategy pattern if behaviours being added is more involved
* Does lead to proliferation of small classes
* Debugging is more difficult for layered objects

#### MVC

Separates the view of an application from its implementation so that they can be changed independently because, in practice, the view changes the most.

* Business logic and UI logic is usually separate
* Eases testability

Main components:

* Model
  * Contains all the data for an application and can validate its own integrity
  * Domain independent
  * A data-centric that doesn't know how to render itself
  * Highly reusable
* View
  * Model notifies it of any changes
  * Retrieves states from Model
  * There can be multiple views that depend on the same Model
* Controller
  * Tightly bound to the view it's associated with
  * Responses to changes in the view and updates the Model accordingly

1. Changes occur in the view and Controller handles those changes
2. Controller updates the Model
3. Model notifies View of changes
4. View retrieves state from Model for render

* The massive view controller paradigm—to easy to put too much logic in the view, which is the most difficult part to test. Functionalities should be implemented in Controller as much as possible
* Performance could be an issue if there are too many objects in Model
* Deconstruction can be difficult

#### MVP

A modernisation on MVC that aims to improve testability, by decreasing the code that's in the View and forcing them onto the Presenter.

* Presenter
  * Should not have dependencies on UI components
  * Needs to be easy to test
  * Updates the Model and retrieve states from it
  * Make sure that View does not interact with Model

1. Actions in the View get passed to the presented
2. Presenter updates the Model
3. Model notifies Presenter of changes/event bus
4. Presenter updates the View

### Construction

#### Non-Structural Properties: Readability

* Easier to understand
* Easier to make changes
* Reduce the chance of introducing bugs

**Anti-patterns**

* Deep nesting
* Bad naming—one shouldn't need to go into the implementation of a method to know what is going on
* Individual style
* No comments
* Large blocks with significant sub-blocks

#### Static Analysis and Linters

* Simplest form of static analysis is the compiler

#### Automation

#### Code Smells

Software changes:

* Corrective—fix defects
* Adaptive—help systems with external changes
* Perfective—add or modify functionality
* Preventative—improve structure

Code smells:

* Bloaters
* OO Abusers
* Change preventers
* Dispensables
* Couplers

#### Refactoring

* Often triggered by quality concerns
* Indication from code review
* Benefits are mainly for technical teams, risks and costs are borne by other stakeholders
* Often suffers from second system effect
* Earlier is easier
* Integrates well with Agile

**Rule of Threes**

1. Code the feature
2. Code but take note
3. Refactor first

* Premature refactoring could be a problem itself

**Refactoring Steps**

1. Understand
2. Transform
3. Refine


## Resources

* See Introduction to Concurrency and Asynchronous Development: Part 2 for a good explanation of how the call stack, web APIs, event loop and callback queue interact
