# Post-mount with `componentDidMount()`
 The last step in the Birth/Mount process life cycle phase is our post-mount access via `componentDidMount()`. This method is called once all our children Elements and our Component instance is mounted onto the Native UI. When this method is called we now have access to the Native UI (DOM), access to our children `refs` and the ability to *potentially* trigger a new render pass.
 
## Understanding call order
 Similar to `componentWillMount()`, `componentDidMount()` is only called one time. Unlike our other life cycle methods, where we start at the top and work down, `componentDidMount()` works from the bottom up. Let's consider the following Component/Element Tree:
 
 ![React Element Tree](react-element-tree.png)

 When we begin the Birth phase, we process initialize through `render()` in this order: 
 
 ```
 A -> A.0 -> A.0.0 -> A.0.1 -> A.1 -> A.2.
 ```
 
 With `componentDidMount()` we start at the end and work our way back.
 
 ```
 A.2 -> A.1 -> A.0.1 -> A.0.0 -> A.0 -> A
 ```
 
 By walking backwards, we know that every child has mounted and also run it's own `componentDidMount()`. This guarantees the parent can access the Native UI elements for itself and it's children.
  
 Let's consider the following three components and their call order.

**GrandChild.js**
```javascript
/** 
 * GrandChild
 * It logs the componentDidMount() and has a public method called value.
 */ 
import React from 'react';
import ReactDOM from 'react-dom';

export default class GrandChild extends React.Component {

  componentDidMount() {
    console.log('GrandChild did mount.');
  }

  value() {
    return ReactDOM.findDOMNode(this.refs.input).value;
  }

  render() {
    return (
      <div>
        GrandChild
        <input ref="input" type="text" defaultValue="foo" />
      </div>
    );
  }
}
```

**Child.js**
```javascript
/*
 * Child
 * It logs the componentDidMount() and has a public method called value,
 * which returns the GrandChild value.
 */
import React from 'react';
import GrandChild from './GrandChild';

export default class Child extends React.Component {

  componentDidMount() {
    console.log('Child did mount.');
  }

  value() {
    return this.refs.grandChild.value();
  }

  render() {
    return (
      <div>
        Child
        <GrandChild ref="grandChild" />
      </div>
    );
  }
}
```

**Parent.js**
  