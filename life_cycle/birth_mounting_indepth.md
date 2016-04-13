# Birth/Mounting In-depth
 A React component kicks off the life cycle when an Element instance is initialized from a Component Class during the initial `render()`. With the initialization of the element instance, we start moving through the birth phase of the life cycle. Before we dig deeper into the mechanics of the birth phase, let's step back a bit and talk about what this phase should focus on.
 
 The most obvious focus of the birth phase is the initial configuration for our component instance. Here we pass in the props that will define instance. The instance configures the default state and gets ready for the display. Children creation begins. We get access to the DOM (UIView) 
 
## Components vs. Elements
 One of the early misconceptions that a lot of developers face when learning react is that once mounted, we are not working with the 
 
## The First `render()`
 
## Initialization & Construction

## Default Props

## Initial State

## Pre-mounting with `componentWillMount()`

## Component `render()`

## Managing Children Components

## Post-mount with `componentDidMount()`

 