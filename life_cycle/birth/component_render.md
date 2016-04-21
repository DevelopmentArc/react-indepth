# Component `render()`
Now that we have pre-configured our component, we enter the first rendering of our content. As React developers, the `render()` method is the most familiar. We create Elements (generally via JSX) and return them. We access the Component `this.props` and `this.state` and let these values derive how content should be generated. When we access `this.state`, any changes we made during `componentWillMount()` are fully applied. 

Unlike any other method in the Life Cycle, `render()` is the one method that exists across multiple life cycle phases. It occurs here in Birth and it is where we spend a lot of time in Growth. 

In both cases, we have the core principal of keeping `render()` a pure method. What does that mean? That means we shouldn't call `setState()`, query the Native UI or anything else that can mutate the existing state of the application. The reason why is if we do this kind of interaction in `render()`, then it will kickoff another render pass. Which once again, triggers `render()` which then does the same thing... infinitely.

The React development tools are generally great at catching these kinds of errors and will yell at you if you do them. For example, if we did something silly like this

```javascript
render() {
  // BAD: Do not do this!
  this.setState({ foo: 'bar' });
  return (
    <div className={ classNames('person', this.state.mode) }>
      { this.props.name } (age: { this.props.age })
    </div>
  );
}
```

React would log out the following statement:

> Warning: setState(...): Cannot update during an existing state transition (such as within `render`). Render methods should be a pure function of props and state.

## Native UI access in render is often fatal
React will also warn you if you try to access the Native UI elements in the render pass.

```javascript
render() {
  // BAD: Don't do this either!
  let node = ReactDOM.findDOMNode(this);
  return (
    <div className={ classNames('person', this.state.mode) }>
      { this.props.name } (age: { this.props.age })
    </div>
  );
}
```

> VM943:45 Warning: Person is accessing getDOMNode or findDOMNode inside its render(). render() should be a pure function of props and state. It should never access something that requires stale data from the previous render, such as refs. Move this logic to componentDidMount and componentDidUpdate instead.

In the above example, it may seem safe since you are just querying the node. But, as the warning states, we might be querying potentially old data. But in our case, during the Birth phase, this would be a fatal error.

> Uncaught Invariant Violation: findComponentRoot(..., .0): Unable to find element. This probably means the DOM was unexpectedly mutated (e.g., by the browser), usually due to forgetting a &lt;tbod&gt; when using tables, nesting tags like &lt;form&gt;, &lt;p&gt;, or &lt;a&gt;, or using non-SVG elements in an &lt;svg&gt; parent. Try inspecting the child nodes of the element with React ID ``.

This is one of those cases where the React error doesn't clearly point to the cause of the problem. In our case we didn't modify the DOM, so it feels like an unclear and potentially misleading error. This kind of error can cause React developers a lot of pain early on. Because we instinctually look for a place where we are changing the Native UI.

The reason we get this error is because during the first render pass, the Native UI doesn't exist yet. We are essentially asking React to find a DOM node that doesn't exist. Generally, when `ReactDOM` can't find the node, this is because something or someone mutated the DOM. So, React falls back to the most common cause. 

As you can see, having an understanding of the Life Cycle can help troubleshoot and prevent these often un-intuitive issues.

***Up Next:***[ Managing Children Components and Mounting](managing_children_components_and_mounting.md)