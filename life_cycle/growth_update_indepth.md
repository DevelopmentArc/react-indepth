# Growth/Update In-depth
 Once our Component is mounted in the Birth phase, we are prepped and ready for the Growth/Update phase. The Growth phase is where a component spends most of its time. Here we get data updates, act on user or system actions and provide the overall experience for our application.
 
 The Growth phase is triggered in three different ways: changing of `props`, changing of `state` or calling `forceUpdate()`. Depending on which change is made, will determine how we enter the different update life cycle methods and functionality.
 
 In this Section, we will dive into the different methods. We'll look at how and when they are triggered. We will also discuss what tasks are best handled during each method and possible gotchas that could lead to performance issues.
 
 ## Starting Update: Changing Props
  As mentioned earlier, we have three ways to start the Growth/Update phase. The first way is when the components `props` are changed. This occurs when either the root Element (ex: `ReactDOM.render(<MyComponent data={ dataVaule } />, ...)` has the props value change, or the parent of the Component's prop changes.
  
  From a Component's instance perspective (such as `<Person name="Bill" />`), the passed in props are immutable. In other words, the Person instance cannot update the value name internally. In fact, if you try you will get an Error in React.
  
  ```javascript
  render() {
    // DON'T DO THIS!
    this.props.name = 'Tim';
    return (
      <div className={ classNames('person', this.state.mode) }>
        { this.props.name } (age: { this.props.age })
      </div>
    );
  }
  ```
  
> TypeError: Cannot assign to read only property 'name' of object '#&lt;Object&gt;'

When new props are passed in via root or the parent, this triggers the Update phase.

## Starting Update: Changing State
 Similar to changing `props`, when a Component changes its state value via `this.setState()`, this also triggers a new Update phase. For a lot of React developers, the first major (and to be honest ongoing) challenge is understanding and managing state in Components. State itself can be a controversial topic in the community. Many developers avoid state at all cost. Other systems, such as [MobX](http://mobxjs.github.io/mobx/), is in essence trying to replace it. Many uses of state can fall into the anti-pattern, such as transferring `props` into `state` [^1].
 
 Fundamentally, state can be a tricky and confusing topic. When do we use state? What data should or shouldn't be stored in state? Should we even use state at all? To be honest, this is a topic that we are still trying to grapple with ourselves. 
 
 Keeping that in mind, it is still important to understand how state works in React. We will continue to discuss state in-depth and how the mechanics work. We will try to share best practices that we have found, but in general what is good today will probably be bad tomorrow.
 
 ### The asynchronicity of state
 Before we move on to the final way to start an update, we should talk a little about how state is managed in the underpinnings of React. When developers first start using `setState()` there is an assumption that when you query `this.state` the values applied on the set call will be available. This is not true. The `setState()` method should be treated as an asynchronous process [^2].
 
 When we call `setState()` this is considered a partial state change. We are not flushing/replacing the entire state, just updating part(s) of it. React uses a queuing system (React internal method `enqueueSetState()` to be exact) to apply the partial state change. Because we may set the state multiple times in a method chain, a change queue is built up. Once the state change is added to the queue, React makes sure the Component is added to the dirty queue. This dirty queue tracks the Component instances that have changed. Essentially, this is what tells React which Components need to enter the Update phase later.
 
 When working with state, it is very important to keep this in mind. A common error is to set state in one method and then later in the same synchronous method chain try to access the state value.
 
## Starting Update: `forceUpdate`
 There is one more way to kick off an Update phase. There is a special method on a component called `forceUpdate()`. This does exactly what you think, it forces the Component into an Update phase. The `forceUpdate()` method has some specific ramifications about how the life cycle methods are processed and we will discuss this later on.

 
 Up Next: [Updating and `componentWillReceiveProps()`](update/component_will_receive_props.md)
 
 ---
 
 [^1] Even though moving `props` to `state` [is considered an anti-pattern](https://facebook.github.io/react/tips/props-in-getInitialState-as-anti-pattern.html), there are a few use cases. The most common is having a `defaultValue` prop that becomes the internal `value` in state. We see this pattern with most Form elements in React. All though, there is a strong movement to get away from this and work with only [Controlled Components](https://facebook.github.io/react/docs/forms.html#controlled-components).
 
 [^2] The React code comments recommend that *"... You should treat `this.state` as immutable. There is no guarantee that `this.state` will be immediately updated, so accessing `this.state` after calling [the setState] method may return the old value."*
 