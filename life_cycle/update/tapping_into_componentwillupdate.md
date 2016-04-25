# Tapping into `componentWillUpdate()`
 Once we have determined that we do need to re-render in our Update phase, the `componentWillUpdate()` will be called. The method is passed in two arguments: `nextProps` and `nextState`. The method `componentWillUpdate()` is [similar to `componentWillMount()`](../birth/premounting_with_componentwillmount.md). The difference being that `componentWillUpdate()` is called every time a re-render is required and we get access to the next props and state.
 
 