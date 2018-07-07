# Software Engineering: Introduction

## Table of Contents

* [Software Construction: Data Abstraction](#software-construction-data-abstraction)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [Software Construction: Data Abstraction](https://www.edx.org/course/software-construction-data-abstraction-ubcx-softconst1x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### 1. Introduction to Software Construction

#### First Look

* Look and files and folders
* Run the program
* Relate the app to the code
* Inter-package relationship
* Inter-file relationship

### 2a. Control and Data Flow: Structures

#### Call Graphs

A graph showing what methods is called by other methods.

#### Classes and Objects

* Class is a way of encapsulating information
* A class contains the characteristics of the data it represents
* A class contains operations which can be performed on the data it represents (methods)

#### Anatomy of a Class

* `final`: immutable
* `static`: stays within a class and not instantiated with an object

**Style conventions**

1. Important statements
2. Class declaration
3. Constants and fields
4. Constructors
5. Public and private methods

### 2b: Control and Data Flow: Models

#### Data Flow

How information is passed around in a program.

#### Debugging, Part 3

> What is the main reason to avoid magic numbers?

> Because we don't do magic, we do computer science

Gold!

#### Quiz

> if the condition at A was changed to (!(condition at A)) then how would the flowchart for the method change: (Assume only labels are used for the flowchart)

That last bit about "Assume only labels are used for the flowchart" is misleading. D: The chart would reverse if we were not using labels, but if the label `A` is used to abstract away all the detail, as suggested by the provided assumption, then it does not matter if the conditions inside A is now the reverse of what it was before—because `A` will always evaluate to either `true` or `false` and the branches in the flow chart does not reverse...

### 3a: Data Abstraction: Specifying and Using

#### What is Abstraction?

* Hiding implementation detail behind an interface/container
* There are different levels of abstraction

Examples:

* Statement
* Functional
* Data abstraction
  * Classes

#### Visibility

Four phase process for designing and implementing abstraction:

1. Specify
2. Use
3. Test
4. Implement

#### Specifying a Data Abstraction

1. Requires clause—preconditions for correctness
2. Modifies clause—what does the method change
3. Effects clause—what does the method do

#### Requires

Examples:

* Requires: `amount >= 0`
* Requires: `y != 0`

#### Modifies

Examples:

* Modifies: `this` (implementation detail)
* Modifies: `this`, `deposits`

#### Effects

Examples:

* Effects: Adds `amount` fo `balance`

#### Using a Data Abstraction

Going through possible usage scenarios for how a data abstraction may be called by a client.

#### Discussion: Black Box and White Box Testing

> Black Box Testing is a way of testing software without knowing any of the internal structure or implementation details of the system being tested. In this case, we are interested in testing the functionality of the software.

> White Box Testing is a way of testing software that actually tests the internal structure and implementation details of the system.

#### JUnit, part 1 (\@Test and Fields)

* `@Test`
* `@Before`

#### Testability

### 3b: Data Abstraction: Testing and Implementing

#### Implementing a Data Abstraction




## Resources
