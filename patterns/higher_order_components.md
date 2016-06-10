# Higher Order Components
 The last Component composition pattern we will examine in this section is *Higher Order Components* (HOC). As [Dan Ambrov discusses](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.b74nxbqew), Higher Order Components were first proposed by [Sebastian Markbåge](https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775) in a gist. The core idea of HOC is to define a function, which you pass one or more Components to. This function generates and returns a new Component, which is a wrapper around the passed in Component(s).
 
 The need for HOC came about with React's move to support ES6 classes and the lack of mixin support with the new JavaScript Class syntax. To handle this change, a new pattern needed to be defined to replace mixins. Typically, mixins add/override functionality around the [Component Life Cycle](../life_cycle/introduction.md) and enable sharing reusable code in a elegant way. Without mixin support in ES6, the HOC pattern is required.
 
 ### A form group example
  For our HOC example, we will create a function for wrapping a Component in a custom form group with an optional `<label>` field. The goal of the HOC is to allow us to create two outputs, with and without a label:
  
  ```html
  <!-- With a label -->
  <div class="form-group">
    <label class="form-label" for="firstName">First Name:</label>
    <input type="text" name="firstName" />
  </div>
  
  <!-- Without a label -->
  <div class="form-group">
    <input type="text" name="lastName" />
  </div>
  ```
 
 Because this could become a common task, we can use the HOC pattern to generate our form group wrapper and let it decide if it should inject the label or not.
 
 **formGroup.js**
```javascript
import React from 'react';
import { isString } from 'lodash';

function formGroup(Component, config) {
  const FormGroup = React.createClass({
    __renderLabel() {
      // check if the passed value is a string using Lodash#isString
      if (isString(this.props.label)) {
        return(
          <label className="form-label" htmlFor={ this.props.name }>
            { this.props.label }
          </label>
        );
      }
    },

    __renderElement() {
      // We need to see if we passed a Component or an Element
      // such as Profile vs. <input type="text" />
      if (React.isValidElement(Component)) return React.cloneElement(Component, this.props);
      return( <Component { ...this.props } />);
    },

    render() {
      return(
        <div className="form-group">
          { this.__renderLabel() }
          { this.__renderElement() }
        </div>
      );
    }
  });

  return(<FormGroup { ...config } />);
}

export default formGroup;
```

To use this HOC we can do the following:

**index.js**
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import formGroup from './higherOrderComponents/formGroup';

let MyComponent = React.createClass({
  render() {
    return (
      <div>
        { formGroup(<input type="text" />, { label: 'First Name:', name: 'firstName' }) }
      </div>
    );
  }
});

ReactDOM.render(<MyComponent />, document.getElementById('mount-point'));
```

Let's examine the above code. The first thing we do for the HOC is create a function called `formGroupBuilder` which takes two arguments: `Component` and `config`.

```javascript
function formGroupBuilder(Component, config) {
  ...
}

export default formGroupBuilder;
```

The Component will be the instance we want to wrap in our form group. In the function, we create a new React Component and then return an Element instance using the config as props.

```javascript
const FormGroup = React.createClass({
  ...
});

return(<FormGroup { ...config } />);
```

We take advantage of the [ES6 spred operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) to pass in our `config` object as the props for the generated JSX Element. In our `render()` method we create the form group `<div>` and then render out our optional label and Component content.

```javascript
render() {
  return(
    <div className="form-group">
      { this.__renderLabel() }
      { this.__renderElement() }
    </div>
  );
}
```

In our `__renderLabel()` method[^1] we use the [Lodash `isString`](https://lodash.com/docs#isString) method to check if the label value is a string. If so, we render out our label DOM element, otherwise we return `null`.

```javascript
__renderLabel() {
  // check if the passed value is a string using Lodash#isString
  if (isString(this.props.label)) {
    return(
      <label className="form-label" htmlFor={ this.props.name }>
        { this.props.label }
      </label>
    );
  }
},
```

Because `null` does not render out to the Native UI in React, this is how we make the `<label>` optional based on the passed value. 

Finally, we had to add a check to determine what type was passed to our HOC function for the Component. This is an important check because we want to support both React Components and Elements. 

In our `index.js` we are passing in:

```javascript
formGroup(<input type="text" />, { label: 'First Name:', name: 'firstName' })
```

Because we are using JSX to generate our `<input />` the HOC will receive an Element. But, if we used our Profile component, we may not want to use JSX:

```javascript
formGroup(Profile, { label: 'First Name:', name: 'firstName' })
```

To support both options and pass on the `props`, we use the `__renderElement()` method to handle the inspection and output generation:

```javascript
__renderElement() {
  // We need to see if we passed a Component or an Element
  // such as Profile vs. <input type="text" />
  if (React.isValidElement(Component)) return React.cloneElement(Component, this.props);
  return( <Component { ...this.props } />);
},
```

If the Component instance is an element, we [clone the element](https://facebook.github.io/react/docs/top-level-api.html#react.cloneelement) and pass on the new props. Otherwise, we generate a new Element using JSX and the passed in React Component.

This HOC example is just the tip of the iceberg when it comes to self-generating wrapper components. Using this pattern, We can tap into the [Component Life Cycle methods](../life_cycle/introduction.md), we can make more complex decisions based on the data, we can register to stores or other events, and many other possible combinations.

For more in-depth examples we highly recommend reading Dan Abramov's *[Mixins Are Dead. Long Live Composition](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.9y0gg1ix5)* and @franlplant's *[React Higher Order Components in depth](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e#.d38rbnsu8)* 

---

[^1] In these examples we are prefixing our methods with `__` to reflect that these are internal component methods. This is completely optional and is just our preferred style syntax.

