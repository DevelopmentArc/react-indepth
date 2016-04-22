# Managing Children Components and Mounting
Now that we have completed the first render pass, our `render()` method returns a single React Element. This Element may have children elements. Those children, may also have children. etc. etc.

![React Element Tree](react-element-tree.png)

With the potential for an *n* depth tree of Elements, each of the Elements need to go through their own entire life cycle process. Just like the parent Element, React creates a new instance for each child. They go through construction, default props, initial state, `componentWillMount()` and `render()`. If the child has children, the process starts again... all the way down.

***Up Next:*** [Post-Mount with `componentDidMount()`](post_mount_with_component_did_mount.md)