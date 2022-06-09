---
title: Java Learning - Design Patterns
tags:
  - Java
categories:
  - Coding
    - Study
date: 2022-03-18 23:56:01
---


<!-- more -->
## Creational Patterns

> Manage the creation of objects as the code becomes increasingly complex.

1.  **Singleton**

*   A class that has only one instance, but no clear owner.
*   That instance is available everywhere.
*   The instance is initialized only when it's first used (_lazy initialization_).

2.  **Builder**

*   A **mutable factory** that constructs the to-be-created object property by property.
*   Usually supports **method chaining**.
*   Often used to create **immutable** data objects.

1.  **Abstract Factory**

*   The thing creating objects is also an object.
*   Encapsulate construction of several related objects into a single Java interface.
*   Hide construction details from callers.

* * *

## Behavioral Patterns

> Patterns that involve how different objects interact.

1.  **Strategy**

*   You define an **interface** to represent a kind of **task** or problem.
*   Each concrete implementation defines a different "strategy" for solving the task.
*   The strategies can be swapped for each other.

2.  **Template Method**

*   You define a **base class** or interface (**template**) for a procedure or algorithm.
*   But leave **empty placeholders** (blank or default **method**) for some parts of the procedure.
*   Callers fill in the blanks by **extending** the base class and overriding the placeholder methods.

* * *

## Structural Patterns

> Patterns that involve how objects fit together to form the structure of the software.

1.  **Adapter**

*   Transform one API or interface into another.
*   Typically "wrap" an existing interface to adapt it to a different interface.
*   One common use of the adapter pattern is to wrap legacy APIs.

2.  **Decorator**

#### <span style="color:red">\***Adapter vs Decorator**</span>

*   These patterns both "wrap" another object, called the **delegate**.
*   An **Adapter** returns a **different interface** than the delegate.
*   A **Decorator** returns the **same interface**, but with **added functionality** or responsibilities.

* * *

## Dependency Injection

> Moves the creation of dependencies to outside of your code.

*   Add `@Inject` annotations to inject objects from a DI framework.
*   Use **modules** to configure which classes or objects should be used when an interface is injected.
*   Usually can be configured to return a **specific instance** of an object.
*   Take care of indirect, or transitive, dependencies.