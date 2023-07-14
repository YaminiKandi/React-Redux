```js
// App.js
import './App.css';
import Profile from './components/Profile';
import Login from './components/Login';
import ChangeColor from './components/ChangeColor';
function App() {
  return (
    <div className="App">
      <Profile></Profile>
      <Login></Login>
      <ChangeColor></ChangeColor>
    </div>
  );
}
export default App;

// index.js
import React, { useReducer } from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import {configureStore} from "@reduxjs/toolkit"
import { Provider } from 'react-redux';
import userReducer from './features/user';
import themeReducer from './features/theme'

const store = configureStore({
  reducer:{
    user: userReducer,
    theme: themeReducer,
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

// src/components/Profile.js
import React from 'react'
import './Components.css'
import { useSelector } from 'react-redux'

const Profile = () => {
    const user = useSelector((state) => state.user.value)
    const themeColor = useSelector((state) => state.theme.value)
    return(
        <div className='profile-page' style={{ color: themeColor}}>
            <h2>Profile Page</h2>
            <form className='profile-form'>
                <label>
                    Name: <input type="text" value={user.name}></input>
                </label>
                <label>
                    Age: <input type="number" value={user.age}></input>
                </label>
                <label>
                    Email: <input type="email" value={user.email}></input>
                </label>
            </form>
        </div>
    )
}
export default Profile

// src/components/Login.js
import React from "react";
import { useDispatch } from "react-redux";
import { login, logout } from "../features/user";

const Login = () => {
    const dispatch = useDispatch()
    return (
        <div>
            <button onClick={() => {dispatch(login({name:"yam", age: 24, email:'yam@gmail.com'}))}}>Login</button>
            <button onClick={() => {dispatch(logout())}}>Logout</button>
        </div>
    )
}
export default Login

// src/components/ChangeColor.js
import React, { useState } from "react";
import { useDispatch } from "react-redux";
import { changeColor } from "../features/theme";

const ChangeColor = () => {
    const [color, setColor] = useState('')
    const dispatch = useDispatch();
    const colorChange = (event) => {
        setColor(event.target.value)
    }
    return (
        <div>
            <input type="text" onChange={colorChange}/>
            <button onClick={() => {dispatch(changeColor(color))}}>Change Color</button>
        </div>
    )
}
export default ChangeColor

// src/features/user.js
import { createSlice } from "@reduxjs/toolkit";

const initialStateValue = {name:"", age: 0, email:''}
export const userSlice = createSlice({
    name: 'user', 
    initialState: { value: initialStateValue},
    reducers: {
        login: (state, action) => {
            state.value = action.payload
        },
        logout: (state, action) => {
            state.value = initialStateValue
        }
    }
})
export const {login, logout} = userSlice.actions
export default userSlice.reducer

// src/features/theme.js
import { createSlice } from "@reduxjs/toolkit";

const initialStateValue = '';
export const themeSlice = createSlice({
    name: 'theme', 
    initialState: { value: initialStateValue},
    reducers: {
        changeColor: (state, action) => {
            state.value = action.payload
        },
    }
})
export const {changeColor} = themeSlice.actions
export default themeSlice.reducer
```