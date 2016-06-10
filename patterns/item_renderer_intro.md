# Component Evolution and Composition
 Component reuse and composability are some of the core tenants of React development. As our applications scale, development time can be dramatically reduced through this process. Yet, creating reusable Components takes planning and understanding to support multiple use cases.
 
 Understanding the intention of the Component is the first step towards reuse. Sometimes, we know a Component will be used in many different ways from the start. In those situations, we can plan for the different scenarios right away. In other situations, Component intentions will change over the lifespan of the application. Understanding how to evolve a Component is just as important as understanding how to create re-usability.
 
 ##The Application Architecture process
 
 Let's take a quick moment and discuss the process of application architecture. We often hear about over-architected systems. This often occurs when we try to plan for every possible scenario that could ever occur through the life of an application. To try and support every conceivable use is a fools errand. When we try to build these systems we add unnecessary complexity and often make development harder, rather then easier.
 
 At the same time, we don't want to build a system that offers no flexibility at all. It maybe faster to just build it without future thought, but adding new features can be just as time consuming later on. Trying to find the right balance is the hardest part of application architecture. We want to create a flexible application that allows growth but we don't waste time on possibilities.
 
 The other challenge with application architecture is trying to understand our needs. With development, we often have to build out something to truly understand it. This means that our application architecture is a living process. It changes over time due to having better understanding of what's required. Refactoring Components is critical to the success of a project and makes adding new features easier.
 
 Because of this process, we felt it is important to walk through the evolution of a Component. 
We will start with a naive approach to building a List Component and then walk through different refactorings to support reusablity. More then likely, we would know early on that a List should be reusable. But, walking through the evolution process can help deepen our understanding of how to enable reusablity.
 
 Up Next: [The Evolution of a List Component](/patterns/the_evolution_of_a_list_component.md)