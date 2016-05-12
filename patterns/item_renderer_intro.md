# Component Composition and Renderers
 Component reuse and composability are some of the core tenants of React development. As our applications scale, development time can be dramatically reduced through this process. Yet, creating reusable Components takes planning to cover as many use cases as possible.
 
 Understanding the intention of the Component is the first step towards reuse. From there we can focus on how to make it more flexible for different scenarios. Once these have been defined we can now start approaching the development process.
 
## The evolution of a List Component
 A general reuse pattern is displaying a series of data in a linear display, i.e a List. UI lists are everywhere in applications today. The list is crucial to Social Media UIs, such as Facebook, Twitter, Reddit, Instagram, etc. The current demo app trend of Todos are all about displaying a list of items. The lowly drop-down displays a list of select-able options. It's so common, most of us take lists for granted.
 
 So, when we start building our application how should we approach creating lists and potentially other reusable layouts? Let's walk through a possible progression of a list feature.
 
### The first pass
 Typically, the first approach is to build a React component that renders the UI to the specific layout and data needs. For our example, we are building a list of user profiles. The first design round requires the profile to have an avatar/picture and descriptive text.
 
![A simple profile](react-indepth-avatar-list.png)

The first approach would be to create a Component that takes an Array of Objects, which has an image path and the description text. Our Component would then loop over this Array and render out each element, using `<li>` items.

```javascript
import React from 'react';

class List extends React.Component {
  renderProfiles () {
    return this.props.profile.map( (profile) => {
      return (
        <li>
          <img href={ profile.imagePath } align="left" width="30" height="30" />
          <div className="profile-description">
            { profile.description }
          </div>
        </li>
      );
    });
  }

  render() {
    return (<ul className="profile-list">{ this.renderProfiles() }</ul>);
  }
}

List.defaultProps = { profile: [] };
export default List;
```

We would then apply styling to the `<ul>`, `<li>` and `<div>` elements to meet our design needs.