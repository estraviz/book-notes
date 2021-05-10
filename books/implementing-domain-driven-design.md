# Implementing Domain-Driven Design

by Vaughn Vernon

![](./resources/implementing-domain-driven-design.jpg "Implementing Domain-Driven Design")

---

## Table of contents

- [Chapter 1: Getting Started with DDD](#chapter1)
- [Chapter 2: Domains, Subdomains, and Bounded Contexts](#chapter2)
- [Chapter 3: Context Maps](#chapter3)
- [Chapter 4: Architecture](#chapter4)
- [Chapter 5: Entities](#chapter5)
- [Chapter 6: Value Objects](#chapter6)
- [Chapter 7: Services](#chapter7)
- [Chapter 8: Domain Events](#chapter8)
- [Chapter 9: Modules](#chapter9)
- [Chapter 10: Aggregates](#chapter10)
- [Chapter 11: Factories](#chapter11)
- [Chapter 12: Repositories](#chapter12)
- [Chapter 13: Integrating Bounded Contexts](#chapter13)
- [Chapter 14: Application](#chapter14)

---

<a name="chapter1">
    <h1>Chapter 1: Getting Started with DDD</h1>
</a>

* Domain Driven Design or DDD goal &rarr; high-quality SW model designs.
* DDD &rarr; Strategic + Tactical modeling tools.

### Can I DDD?

* DDD &rarr; discussion, listening, understanding, discovery, and business value.
* Produce a Ubiquitous Language.
* Learn from domain experts (who don't know everything about the business, BTW). Domain modeling concepts.
* Domain Model &rarr; SW model about your business domain.

### Why You Should Do DDD

* Domain experts + Develpers together. One cohesive team.
* SW as closes as possible to the business. Teach the business about itself.
* No translations. Common shared language.
* _The design is the code, and the code is the design._
* Strategic Design &rarr; what are the most important investments to make.
* Tactical Design &rarr; single elegant model with SW.

#### Delivering Business Value Can Be Elusive

* SW that delivers business value and aligns with strategic initiatives.

#### How DDD Helps

* Domain experts + SW Developers together. SW to reflect the mental model of business experts.
  * Develop a Ubiquitous Language. SW development as a justifiable business investment, not a cost center.
* Address the strategic initiatives of the business.
* Tactical design &rarr; SW as a codification of domain experts' mental model.

#### Grapping with the Complexity of Your Domain

* Core Domain &rarr; DDD in key areas to the business. Other areas &rarr; Supporting Domains.

#### Anemia and Memory Loss

* Anemic Domain Model &rarr; a model w/o behaviour.
* Object-relational impedance mismatch. A lot of effort in mapping to persistence, little benefit.

##### _Reasons Why Anemia Happens_

* Anemic Domain Models &rarr; procedural programming mentality.

##### _Look at What Anemia Does to Your Model_

* When domain experts cannot help understand the code, only programmers. Anemia-induced memory loss.

### How to Do DDD

* Pillars of DDD: 1) Ubiquitous Language, 2) Bounded Context.

#### Ubiquitous Language

* Shared team (domain experts + developers) language. Centered on the business.
* Drawings, glossary to capture the terms. Out-of sync with time, so it'll be abandoned.

##### _Ubiquitous, but Not Universal_

* Spoken among the team in a single domain, not the whole company. One per Bounded Context.

### The Business Value of Using DDD

* The organization gains a useful model of its domain.
* Understanding of the business.
* Domain experts contribute to SW design.
* Better user experience.
* Clean boundaries around pure models.
* Better organized Enterprise Architecture.
* Agile, continuous modeling.
* New strategic + tactical tools.

### The Challenges of Applying DDD

* Create a Ubiquitous Language.
* Involve domain experts.
* Challenge developers thought process to get to solutions. Conversations with domain experts.
* _It's about designing the behaviours of objects._

##### _Justification for Domain Modeling_

* Tactical modeling (Aggregates, Value Objects, Events...) more complex than strategic modeling.
* If Bounded Context = Core Domain &rarr; vital for the business.

##### _DDD Is Not Heavy_

* Test-first approach. Lightweight development.

<a name="chapter2">
    <h1>Chapter 2: Domains, Subdomains, and Bounded Contexts</h1>
</a>


<a name="chapter3">
    <h1>Chapter 3: Context Maps</h1>
</a>
