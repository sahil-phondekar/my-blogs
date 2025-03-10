---
sidebar_position: 2
---

# Introduction to Inversion of Control(IoC) & Dependency Injection(DI)

- Inversion of Control (loC) is a Software Design Principle, independent of language, which does not actually create the objects but describes the way in which object is being created. 

- IoC is the principle, where the control flow of a program is inverted: instead of the programmer controlling the flow of a program, the framework or service takes control of the program flow. 

- Dependency Injection is the pattern through which Inversion of Control achieved. 

- Through Dependency Injection, the responsibility of creating objects is shifted from the application to the Spring loC container. It reduces coupling between multiple objects as it is dynamically injected by the framework. 

## Advantages of IoC & DI

- Loose coupling between the components
- Minimizes the amount of code in your application
- Makes unit testing easy with different mocks
- Increased system maintainability & module reusability
- Allows concurrent or independent development
- Replacing modules has no side effect on other modules
