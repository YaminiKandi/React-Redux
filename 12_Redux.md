#### Integrating Redux with a UI:
Using Redux with any UI layer requires the same consistent set of steps:
1. Create a Redux store
2. Subscribe to updates
3. Inside the subscription callback:
    - Get the current store state
    - Extract the data needed by this piece of UI
    - Update the UI with the data
4. If necessary, render the UI with initial state
5. Respond to UI inputs by dispatching Redux actions

#### Why to use redux:
* To Manage the state of our application in a predictable way.

#### Install Redux Toolkit and React Redux
`npm install redux react-redux @reduxjs/toolkit`

#### Create a Redux store:
```js
// Index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import {configureStore} from "@reduxjs/toolkit"
import { Provider } from 'react-redux';
import { userReducer } from './features/user';

const store = configureStore({
  reducer:{
    user: userReducer,
  }
})
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </Provider>
);
reportWebVitals();
```
* reducer - a function that takes the information about current states i.e., previous state and performs some action and returns the newvalue of that state
* This is how we manage the state of our application in redux


**OR**

* We can create a file named src/app/store.js. Import the configureStore API from Redux Toolkit. We'll start by creating an empty Redux store, and exporting it
```js
// app/store.js
import { configureStore } from '@reduxjs/toolkit'
export default configureStore({
  reducer: {},
})
```

* And Providing the redux store to react

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import './index.css'
import App from './App'
import store from './app/store'
import { Provider } from 'react-redux'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <React.StrictMode>
      <App />
    </React.StrictMode>
  </Provider>
);
reportWebVitals();
```

#### Creating a redux state slice:
* Creating a slice requires a string name to identify the slice, an initial state value, and one or more reducer functions to define how the state can be updated. 
* Once a slice is created, we can export the generated Redux action creators and the reducer function for the whole slice.
* Redux requires that we write all state updates immutably, by making copies of data and updating the copies. 
* However, Redux Toolkit's `createSlice` and `createReducer` APIs use Immer inside to allow us to write "mutating" update logic that becomes correct immutable updates.

```js
// user.js
import { createSlice } from "@reduxjs/toolkit";

export const userSlice = createSlice({
    name: 'user', // name of the slice ~ name of the state
    initialState: { value: {name:"", age: 0, email:''}}, // empty user
    reducers: {
        login: (state, action) => {
        // state holds the information about the current and previous value
            state.value = action.payload
        }
    }
    // functions we want to use in the application
    // appears in reducers
})

export default userSlice.reducer
```
```js
// state
(parameter) state: WritableDraft<{
    value: {
        name: string;
        age: number;
        email: string;
    };
}>

// action
(parameter) action: {
    payload: any;
    type: string;
}
```
* payload - object, where we pass the information we use when changing the state

#### Add Slice Reducers to the store:
* we need to import the reducer function from the user slice and add it to our store.
* By defining a field inside the reducer parameter, we tell the store to use this slice reducer function to handle all updates to that state.

```js
const store = configureStore({
  reducer:{
    user: userReducer,
  }
})
```

#### Use Redux State and Actions in React component:
* Now we can use the React Redux hooks to let React components interact with the Redux store. 
* We can read data from the store with useSelector, and dispatch actions using useDispatch.

useSelector - used for accessing values of our states
useDispatch - used to modify values of our states


#### Summary:
1. Create a Redux store with configureStore
    - configureStore accepts a reducer function as a named argument
    - configureStore automatically sets up the store with good default settings

2. Provide the Redux store to the React application components
    - Put a React Redux `<Provider>` component around your `<App />`
    - Pass the Redux store as `<Provider store={store}>`

3. Create a Redux "slice" reducer with `createSlice`
    - Call `createSlice` with a string name, an initial state, and named reducer functions
    - Reducer functions may "mutate" the state using Immer
    - Export the generated slice reducer and action creators

4. Use the React Redux `useSelector/useDispatch` hooks in React components
    - Read data from the store with the `useSelector` hook
    - Get the dispatch function with the `useDispatch` hook, and dispatch actions as needed
