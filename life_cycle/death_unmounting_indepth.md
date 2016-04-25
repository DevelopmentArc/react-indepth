# Death/Unmounting In-depth
 After our Component has spent time in the Update phase, we eventually enter the Death phase. During this phase our component is Unmounted from the Native UI stack and is marked for Garbage Collection.
 
 We enter this phase when our UI changes and the Element Tree no longer has a matching key to our Component. This could be changing layout or programmatically changing keys (forcing a new Component instance to be created). Once this occurs, React looks at the instance being removed and its children.
 
## Using `componentWillUnmount()`
 Just like the rest of our life cycle phases, the Death/Unmount phase has a method hook for us. This method allows us to do some cleanup before we are removed from the UI stack. Typically we want to reverse any setup we did in either `componentWillMount()` or `componentDidMount()`.
 
 For example, we would want to unregister any global/system/library events, destroy 3rd party UI library elements, etc. If we don't take the time to remove events we can create memory leaks in our system or leave bad references laying around.
 
  ![](birth/react-element-tree.png)
 
 React starts with the Element being removed, for example **A**, and calls `componentWillUnmount()` on it. Then React goes to the first child (**A.0**) and does the same, working its way down to the last child. Once all the calls have been made, then React will remove the elements from the UI and ready them for Garbage Collection.
 
 ***Up Next:*** Life Cycle Recap 
 

 