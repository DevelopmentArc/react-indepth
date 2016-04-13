# Birth/Mounting In-depth
 A React component kicks off the life cycle during the initial application ex: `ReactDOM.render()`. With the initialization of the component instance, we start moving through the birth phase of the life cycle. Before we dig deeper into the mechanics of the birth phase, let's step back a bit and talk about what this phase focuses on.
 
 The most obvious focus of the birth phase is the initial configuration for our component instance. Here we pass in the props that will define the instance. But during this phase there are a lot more moving pieces that we can tie into. For example, this phase is where the instance configures the default state and gets ready for the display. 
 
 After configuring the current instance, it starts the creation process for children of the component. Once the children are mounted, we get initial access to the Native UI layer[^1] (DOM, UIView, etc.). With Native UI access, we can start to query and modify how our content is actually displayed. This is also when we can begin the process of integrating 3rd Party UI libraries and components.
 
## Components vs. Elements
 When learning React, may developers have a common misconception. At first glance, one would assume that a mounted instance is the same as a component class. For example, if I create a new React component and then `render()` it to the DOM:
 
 ```javascript
 import React from 'react';
 import ReactDOM from 'react-dom';
 
 class MyComponent extends React.Component {
   render() {
     return <div>Hello World!</div>;
   }
 };
 
 ReactDOM.render(<MyComponent />, document.getElementById('mount-point')); 
 ```
 
 The initial assumption is that during `render()` an instance of the `MyComponent` class is created, using something like `new MyComponent()`. This instance is then passed to render. Although this assumption sounds reasonable, the reality of the process is a little more involved.
 
 What is actually occurring is the JSX processor converts the `ReactDOM.render()` line to use `React.createElement` to generate the instance. This generated Element is passesed to the `render()` method:
 
 ```javascript
 // generated code post-JSX processing
 ReactDOM.render(
   React.createElement(MyComponent, null), document.getElementById('mount-point')
 );
 ```
 
 A React Element is really a description[^2] of what will eventually be handed to the Native UI. This is a core, pardon the pun, *element* of virtual DOM technology in React.
 
 
> The primary type in React is the ReactElement. It has four properties: type, props, key and ref. It has no methods and nothing on the prototype.
>
> -- https://facebook.github.io/react/docs/glossary.html#react-elements


The Element is a lightweight object, so if we try to access it like it is the Class we will have some issues, such as availability of methods. 

So, how does this tie into the life cycle? 
 
## The First `render()`
 
## Initialization & Construction

## Default Props

## Initial State

## Pre-mounting with `componentWillMount()`

## Component `render()`

## Managing Children Components

## Post-mount with `componentDidMount()`

 
 

---

[^1] The Native UI layer is actual system that handles UI rendering to screen. In a browser, this is the DOM. On device, this would be the UIView. React handles the translation of content to the native layer format, but offloads the actual visual rendering to the platform being used.

[^2] Dan Abramov chimed in with this terminology on a StackOverflow question. http://stackoverflow.com/a/31069757