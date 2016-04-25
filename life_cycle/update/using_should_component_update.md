# Using `shouldComponentUpdate()`
 The next method in the Update life cycle, is `shouldComponentUpdate()`. This method allows your Component to exit the Update life cycle if there is no reason to apply a new render. Out of box, the `shouldComponentUpdate()` is a no-op that returns `true`. This means every time we start an Update in a Component, we will re-render.
 
 If you recall, React does [not deeply compare `props`](https://facebook.github.io/react/blog/2016/01/08/A-implies-B-does-not-imply-B-implies-A.html) by default. When `props` or `state` is updated React assumes we need to re-render the content. But, if the `props` or `state` have not changed, should we really be re-rendering?
 
## Preventing unnecessary renders
 The `shouldComponentUpdate()` method is the first real life cycle optimization method that we can leverage in React. We can look at our current and new `props` & `state` and make a choice if we should move on. [React's PureRenderMixin](https://facebook.github.io/react/docs/pure-render-mixin.html) does exactly this. It checks the current props and state, compares it to the next props and state and then returns `true` if they are different, or `false` if they are the same.
 
 ```javascript
/**
 * Performs equality by iterating through keys on an object and returning false
 * when any key has values which are not strictly equal between the arguments.
 * Returns true when the values of all keys are strictly equal.
 */
function shallowEqual(objA: mixed, objB: mixed): boolean {
  if (objA === objB) {
    return true;
  }

  if (typeof objA !== 'object' || objA === null ||
      typeof objB !== 'object' || objB === null) {
    return false;
  }

  var keysA = Object.keys(objA);
  var keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }

  // Test for A's keys different from B.
  var bHasOwnProperty = hasOwnProperty.bind(objB);
  for (var i = 0; i < keysA.length; i++) {
    if (!bHasOwnProperty(keysA[i]) || objA[keysA[i]] !== objB[keysA[i]]) {
      return false;
    }
  }

  return true;
}

function shallowCompare(instance, nextProps, nextState) {
  return (
    !shallowEqual(instance.props, nextProps) ||
    !shallowEqual(instance.state, nextState)
  );
}

var ReactComponentWithPureRenderMixin = {
  shouldComponentUpdate: function(nextProps, nextState) {
    return shallowCompare(this, nextProps, nextState);
  },
};
 ```
 
The above code is extracted from the React addon/source[^1]. The mixin defines the `shouldComponentUpdate(nextProps, nextState)` and compares the instance's `props` against the `nextProp` and the `state` against the `nextState`.

### Mutability and pure methods
 It is important to note that the `shallowCompare` method simply uses `===` to check each instance. This is why the React team calls the mixin *pure*, because it will not properly check against mutable data.
 
```javascript
// psuedo code
this.setState({ data: [1, 2, 3] });

<MyComponent data={ this.state.data } />
```

Let's think back to our `data` props Array example where we use `push()` to add a new piece of data onto the Array.

The `shallowCompare` will see the current `props.data` as the same instance as the `nextProps.data` (`props.data === nextProps.data`) and therefore not render an update. Since we mutated the `data` Array, our code is not considered to be *pure*.

This is where systems like [Redux](http://redux.js.org/) requires pure methods for reducers. If you need to change nested data you have to clone the objects and make sure a new instance is always returned. This allows for `shallowCompare()` to see the change and update the component.

Other ways to handle this is to use an immutable data system, such as [Immutable.js](https://facebook.github.io/immutable-js/). These data structures prevent developers from accidentally mutating data. By enforcing immutable data structures, we can leverage `shouldComponentUpdate()` and have it verify of our `props` and `state` have changed[^2].

### Stop renders at the source
 If you recall our nested Component structure:
 
 ![](../birth/react-element-tree.png)
 
 By default, if an Update is triggered in **A**, then all the other children will also go through their updates. This can easily cause a performance issue, because now we have many Components going through each step of the process.
 
 By adding logic checks in `shouldComponentUpdate()` at **A** we can prevent all its children from re-rendering. This can improve general performance significantly. But keep in mind, if you prevent **A** from passing props down to the children you may prevent required renders from occurring.
 
## Jump ahead with `forceUpdate()`
 Like `componentWillReceiveProps()`, we can skip `shouldComponentUpdate()` by calling `forceUpdate()` in the Component. This sets a flag on the Component when it gets added to the dirty queue. When flagged, `shouldComponentUpdate()` is ignored. Because we are forcing an update we are stating something has changed and the Component *must* re-render.
 
 Since `forceUpdate()` is a brute force method, it should always be used with caution and careful consideration. You can easily get into an endless render loop if you keep triggering `forceUpdate` over and over. Troubleshooting infinite render loops can be very tricky. So, when reaching for `forceUpdate` keep all this in mind.
 
***Next Up:*** [Tapping into `componentWillUpdate()`](tapping_into_componentwillupdate.md)

---

[^1] Captured from [React 15.0.1](https://github.com/facebook/react/blob/15-stable/src/addons/shallowCompare.js)

[^2] If you use Immutable.js, there is a [ImmutableRenderMixin library](https://github.com/jurassix/react-immutable-render-mixin) that provides both Object shallow compare and Immutable data comparison.