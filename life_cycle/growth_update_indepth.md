# Growth/Update In-depth
 Once our Component is mounted in the Birth phase, we are prepped and ready for the Growth/Update phase. The Growth phase is where a Component spends most of its time. Here we get data updates, act on user or system actions and provide the overall experience for our application.
 
 The Growth phase is triggered in three different ways: changing of `props`, changing of `state` or calling `forceUpdate()`. The changes that are made affect how the Update phase is managed. We will discuss each of these changes in depth as we walk through the entire Growth process.
 
 In this Section, we will dive into the different methods. We'll examine the order of the methods called and how they affect the overall process. We will also discuss what tasks are best handled during each method and discuss application optimization.
 
 ## Starting Update: Changing Props
  As mentioned earlier, we have three ways to start the Growth/Update phase. The first way is when the components `props` update. This occurs when either the root Element (ex: `ReactDOM.render(<MyComponent data={ dataVaule } />, ...)` has the `props` value changed or the parent of the Component's `prop` changes.
  
  From a Component's instance perspective (such as `<Person name="Bill" />`) the passed in props are immutable. In other words, the Person instance cannot update the value name internally. In fact, if you try you will get an Error in React.
  
  ```javascript
  render() {
    // BAD: DON'T DO THIS!
    this.props.name = 'Tim';
    return (
      <div className={ classNames('person', this.state.mode) }>
        { this.props.name } (age: { this.props.age })
      </div>
    );
  }
  ```
  
> TypeError: Cannot assign to read only property 'name' of object '#&lt;Object&gt;'

Because props are immutable by the Component itself, the parent must provide the new values. When new props are passed in via root or the parent, this starts the Update phase.

## Starting Update: `setState()`
 Similar to changing `props`, when a Component changes its state value[^1] via `this.setState()`, this also triggers a new Update phase. For a lot of React developers, the first major (and to be honest ongoing) challenge is managing state in Components. State itself can be a controversial topic in the community. Many developers avoid state at all cost. Other systems, such as [MobX](http://mobxjs.github.io/mobx/), are in essence trying to replace it. Many uses of state can fall into different anti-patterns, such as transferring `props` into `state`[^2].
 
 Fundamentally, state can be a tricky and confusing topic. When do we use state? What data should or shouldn't be stored in state? Should we even use state at all? To be honest, this is a topic that we are still trying to grapple with ourselves.
 
 Keeping that in mind, it is still important to understand how state works in React. We will continue to discuss state in-depth and how the mechanics work. We will try to share best practices that we have found, but in general what is good today will probably be bad tomorrow.
 
 ### The asynchronicity of state
 Before we move on to the final way to start an update, we should talk a little about how state is managed in the internals of React. When developers first start using `setState()` there is an assumption that when you call `this.state` the values applied on the set will be available. This is not true. The `setState()` method should be treated as an asynchronous process [^3]. So how does `setState()` work?
 
 When we call `setState()` this is considered a partial state change. We are not flushing/replacing the entire state, just updating part(s) of it. React uses a queuing system[^4] to apply the partial state change. Because we can set the state multiple times in a method chain, a change queue is constructed to manage all the various updates. Once the state change is added to the queue, React makes sure the Component is added to the dirty queue. This dirty queue tracks the Component instances that have changed. Essentially, this is what tells React which Components need to enter the Update phase later.
 
 When working with state, it is very important to keep this in mind. A common error is to set state in one method and then later in the same synchronous method chain try to access the state value. This can sometimes cause tricky bugs, especially if you expose state values via public methods on your Component, such as `value()`. We will talk later about when `this.state` is finalized.
 
## Starting Update: `forceUpdate`
 There is one more way to kick off an Update phase. There is a special method on a Component called `forceUpdate()`. This does exactly what you think, it forces the Component into an Update phase. The `forceUpdate()` method has some specific ramifications about how the life cycle methods are processed and we will discuss this in-depth later on.

 
 Up Next: [Updating and `componentWillReceiveProps()`](update/component_will_receive_props.md)
 
 ---
 
 [^1] With Component state, we consider this internal functionality. In theory, we can access and even edit state outside of the instance but this is an anti-pattern. Accessing a Component's state from outside injects a lot of fragility into the system (pathing dependency, changing of internal values, etc.). Only a Component instance should `setState()` on itself. 
 
 [^2] Even though moving `props` to `state` [is considered an anti-pattern](https://facebook.github.io/react/tips/props-in-getInitialState-as-anti-pattern.html) there are a few use cases. The most common is having a `defaultValue` prop that becomes the internal `value` in state. We see this pattern with most Form elements in React. Although, there is a strong movement to get away from this and work with only [Controlled Components](https://facebook.github.io/react/docs/forms.html#controlled-components).
 
 [^3] The React code comments recommend that *"... You should treat `this.state` as immutable. There is no guarantee that `this.state` will be immediately updated, so accessing `this.state` after calling [the setState] method may return the old value."*
 
 [^4] React internal method `enqueueSetState()` to be exact.
 