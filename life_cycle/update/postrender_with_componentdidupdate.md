# Post-Render with `componentDidUpdate()`
 Continuing the trend of corresponding methods, the `componentDidUpdate()` is the Update version of [`componentDidMount()`](../birth/post_mount_with_component_did_mount.md). Once again, we can access the Native UI stack, interact with our `refs` and if required start another re-render/update [^1].
 
 When `componentDidUpdate()` is called, two arguments are passed: `prevProps` and `prevState`. This is the inverse of `componentWillUpdate()`. The passed values are what the values were, and `this.props` and `this.state` are the current values.
 
 ![](react-tree-update.png)
 
 Just like `componentDidMount()`, the `componentDidUpdate()` is called after all of the children are updated. Just to refresh your memory, **A.0.0** will have `componentDidUpdate()` called first, then **A.0**, then finally **A**.
 
## Common Tasks
 The most common uses of `componentDidUpdate()` is managing 3rd party UI elements and interacting with the Native UI. When using 3rd Party libraries, like our Chart example, we need to update the UI library with new data.
 
```javascript
componentDidUpdate(prevProps, prevState) {
  // only update chart if the data has changed
  if (prevProps.data !== this.props.data) {
    this.chart = c3.load({
      data: this.props.data
    });
  }
}
```

Here we access our Chart instance and update it when the data has changed[^2]. 

### Another render pass?
We can also query the Native UI and get sizing, CSS styling, etc. This may require us to update our internal state or `props` for our children. If this is the case we can call `this.setState()` or `forceUpdate()` here, but this opens a lot of potential issues because it forces a new render pass.

One of the worst things to do is do an unchecked `setState()`:

```javascript
componentDidUpdate(prevProps, prevState) {
  // BAD: DO NOT DO THIS!!!
  let height = ReactDOM.findDOMNode(this).offsetHeight;
  this.setState({ internalHeight: height });
}
```

By default, our `shouldComponentUpdate()` returns true, so if we used the above code we would fall into an infinite render loop. We would render, then call did update which sets state, triggering another render.

If you need to do something like this, then you can implement a check at  `componentDidUpdate()` and/or add other checks to determine when a re-size really occurred.

```javascript
componentDidUpdate(prevProps, prevState) {
  // One possible fix...
  let height = ReactDOM.findDOMNode(this).offsetHeight;
  if (this.state.height !== height ) {
    this.setState({ internalHeight: height });
  }
}
```

In general, this is not a common requirement and re-rendering has performance impacts for your Component and applications. Keep this in mind if you find yourself having to add a second render pass in `componentDidUpdate()`.

***Up Next:*** [Death/Unmounting In-depth](../death_unmounting_indepth.md)

---
 
 [^1] This is a risky behavior and can easily enter an infinite loop. Proceed with caution!
 
 [^2] This example assumes that the data is pure and not mutated.
