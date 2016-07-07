# Tapping into `componentWillUpdate()`
 Once we have determined that we do need to re-render in our Update phase, the `componentWillUpdate()` will be called. The method is passed in two arguments: `nextProps` and `nextState`. The method `componentWillUpdate()` is [similar to `componentWillMount()`](../birth/premounting_with_componentwillmount.md), and many of the same considerations and tasks are the same. The difference being that `componentWillUpdate()` is called every time a re-render is required, such as when `this.setState()` is called. Unlike `componentWillMount()` we get access to the next `props` and `state`.
 
 Just like `componentWillMount()`, this method is called before `render()`. Because we have not rendered yet, our Component's access to the Native UI (DOM, etc.) will reflect the old rendered UI. Unlike, `componentWillMount()` we can access `refs` but in general this is not recommended because the refs will soon be out of date. There are use cases for accessing the Native UI here, such as starting animations.

The `componentWillUpdate()` is a chance for us to handle configuration changes and prepare for the next render. If we want to access the old props or state, we can call `this.props` or `this.state`. We can then compare them to the new values and make changes/calculations as required.

Unlike `componentWillMount()`, we should not call `this.setState()` here. The reason we do not call `this.setState()` is that the method triggers another `componentWillUpdate()`. If we trigger a state change in `componentWillUpdate()` we will end up in an infinite loop [^1].

Some of the more common uses for `componentWillUpdate()` is to set a variable based on state changes (not using `this.setState()`), dispatching events or starting animations [^2].

```javascript
\\ dispatching an action based on state change
componentWillUpdate(nextProps, nextState) {
  if (nextState.open == true && this.state.open == false) {
    this.props.onWillOpen();
  }
}
```

***Up Next:*** [Re-rendering and Children Updates](rerendering_and_children_updates.md)

---

[^1] In the previous version of this section we mistakenly said that you can safely call `setState()` in this method. Our assumption at the time was that a dirty flag was tracking the current state of the render pass, but this is not the case. It is technically possible to call `setState()` behind a conditional (such as when a prop/state changes) but it is **not** recommended and should be considered a no go. Special thanks to [Robin Venneman](https://github.com/robinvenneman) for catching this error and calling it to our attention!

[^2] An example of triggering CSS transitions in `componentWillUpdate()` and other discussions around this method's usages is over at this [StackOverflow response](http://stackoverflow.com/a/33075514/815544).