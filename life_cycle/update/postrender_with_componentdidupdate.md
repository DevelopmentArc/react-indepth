# Post-Render with `componentDidUpdate()`
 Continuing the trend of corresponding methods, the `componentDidUpdate()` is the Update version of [`componentDidMount()`](../birth/post_mount_with_component_did_mount.md). Once again, we can access the Native UI stack, interact with our `refs` and if required start another re-render/update [^1].
 
 When `componentDidUpdate()` is called, two arguments are passed: `prevProps` and `prevState`. This is the inverse of `componentWillUpdate()`. The passed values are what the values where and accessing `this.props` and `this.state` are the current values.
 
 ---
 
 [^1] This is a risky behavior and can easily enter an infinite loop. Proceed with caution.
