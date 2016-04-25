# Managing Children Components and Mounting
Now that we have completed the first render pass, our `render()` method returns a single React Element. This Element may have children elements. Those children, may also have children. etc. etc.

![React Element Tree](react-element-tree.png)

With the potential for an *n* depth tree of Elements, each of the Elements need to go through their own entire life cycle process. Just like the parent Element, React creates a new instance for each child. They go through construction, default props, initial state, `componentWillMount()` and `render()`. If the child has children, the process starts again... all the way down.

One of the most powerful concepts in React is the ability to easily compose complex layout through nesting of children. It is encouraged to keep your Components as *'dumb'* as possible. The idea is to only have container[^1] components managing higher level functionality.

Because this is the preferred way of development, this means we will have a lot of smaller components that also have their own life cycle. Keep this in mind as we continue through the life cycle, because every Component will follow the same pattern.

***Up Next:*** [Post-Mount with `componentDidMount()`](post_mount_with_component_did_mount.md)

---

[^1] See [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.pnmirdrso) by Dan Abramov for more details