# Re-rendering and Children Updates
 Once again we return to `render()`. Now that our `props` and `state` are all configured we can update out content and children. Just like our initial render,[ all the same rules and conditions apply](../birth/component_render.md). 
 
 Unlike our first render, React performs different management when it comes to the generated Elements. The main difference is around the initialization phase and children Elements of the Component. React compares the current Element tree structure returned from the `render()` method. React uses the generated keys (or assigned keys) to identify each Element to a Component instance. React can then determine if we have new instances or are updating existing instances.
 
 If the keys are the same, them React will pass the `props` to the existing instance, kicking off its Update life cycle. If we have added new components or changed keys, React will create new instances from the Element data. These new Components then entire the Birth/Mounting phase.
 
 