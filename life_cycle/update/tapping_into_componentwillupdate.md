# Tapping into `componentWillUpdate()`
 Once we have determined that we do need to re-render in our Update phase, the `componentWillUpdate()` will be called. The method is passed in two arguments: `nextProps` and `nextState`. The method `componentWillUpdate()` is [similar to `componentWillMount()`](../birth/premounting_with_componentwillmount.md), and many of the same considerations and tasks are the same. The difference being that `componentWillUpdate()` is called every time a re-render is required and we get access to the next `props` and `state`.
 
 Just like `componentWillMount()`, this method is called before `render()`. Because we have not rendered yet, our Component's access to the Native UI (DOM, etc.) will reflect the old rendered UI. Unlike, `componentWillMount()` we can technically access `refs` but it is not recommended because the refs will also be out of date.

The `componentWillUpdate()` is a chance for us to handle configuration changes, update our state and in general prepare for the next render. If we want to access the old props or state, we can call `this.props` or `this.state`. We can then compare them to the new values and make changes/calculations as required.

```javascript
\\ a similar version to our componentWillMount() method
componentWillUpdate(nextProps, nextState) {
  let mode;
  // we need to access nextProps here, not this.props
  if (nextProps.props.age > 70) {
    mode = 'old';
  } else if (this.props.age < 18) {
    mode = 'young';
  } else {
    mode = 'middle';
  }
  this.setState({ mode });
}
```
 
Because we have not rendered yet, we can still call `setState()` safely and not trigger another update. As you can see, the functionality we define here is to prepare for our impending render.

***Up Next:*** [Re-rendering and Children Updates](rerendering_and_children_updates.md)