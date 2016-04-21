# Initialization & Construction
 During the initialization of the component instance from the Element, the props and state will be created. How these values are defined depends on if you are using `React.createClass()` or `extend React.Component`. Let's first look at `props` and then we will examine `state`.

## Default Props
As we mentioned earlier, the Element instance contains the current props that are being passed to component instance. Most of the time, all the available props on the component are not required. Yet, we do need to have values for the props for our Component to render correctly.

For example, we have a simple component that renders a name and age.

```javascript
import React from 'react';

export default class Person extends React.Component {
  render() {
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
}
```


In our case, we expect two props to by passed in: `name` and `age`. If we want to make `age` optional and default to the text 'unknown' we can take advantage of React's default props. 

**For ES6 Class**
```javascript
import React from 'react';

class Person extends React.Component {
  render() {
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
}

Person.defaultProps = { age: 'unknown' };

export default Person;
```

**For createClass (ES6/ES5/CoffeeScript, etc.)**

```javascript
var Person = React.createClass({
  getDefaultProps: function() {
    return ({ age: 'unknown' });
  },
  
  render: function() {
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
});
```

The result of either process is the same. If we create a new instance without setting the age prop ex: `<Person name="Bill" />`, the component will render `<div>Bill (age: unknown)</div>`.

React handles default props by merging the passed props object and the default props object. This process is similar to [`Object.assign()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) or the Lodash/Underscore [`_.assign()`](https://lodash.com/docs#assign) method. The default props object is the target object and the passed props is the source:

```javascript
// React library code to extract defaultProps to the Constructor
if (Constructor.getDefaultProps) {
   Constructor.defaultProps = Constructor.getDefaultProps();
}

// psuedo code (as an example)
this.props = Object.assign(Constructor.defaultProps, elementInstance.props);
```

In the React code snippet, React checks the underlying Class instance to see if it defines `getDefaultProps()` and uses this to set the values. When using ES6 classes we just define it on the class itself. Any property defined on the `passedProps` value is applied/overridden to the property in the default props object.

### `null` vs. `undefined` props
When using default props, it is important to under how the React merge process works. Often, we are generating props dynamically based on application state (Flux, Redux, Mobx, etc.). This means that we can sometimes generate `null` values and pass this as the prop.

When assigning default props, the React object merge code sees `null` as a defined value.

```javascript
<Person name="Bob" age={ null } />
```

Because `null` is a defined value our Component would render this as `<div>Bob (age:)</div>` instead of rendering *unknown*. But, if we pass in `undefined` instead of `null`, React treats this as undefined (well yeah, obviously) and we would render *unknown* as expected.

Keep this in mind when defining default props, because tracing down an `null` value can be tricky in larger application.