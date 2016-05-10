# The Item Renderer Pattern
 Component reuse and composability are some of the core tenants of React development. As our applications scale, development time can be dramatically reduced through this process. Yet, creating reusable Components takes planning to cover as many use cases as possible.
 
 Understanding the intention of the Compoment is the first step towards resuse. From there we can focus on how to make it more flexible for different scenarios. Once these have been defined we can now start approaching the development process. Let's look at some common examples and how we can apply a design pattern to solve them in React.
 
## The basic List example
 A general reuse pattern is displaying a series of data in a linerar display, i.e a List. UI lists are everywhere in applications today. The list is crucial to Social Media UIs, such as Facebook, Twitter, Reddit, Instagram, etc., etc. The current demo app Todo trend is all about displaying a list of items. The lowly drop-down displays a list of selectable options. It's so common, most of us take lists for granted.
 
 So, when we start building our application how should we approach list and potentially other reusable layouts? Typically, the first approach is to build a React component that renders the UI to the specific layout and data needs. For example, we are building a list of profiles. Each profile has an avatar/headshot and some descriptive text.
 
![A simple profile](react-indepth-avatar-list.png)

The first approach would be to create a Component that takes an Array of Objects that have the image path and the description text. Our Component would then loop over this Array and render out each element, maybe using `<li>` items.

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

We would then apply styling to the `<ul>`, `<li>`