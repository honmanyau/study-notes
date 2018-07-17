# Software Development Capstone Project

## Table of Contents

* [Software Development Capstone Project](#software-development-capstone-project)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)

## Disclaimer

Everything in this file is derived from going through [Software Development Capstone Project](https://www.edx.org/course/software-development-capstone-project-ubcx-softengprjx) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### 1. Information Security

#### Information Security Introduction

Information security refers to the design and implementation of systems that can provide the following high-level properties:

* Confidentiality—prevents unauthorised parties from accessing information
* Integrity—the integrity of the data in a system should allow only authorised parties to access and manipulate the part that they are allowed to
* Availability—resources in a system should be available to authorised whenever it's needed
* Accountability—being able to audit confidentiality and integrity of the data within a system

Risk-cost balance.

Terms:

* Assets—parts being secured
* Subjects—people accessing a system (both authorised and unauthorised)
* Policies—rules that govern how the system is accessed by subjects
* Threats—the actors/subjects that/who try to violate policies to access assets in an unauthorised manner

Primary steps:

1. Understanding—model system and execution context
2. Analysis—what assets an attacker might want to access and how appropriate policies could be designed
3. Mitigation—what threats are to the policies identified and develop countermeasures
4. Validation

#### Information Security: Understanding

Where bugs vulnerabilities may be introduced.

* Bugs
* Vulnerabilities

Types of vulnerabilities:

* Physical
* System—how would a system react to DDoS by an unauthorised party, for example
* Network—MITM
* Human (Social?)

#### Information Security: Analysis

Consider subjects who could have an impact on high-level security properties.

* Malicious actors
* Untrained/unlucky authorised users

Things to consider:

* Costs
* Who is affected
* What a bad actor might be trying to achieve

**DREAD model** for modelling the impact of a breach:

* Damage
* Reproducibility
* Exploitability—how hard it is to perform the attack
* Affected users
* Discoverability

#### Information Security: Mitigation Part 1

Specific ways to defend against attacks.

* Attack trees
* Microsoft STRIDE mnemonic for consider different ways in which a system can be compromised
  * Spoofing—an attacker pretends to be someone else in order to gain access to a system
  * Tampering—gain access to the system so that it behaves incorrectly in the future
  * Repudiation—attacker gains access to a system, defeat the security in some way, but covers up tracks and remove traces
  * Information disclosure—gains access to information that the attacker shouldn't otherwise have
  * DDoS—deny access to the system of valid users
  * Elevation of privilege

* Think about inputs and outputs to the system because an attacker would use these channels to do IO
* Think about the code and system of data/authentication/encryption that's flowing in and out of the system
* Think about ways that the channels can be secured

Security is often in conflict with functional goals of a system.

#### Information Security: Mitigation Part 2

Mitigation principles for making systems easier to secure:

* Defence in depth—small breach shouldn't become a large one (for example, asking for password whenever changing data/access sensitive data). Cons: complexity, less usable
* Least Privilege—user should need only the minimum security credentials for performing a job. Implement finer-grain access control and by having strong security policies to begin with. Cons: complexity
* Separation of duties—authentication important and impactful tasks; before one person can do something important in a system, have validation from another person. Cons: could reduce velocity of the organisation
* Strong authentication principle—used enhance approaches for authenticating users, for example, passwords are weak. Cons: complexity
* Non-repudiation—users should be held accountable of their actions. A mechanism, for example, for providing non-repudiation is through auditing, for example, atomic logging mechanism. Cons: for example, log files may contain sensitive information

Google white paper on attack services from a layered perspective (and how mitigation principles can be applied in practice):

* Hardware infrastructure—verify hardware to meet security standards
* Service deployment—services can only talk to predefined services using explicit communication channels so no unexpected interactions
* User/identity—users use multi-factor authentication, users are only allowed access to services they work with
* Secure storage—encrypted storage (for example, a rotating set of encryption keys at Google)
* Internet communication—for example, Google Front End ensures optimal forward security applied
* Operational security—safe coding practices, for example, code review, think about security of code when writing

#### Information Security: Validation

Non-trivial.

Common anti-patterns:

* Rely on password auth alone
* Make assumptions about how firewalls and other countermeasures will protect a system perfectly
* Rely on update policy, OS, library an frameworks
* Rely on penetration testing alone

Approaches to validate security:

* Code review—best time to see if security threats may be introduced
* Static analysis—use tools to analyse source code for known vulnerabilities; data flow analysis to see if sensitive information can leak out
* Dynamic analysis—examples, running malware bots against a system or automated fuzzing techniques
* Penetration testing—specialised dynamic analysis. User try to defeat a system by creative means. It does not show the absence of security issues and should not be solely relied upon

Fuzz testing—black box, complementary to other techniques listed above and can indicate issues that evade other approaches listed above. 1. Identifies main points of entries; 2. generate random data and types of data against these endpoints; 3. trick the system into accepting requests that it should reject. Most common failure is program crash.

### 2. Intellectual Property Introduction
















## Resources
