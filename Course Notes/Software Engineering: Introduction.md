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










## Resources

* See Introduction to Concurrency and Asynchronous Development: Part 2 for a good explanation of how the call stack, web APIs, event loop and callback queue interact
