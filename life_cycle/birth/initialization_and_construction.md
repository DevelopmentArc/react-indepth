# Initialization & Construction
 During the initialization of the Component from the Element, the `prop`s and `state` are defined. How these values are defined depends on if you are using `React.createClass()` or `extend React.Component`. Let's first look at `props` and then we will examine `state`.

## Default Props
As we mentioned earlier, the Element instance contains the current `props` that are being passed to Component instance. Most of the time, all the available `props` on the Component are not required. Yet, some times we do need to have values for all the `props` for our Component to render correctly.

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
When using default props, it is important to under how the React merge process works. Often, we are generating props dynamically based on application state ([Flux](https://facebook.github.io/flux/), [Redux](http://redux.js.org/), [Mobx](https://github.com/mobxjs/mobx), etc.). This means that we can sometimes generate `null` values and pass this as the prop.

When assigning default props, the React object merge code sees `null` as a defined value.

```javascript
<Person name="Bob" age={ null } />
```

Because `null` is a defined value our Component would render this as `<div>Bob (age:)</div>` instead of rendering *unknown*. But, if we pass in `undefined` instead of `null`, React treats this as undefined (well yeah, obviously) and we would render *unknown* as expected.

Keep this in mind when defining default props, because tracing down a `null` value can be tricky in larger application.

## Initial State
 Once the final props are defined (passed w/ defaults), the Component instance configures the initial `state`. This process occurs in the construction of the instance itself. Unlike props, the Component state is an internal object that is not defined by outside values.
 
 To define the initial `state` depends on how you declare your Component. For ES6 we declare the state in the constructor. Just like `defaultProps`, the initial state takes an object.
 
 **For ES6 Class**
```javascript
import React from 'react';

class Person extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  render() {
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
}

Person.defaultProps = { age: 'unknown' };

export default Person;
```

For `React.createClass` Components, there is a helper method called `getInitialState()` which returns the state object. This method is called during setup to set the state on the instance.

**For createClass (ES6/ES5/CoffeeScript, etc.)**

```javascript
var Person = React.createClass({
  getDefaultProps: function() {
    return ({ age: 'unknown' });
  },
  
  getInitialState: function() {
    return ({ count: 0 });
  },
  
  render: function() {
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
});
```

### State defaults
 It is important to keep in mind that if we do not define a state in the constructor/getInitialState then the state will be `undefined`. Because the state is `undefined` and not an empty Object (`{}`), if you try to query the state later on this will be an issue.
 
 In general, we want to set a default value for all our state properties. There are some edge cases where the initial value for the state property may be `null` or `undefined`. If this state happens to be only state property, it maybe tempting to skip setting a default state. But, if our code tries to access the property you will get an error.
 
 ```javascript
 class Person extends React.Component {
  render() {
    // This statement will throw an error
    console.log(this.state.foo);
    return (
      <div>{ this.props.name } (age: { this.props.age })</div>
    );
  }
}
 ```

The log statement fails because `this.state` is undefined. When we try to access `foo` we will get a *TypeError: Cannot read property 'foo' of null*. To solve this we can either set the default state to `{}` or, to have a clearer intention, set it to `{ foo: null }`.

***Next Up:*** [Pre-mounting with `componentWillMount()`](premounting_with_componentwillmount.md)