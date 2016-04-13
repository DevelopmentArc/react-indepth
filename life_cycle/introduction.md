# The React Life Cycle
 One of the defining factors of a life form is its life cycle. The common path is birth, growth into maturity and then the inevitable death. UI applications often follow a similar path. When the application is first started, we consider this Birth. The users interacts with application, which is Growth into Maturity. Eventually, the application is closed or navigated away from, which is leads to Death.
 
 Within the application, elements also follow this pattern. In the world of React, these elements are our components. The component life cycle is a continuous process, which occurs throughout the overall life cycle of our application. Understanding this process can lead to faster development, easier optimization and overall application health.
 
 ## Life cycle phases in React components
 Not all UI systems enable a life cycle pattern. This doesn't mean that a system is better or worse if a life cycle is or isn't implemented. All a life cycle does is provide a specific order of operation and a series of hooks to tie into said system. The React life cycle follows the common Birth, Growth, and Death flow. The React team has provided a series of methods you can implement/override to tap into the process.
 
 ### Phase 1: Birth / Mounting
 The first phase of the React component life cycle is the Birth/Mounting phase. This is where we start initialization of the component. The component's props and state are defined and configured. The component and all its children are mounted on to the DOM (or UI stack for Native). And finally we can do post-processing if required.
 
 ### Phase 2: Growth / Update
 The next phase of the life cycle is the Growth/Update phase. This is were we spend most of our time in the component's life. Here we get new props, change state, handle user interactions and communicate with the component heirarchy.
 
 ### Phase 3: Death / Unmounting
 


