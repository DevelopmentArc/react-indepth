# Growth/Update In-depth
 Once our Component is mounted in the Birth phase, we are prepped and ready for the Growth/Update phase. The Growth phase is where a component spends most of its time. Here we get data updates, act on user or system actions and provide the overall experience for our application.
 
 The Growth phase is triggered in three different ways: changing of `props`, changing of `state` or calling `forceUpdate()`. Depending on which change is made, will determine how we enter the different update life cycle methods and functionality.
 
 In this Section, we will dive into the different methods. We'll look at how and when they are triggered. We will also discuss what tasks are best handled during each method and possible gotchas that could lead to performance issues.
 
 ## Triggering Update
  As mentioned earlier, we have three ways to start the Growth/Update phase. The first way is when the components `props` are changed. 
 
 