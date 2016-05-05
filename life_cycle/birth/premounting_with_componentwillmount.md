# Pre-mounting with `componentWillMount()`
 Now that the props and state are set, we finally enter the realm of Life Cycle methods. The first true life cycle method called is `componentWillMount()`. This method is only called one time, which is before the initial render. Since this method is called before `render()` our Component will not have access to the Native UI (DOM, etc.). We also will not have access to the children `refs`, because they are not created yet.

The `componentWillMount()` is a chance for us to handle configuration, update our state, and in general prepare for the first render. At this point, props and initial state are defined. We can safely query `this.props` and `this.state`, knowing with certainty they are the current values. This means we can start performing calculations or processes based on the prop values.

**Person.js [^1]**
```javascript
import React from 'react';
import classNames from 'classnames';

class Person extends React.Component {
  constructor(props) {
    super(props);
    this.state = { mode: undefined } ;
  }

  componentWillMount() {
    let mode;
    if (this.props.age > 70) {
      mode = 'old';
    } else if (this.props.age < 18) {
      mode = 'young';
    } else {
      mode = 'middle';
    }
    this.setState({ mode });
  }

  render() {
    return (
      <div className={ classNames('person', this.state.mode) }>
        { this.props.name } (age: { this.props.age })
      </div>
    );
  }
}

Person.defaultProps = { age: 'unknown' };

export default Person;
```

In the example above we call `this.setState()` and update our current state before render. If we need state values on calculations passed in `props`, this is where we should do the logic. 

Other uses for `componentWillMount()` includes registering to global events, such as a Flux store. If your Component needs to respond to global Native UI events, such as `window` re-sizing or focus changes, this is a good place to do it[^2].

***Next Up:*** [Component `render()`](component_render.md)

---
[^1] In our example above, we are use the `classNames()` library, which was originally included as a React Addon. However, the feature has been removed from React and moved to it's [own library](https://github.com/JedWatson/classnames) for use with or without React.

[^2] It's important to remember that many Native UI elements do not exist at this point in the life cycle. That means we need to stick to very high-level/global events such as `window` or `document`.