# Updating and `componentWillReceiveProps()`
 Now that we have discussed starting an Update, let's dive into the Update life cycle methods. The first method available to us `componentWillReceiveProps()`. This method is called when `props` are passed to the Component instance. Let's dig a little deeper into what this means.
 
 ## Passing `props`
 The most obvious example is when new props are passed to a Component. For example, we have Form Component and a Person Component. The Form Component has a single `<input />` that allows the user to change the name by typing into the input. The input is bound to the `onChange` event and sets the state on the Form. The state is then is passed to the Person component as a `prop`.
  
***Form.js***
```javascript
import React from 'react';
import Person from './Person';

export default class Form extends React.Component {
  constructor(props) {
    super(props);
    this.state        = { name: '' } ;
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    this.setState({ name: event.currentTarget.value });
  }

  render() {
    return (
      <div>
        <input type="text" onChange={ this.handleChange } />
        <Person name={ this.state.name } />
      </div>
    );
  }
}
```

 Any time the user types into the `<input />` this begins an Update for the Person component. The first method called on the Component is `componentWillReceiveProps(nextProps)` passing in the new `prop` value. This allows us to compare the incoming props against our current props and make logical decisions based on the value. We can get our current props by calling `this.props` and the new value is the `nextProps` argument passed to the method.
 
### Updating State
 So why do we need `componentWillReceiveProps`? This is the first hook that allows us to look into the incoming Update. Here we could extract the new props and update our internal state. If we have a state that is a calculation of multiple props, we can safely apply the logic here and store the result using `this.setState()`.
 
> Use this as an opportunity to react to a prop transition before render() is called by updating the state using this.setState(). The old props can be accessed via this.props. Calling this.setState() within this function will not trigger an additional render.
> 
> -- https://facebook.github.io/react/docs/component-specs.html#updating-componentwillreceiveprops


### Props may not change
 A word of caution with `componentWillReceiveProps()`. Just because this method was called, does not mean the value of props has changed.
  
> To understand why, we need to think about what could have happened. The data could have changed between the initial render and the two subsequent updates ... React has no way of knowing that the data didn’t change. Therefore, React needs to call `componentWillReceiveProps`, because the component needs to be notified of the new props (even if the new props happen to be the same as the old props).
>  
>  -- See [(A =&gt; B) !=&gt (B =&gt A)](https://facebook.github.io/react/blog/2016/01/08/A-implies-B-does-not-imply-B-implies-A.html) 

The core issue with `props` and `componentWillReceiveProps()` is how JavaScript provides mutable data structures. Let's say we have a prop called `data` and data is an Array. 

```javascript
// psuedo code
this.setState({ data: [1, 2, 3] });

<MyComponent data={ this.state.data } />
```

If somewhere in our app, a process updates the data array via `push()`, this changes the content of the data Array. Yet, the Array itself is the same instance. Because it is the same instance, React can not easily determine if the internal data has changed. So, to prevent a lot of issues, or having to do deep comparisons, React will push the same props down.

With this being said, `componentWillReceiveProps()` allows us to check and see if new props are coming in and we can make choices based on the data. We just need to make sure we never assume the props are different in this method. Be sure to read the great post [(A =&gt; B) !=&gt (B =&gt A)](https://facebook.github.io/react/blog/2016/01/08/A-implies-B-does-not-imply-B-implies-A.html) by Jim Sproch.

### Skipping this method
 Unlike our methods in the Mounting phase, not all our Update phase are called every time. For example, we will skip `componentWillReceiveProps()` if the Update is triggered by a set state. Going back to our Form.js example above:
 
 ```javascript
 handleChange(event) {
    this.setState({ name: event.currentTarget.value });
  }

  render() {
    return (
      <div>
        <input type="text" onChange={ this.handleChange } />
        <Person name={ this.state.name } />
      </div>
    );
  }
 ```

When the user types in the `<input />` we trigger a `setState()` method. This will trigger an Update phase in our Form Component and the Person Component.
