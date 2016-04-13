# Birth/Mounting In-depth
 A React component kicks off the life cycle when an Element instance is initialized from a Component Class during the initial `render()`. With the initialization of the element instance, we start moving through the birth phase of the life cycle. Before we dig deeper into the mechanics of the birth phase, let's step back a bit and talk about what this phase should focus on.
 
 The most obvious focus of the birth phase is the initial configuration for our component instance. Here we pass in the props that will define instance. But during this phase there are a lot more moving pieces that we can tie into. 
 
 For example, this phase is where the instance configures the default state and gets ready for the display. We state the child component creation process. It is also were we get initial access to the DOM (UIView). With this access, we can begin the process of integrating 3rd Party UI libraries. Or, we can start to query and modify how our content is actually displayed on the native UI layer[^1].
 
 The birth phase is a one time event, that 
 
## Components vs. Elements
 One of the early misconceptions that a lot of developers face when learning React is that once mounted, we are not working with the 
 
## The First `render()`
 
## Initialization & Construction

## Default Props

## Initial State

## Pre-mounting with `componentWillMount()`

## Component `render()`

## Managing Children Components

## Post-mount with `componentDidMount()`

 
 

---

[^1] The native UI layer is actual system that handles UI rendering. In a browser, this is the DOM. On device, this would be the UIView. React handles the translation of content to the native layer format, but offloads the actual rendering to the platform being used.