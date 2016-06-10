# Rendering different content
 By moving our UI rendering of each Profile to a Component, we have separated layout and content display. The List is responsible for layout and data management. The Profile is responsible for UI rendering for each individual item. Because of this first step, we can move one step further and make our List even more flexible.
 
## List Feature expansion
 Continuing our customer example, let's image that our Profile List has started to evolve even more. We have added pagination support, selection management, sorting, filtering, etc. Now, our users request the ability to manage a different kind of content. They now want to manage Posts. 
 
 These Posts have some similar UI elements as our profile: images, descriptions, and details. But the layout and content vary drastically. We still need all the of the functionality of the List; pagination, filtering, etc. The question becomes, how do we handle this?
 
## Item Rendering
 A simple, but not ideal, approach would be to add a switch in our List's `map` method. The switch checks the data type and then choose to use the Profile Component or the Post Component. But, this approach adds a pretty bad [code smell](https://en.wikipedia.org/wiki/Code_smell). Similar to our first draft of the List, it meets our immediate needs but what happens when we need a Message List? Or Viewer List? Soon our List has a lot of switches.
 
 A better way to solve this is through configuration. We can expose a prop on the List component that handles rendering of each item. There are two ways to do this: by passing in a function or by passing in a Component Class.
 
### Function Item Renderer
 The first approach we will examine is passing in a function that handles rendering out each individual item in the List. The first step is to update our List component to require a `itemRenderer` prop that is a function and changing our profiles `prop` to items.
 
**List.js**
```javascript
import React from 'react';

class List extends React.Component {
  render() {
    return (
      <ul>
        { this.props.items.map( (item, index) => this.props.itemRenderer(item, index) ) }
      </ul>
    );
  }
}

List.propTypes = {
  items: React.PropTypes.array,
  itemRenderer: React.PropTypes.func.isRequired 
};
List.defaultProps = { items: [] };
export default List;
```

We have added a `propTypes` configuration to require the `itemRenderer` prop, which needs to be a function. We also added an items `prop`, which replaces `profiles`. In our `render()` we now call the function passing in the item instance data and the index. We will talk more about why we need to pass `index` in a bit. In our parent Component or App we now do the following:

**index.js**
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import List from './components/List';
import Profile from './components/Profile';
import Posts from './components/Posts';

let profileData = [ ... ] // psuedo code, this has all our profile data
let postsData = [ ... ] // psuedo code, this has all our post data

class App extends React.Component {
  renderProfile(profile, key) {
    return (<Profile {...profile} key={ key } />);
  }
  
  renderPosts(posts, key) {
    return (<Posts {...post} key={ key } />);
  }
  
  render() {
    return (
      <div>
        <List items={ profileData } itemRenderer={ this.renderProfile } />
        <List items={ postsData } itemRenderer={ this.renderPosts } />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('mount-point'));
```

In `index.js` we render out two different List components. For the first List, we pass in our profile data and our `renderProfile` method reference. Just like any React action (such as `onClick`) we pass the method reference and do not actually call the method. For the second, we pass in the posts data and the `renderPosts` method reference.

When the Lists render, the `map` method calls either `renderProfile()` or `renderPosts()` with each data element and the current index. 

### React keys and arrays of components
 The reason we pass index is that we need to generate a unique key for each item in the list. When we offload rendering to a method, we no longer get React's built in ability to generate the keys for us.

 React Component keys are used for Component Reconciliation: 
 
> Reconciliation is the process by which React updates the DOM with each new render pass...
> 
> ... The situation gets more complicated when the children are shuffled around (as in search results) or if new components are added onto the front of the list (as in streams). In these cases where the identity and state of each child must be maintained across render passes, you can uniquely identify each child by assigning it a key
>
> When React reconciles the keyed children, it will ensure that any child with key will be reordered (instead of clobbered) or destroyed (instead of reused).
> 
> -- [React Child Reconciliation](https://facebook.github.io/react/docs/multiple-components.html#child-reconciliation) 

If we don't set a key when generating children dynamically (via our `itemRenderer` method) we would get the following warning:

> Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of `List`. See https://fb.me/react-warning-keys for more information.

The quick solution is to pass in the index of the data, but this may not be the ideal solution. The problem with this approach is that it generates a key based on item order. It would be better to use an unique `id` that's defined on the data set. Another option is generating a hash code or some other unique identifier that reflects the data element's content.

By having a identifier based on the data content instead of order, we can help optimize the Component rendering. When we display partial lists, such as filtering or sorting, if our key is based on the content and not order, React knows it doesn't have to generate a new Element for the data. It just needs to reorder the elements.

### Component Item Renderer
 Another option for handling dynamic renderers, is to use a Component Class reference. This process is similar to passing in a function. Instead of offloading the rendering to the return value of a method we create a React Element from the Component and pass in the configuration.
 
 **List.js**
```javascript
import React from 'react';
import Profile from './Profile';

class List extends React.Component {
  render() {
    return (
      <ul>
        { this.props.profile.map( (profile, index) => {
            let newProps = Object.assign({ key: index }, profile);
            return React.createElement(this.props.itemRenderer, newProps);
        }) }
      </ul>
    );
  }
}

List.propTypes = { itemRenderer: React.PropTypes.func };
List.defaultProps = { profile: [], itemRenderer: Profile };
export default List; 
```
 
 In this version of the List Component, we create a new React Element using the `this.props.itemRenderer` as the Component Class type. We generate a `newProps` object that adds the `key` to the profile data and pass this to the Element as its `props`.
 
 Because we define a default item renderer of `Profile` in the `defaultProps` we can update `propTypes` to make `itemRenderer` an optional param. To use this version of the List our index.js now looks like this:
 
**index.js**
 ```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import List from './components/List';
import Profile from './components/Profile';
import Post from './components/Post';

let profileData = [ ... ] // psuedo code, this has all our profile data
let postsData = [ ... ] // psuedo code, this has all our post data

class App extends React.Component { 
  render() {
    return (
      <div>
        <List items={ profileData } />
        <List items={ postsData } itemRenderer={ Post } />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('mount-point'));
```

Since we have a default item renderer (the Profile Component), the first version of the List just needs the profile data. The second version, we change out the renderer type by passing in our Component and pass in the item data.

When our List renders the data it now creates a React Element from the `itemRenderer` value and passes in the current data element. At [DevelopmentArc](http://developmentarc.com), we have found using a React Class is a much cleaner approach to developing replaceable UI elements.

 ***Up Next***: [Higher Order Components](patterns/higher_order_components.md)