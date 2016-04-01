# The React Life Cycle
 Generally, most life forms go through a life cycle. The common path is birth, growth into maturity and then the inevitable death. UI applications follow a similar path. The application is first started, we consider this Birth into Growth. The users interacts with application, Growth into Maturity. Eventually, the application is closed or navigated away from, which is leads to Death.
 
 Within the application, elements also follow this pattern. In the world of React, these elements are our components. The component life cycle is a continuous process, which occurs throughout the overall life cycle of our application. Understanding this process can lead to faster development, easier optimization and overall application health.
 
 ## Life cycle phases in React
 
 ## React life cycle methods
  Not all UI systems implement or follow the life cycle concept. Some systems implement there own process or use different concepts. This doesn't make libraries that use a life cycle better or worse, it just defines the process we can take advantage of.
  
  Because React does follow this pattern, the development team provides a series of hooks we can tap into. These method hooks inform us of where the component is in the life cycle and what we can and cannot do.
  
  Each of the life cycle methods are called in a specific order and at a specific time. The methods are also tied to different parts of the Life Cycle. Here are the methods broken down in order and by their corresponding life cycle phase [^1]:
  
  ** Birth / Creation **
  1. Initialize / Construction
  2. `getDefaultProps()` *(React.createClass)* or `MyComponent.defaultProps` *(ES6 class)*
  3. `getInitialState()` *(React.createClass)* or `this.state = ...` *(ES6 constructor)*
  4. `componentWillMount()`
  5. `render()`
  6. Children initialization & life cycle kickoff
  7. `componentDidMount()`
  
[^1] *Most of the methods are the same if you use either `React.createClass` or using ES6 classes, such as `class MyComponent extends React.Component`. A few are different, mainly around how instantiation/creation occurs. We will call these differences out throught the chapter.*

