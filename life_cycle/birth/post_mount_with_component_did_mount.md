# Post-mount with `componentDidMount()`
 The last step in the Birth/Mount process life cycle phase is our post-mount access via `componentDidMount()`. This method is called once all our children Elements and our Component instance is mounted onto the Native UI. When this method is called we now have access to the Native UI (DOM), access to our children `refs` and the ability to *potentially* trigger a new render pass.
 
## Understanding call order
 Similar to `componentWillMount()`, `componentDidMount()` is only called one time. Unlike our other life cycle methods, where we start at the top and work down, `componentDidMount`