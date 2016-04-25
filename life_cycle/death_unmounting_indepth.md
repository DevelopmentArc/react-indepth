# Death/Unmounting In-depth
 After our Component has spent time in the Update phase, we eventually enter the Death phase. During this phase our component is Unmounted from the Native UI stack and is marked for Garbage Collection.
 
 We enter this phase when our UI changes and the Element Tree no longer has a matching key to our Component. This could be changing layout or programmatically changing keys (forcing a new Component instance to be created). Once this occurs, React looks at the instance being removed and its children.
 
 ![](birth/react-element-tree.png)
 
 If we are removing **A**, then React will start with the bottom most child **A.2** and work its way back up the tree.
 