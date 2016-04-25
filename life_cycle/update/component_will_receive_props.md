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

Any time the user types into the `<input />` this triggers an Update for the Person component.