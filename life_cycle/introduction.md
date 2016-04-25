# The React Life Cycle
 One of the defining factors of a life form is its life cycle. The common path is Birth, Growth into maturity and then the inevitable Death. UI applications often follow a similar path. When the application is first started, we consider this Birth. The users interacts with application, which is Growth. Eventually, the application is closed or navigated away from, which is leads to Death.
 
 Within the application, elements also follow this pattern. In the world of React, these elements are our Components. The Component life cycle is a continuous process, which occurs throughout the overall life cycle of our application. Understanding this process can lead to faster development, easier optimization and overall application health.
 
 ## Life cycle phases in React components
 Not all UI systems enable a life cycle pattern. This doesn't mean that a system is better or worse if a life cycle is or isn't implemented. All a life cycle does is provide a specific order of operation and a series of hooks to tie into said system. The React life cycle follows the common Birth, Growth, and Death flow. The React team has provided a series of methods you can implement/override to tap into the process.
 
 ### [Phase 1: Birth / Mounting](life_cycle/birth_mounting_indepth.md)
 The first phase of the React Component life cycle is the Birth/Mounting phase. This is where we start initialization of the Component. The Component's `props` and `state` are defined and configured. The Component and all its children are mounted on to the Native UI Stack (DOM, UIView, etc.). Finally, we can do post-processing if required. The Birth/Mounting phase only occurs once.
 
 ### [Phase 2: Growth / Update](life_cycle/growth_update_indepth.md)
 The next phase of the life cycle is the Growth/Update phase.  In this phase, we get new `props`, change `state`, handle user interactions and communicate with the component hierarchy. This is were we spend most of our time in the Component's life. Unlike Birth or Death, we repeat this phase over and over.
 
 ### [Phase 3: Death / Unmount](life_cycle/death_unmounting_indepth.md)
 The final phase of the life cycle is the Death/Unmount phase. This phase occurs when a component instance is unmounted from the Native UI. This can occur when the user navigates away, the UI page changes, a component is hidden (like a drawer), etc. Death occurs once and then Component is then ready for garbage collection.

***Next Up:*** [Life Cycle Methods Overview](lifecycle_methods_overview.md)
