### Redux:
* Redux is a `predictable` `state container` for `javascript apps`
* A state management system for cross-component or app-wide state

#### What is cross-component or app-wide state:
* Generally 3 types of state:
    1. Local State
    2. cross-component state
    3. app-wide state

* Local State:
    - State that belongs to a single component
    - Eg: listening to user input in a input field, toggling a showmore details field
    - Should be managed component-interval with useState() / useReducer()

* cross-component state:
    - State that affects multiple components
    - Eg: open/closed state of a modal overlay
    - We can manage them with useState() / useReducer() but they require 'prop chains/prop drilling'

* app-wide state:
    - State that affects the entire app (most/all components)
    - Eg: user authentication status
    - Requires 'prop chains/prop drilling'

#### Redux Vs React Context:
* React context aslo deals with multiple components
* Then why do we need Redux?
    - React Context has potential disadvantages
    - We can use both in single application

* Disadvantages:
    - we cant have complex setup / management (In more complex apps, managing React Context can lead to deeply nested JSX code and/or huge `Context Provider` components)
    - performance (React Context is not optimized for high frequency state changes)

#### How Redux Works?
* Redux means having one `Central Data(state) Store`
* We have data in that store, so that we can use it from inside our components
* Components dont directly manipulate the data in the store, instead we use a concept called reducers i.e., reducer function which mutates the store data
* We connect the components and reducers by trigger, we have actions and components dispatch the actions
* An action is a simple javascript object, which describes the kind of operation the reducers should perform. 

#### Redux is for javascript apps:
* Redux is not tied to react
* It can be used with react, angular, vue or even vanilla javascript
* Redux is a library for javascript applications

#### Redux is predictable:
* React is a state container, the state of an application can change
* Example - todolist app - item (pending) -> item (completed)
* In redux, all state transitions are explicit and it is possible to keep track of them 
* The changes to our application's state become predictable

#### Redux is a state container:
* Redux stores the state of our application

```js
// Example
// LoginFormComponent
state = {
    username: '',
    password: '',
    submitting: false,
}
// UserListComponent
state = {
    users = [],
}
```
* State of an app is the state represented by all the individual components of that app.
* Redux will store and manage the application state
```js
// Application
state = {
    isUserLoggedIn: true,
    username: 'Yamini',
    profileUrl: '',
    onlineUsers: [],
    isModalOpened: false,
}
```

