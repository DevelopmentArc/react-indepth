# Managing Children Components and Mounting
Now that we have completed the first render pass, our `render()` method returns a React Element. This Element may have many children elements. Those children, may also have children. etc. etc.

![React Element Tree](react-element-tree.png)

With the potential for an *n* depth tree of Elements, each of the instances need to go through their own entire life cycle process.