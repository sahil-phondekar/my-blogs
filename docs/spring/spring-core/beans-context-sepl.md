---
sidebar_position: 3
---

# Introduction to Spring Beans, Context, SpEL

- Any normal Java class that is instantiated, assembled, and otherwise managed by a Spring loC container is called Spring Bean.

- These beans are created with the configuration metadata that you supply to the container either in the form of XML configs and Annotations. 

- Spring loC Container manages the lifecycle of Spring Bean scope and injecting any required dependencies in the bean. 

- Context is like a memory location of your app in which we add all the object instances that we want the framework to manage. By default, Spring doesn’t know any of the objects you define in your application. To enable Spring to see your objects, you need to add them to the context. 

- The SpEL (Sprint Expression Language) provides a powerful expression language for querying and manipulating an object graph at runtime like setting and getting property values, property assignment, method invocation etc. 