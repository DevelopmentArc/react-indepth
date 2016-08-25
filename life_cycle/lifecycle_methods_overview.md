# React Life Cycle Methods Overview
 The React development team provides a series of hooks we can tap into at each phase of the life cycle. These method hooks inform us of where the Component is in the life cycle and what we can and cannot do.
  
  Each of the life cycle methods are called in a specific order and at a specific time. The methods are also tied to different parts of the life cycle. Here are the methods broken down in order and by their corresponding life cycle phase [^1]:
  
## Birth / Mounting
1. Initialize / Construction
2. `getDefaultProps()` *(React.createClass)* or `MyComponent.defaultProps` *(ES6 class)*
3. `getInitialState()` *(React.createClass)* or `this.state = ...` *(ES6 constructor)*
4. `componentWillMount()`
5. `render()`
6. Children initialization & life cycle kickoff
7. `componentDidMount()`
  
## Growth / Update
1. `componentWillReceiveProps()`
2. `shouldComponentUpdate()`
3. `componentWillUpdate()`
3. `render()`
4. Children Life cycle methods
5. `componentDidUpdate()`

## Death / Unmount
1. `componentWillUnmount()`
4. Children Life cycle methods
5. Instance destroyed for Garbage Collection

The order of these methods are strict and called as defined above. Most of the time is spent in the Growth/Update phase and those methods are called many times. The Birth and Death methods will only be called once.

***Next Up***: [Birth/Mounting in-depth](birth_mounting_indepth.md)

---

[^1] *Most of the methods are the same if you use either `React.createClass` or use ES6 classes, such as `class MyComponent extends React.Component`. A few are different, mainly around how instantiation/creation occurs. We will call these differences out throughout the chapter.*
