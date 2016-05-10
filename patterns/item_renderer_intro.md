# The Item Renderer Pattern
 Component reuse and composability are some of the core tenants of React development. As our applications scale, development time can be dramatically reduced through this process. Yet, creating reusable Components takes planning to cover as many use cases as possible.
 
 Understanding the intention of the Compoment is the first step towards resuse. From there we can focus on how to make it more flexible for different scenarios. Once these have been defined we can now start approaching the development process. Let's look at some common examples and how we can apply a design pattern to solve them in React.
 
## The List example
 A common challenge is displaying different data in similar ways. The most common example is a list of items. UI lists are everywhere in applications today. The list is crucial to Social Media UIs, such as Facebook, Twitter, Reddit, Instagram, etc., etc. The current demo app Todo trend is all about displaying a list of items. It's so common, most of us take lists for granted.
 
 So, when we start building our application how should we approach list and other reusable layouts? Typically, the first approach is to build a React component that renders the UI to the specific layout and data needs.
 
