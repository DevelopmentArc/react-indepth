# Component Evolution and Composition
 Component reuse and composability are some of the core tenants of React development. As our applications scale, development time can be dramatically reduced through this process. Yet, creating reusable Components takes planning and understanding to support multiple use cases.
 
 Understanding the intention of the Component is the first step towards reuse. Sometimes, we know a Component will be used in many different ways from the start. In those situations, we can plan for the different scenarios right away. In other situations, Component intentions will change over the lifespan of the application. Understanding how to evolve a Component is just as important as understanding how to create re-usability.
 
 ##The Application Architecture process
 
 Let's take a quick moment and discuss the process of application architecture. We often hear about over-architected systems. This often occurs when we try to plan for every possible scenario that could ever occur through the life of an application. To try and support every conceivable use is a fools errand. When we try to build these systems we add unnecessary complexity and often make development harder, rather then easier.
 
 At the same time, we don't want to build a system that offers no flexibility at all. It maybe faster to just build it without future thought, but adding new features can be just as time consuming later on. Trying to find the right balance is the hardest part of application architecture. We want to create a flexible application that allows growth but we don't waste time on possibilities.
 
 The other challenge with application architecture is trying to understand our needs. With development, we often have to build out something to truly understand it. This means that our application architecture is a living process. It changes over time due to having better understanding of what's required. Refactoring Components is critical to the success of a project and makes adding new features easier.
 
 Because of this process, we felt it is important to walk through the evolution of a Component. 
We will start with a naive approach to building a List Component and then walk through different refactorings to support reusablity. More then likely, we would know early on that a List should be reusable. But, walking through the evolution process can help deepen our understanding of how to enable reusablity.

## Higher Order Components
 The last Component composition pattern we will examine in this section is *Higher Order Components* (HOC). As [Dan Ambrov discusses](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.b74nxbqew), Higher Order Components where first proposed by [Sebastian Markbåge](https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775) in a gist. The core idea of HOC is to define a function, which you pass one or more Components to. This function generates and returns a new Component which is a wrapper around the passed in Component(s).
 
 The need for HOC came about with React's move to support ES6 classes and the lack of mixin support with the new JavaScript Class syntax. To handle this change, a new pattern needed to be defined to replace mixins. Typically, mixins add/override functionality around the [Component Life Cycle](../life_cycle/introduction.md) and enable sharing reusable code in a elegant way. Without mixin support in ES6, the HOC pattern was created.
 
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

function formGroupBuilder(Component, config) {
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

export default formGroupBuilder;
```

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

Let's examine the above code. The first thing we do is create a function called `formGroupBuilder` which takes two arguments: `Component` and `config`.

```javascript
function formGroupBuilder(Component, config) {
  ...
}

export default formGroupBuilder;
```

The Component will be the instance we want to wrap in our form group. In function, we create a new React Component and then generate/return an Element instance of the component using the config as props.

```javascript
const FormGroup = React.createClass({
  ...
});

return(<FormGroup { ...config } />);
```

We take advantage of the [ES6 spred operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) to pass in our config object to the generated JSX Element. In our `render()` method we create the form group `<div>` and then render out our optional label and Comoponent content.

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

Our `__renderLabel()` method[^2] we use the [Lodash `isString`](https://lodash.com/docs#isString) method to check if the label value is a string. If so we render out our label DOM element, otherwise we return `null`.

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

Finally, we had to add a check to determine what was passed to our HOC function for the Component. This is an important check because we want to support React Components and Elements. 

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

This HOC example is just the tip of the iceberg when it comes to self-generating wrapper components. We can tap into the [Component Life Cycle methods](../life_cycle/introduction.md), we can make more complex deciscions based on the data, we can register to stores or other events, and many other possible combinations.

For more indepth examples we highly recommend reading Dan Abramov's *[Mixins Are Dead. Long Live Composition](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.9y0gg1ix5)* and @franlplant's *[React Higher Order Components in depth](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e#.d38rbnsu8)* 

---

[^2] In these examples we are prefixing our methods with `__` to reflect that these are internal component methods. This is completly optional and is just our preffered style syntax.
 
 