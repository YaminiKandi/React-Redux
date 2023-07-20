#### Counter:
```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { Provider } from 'react-redux';
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './redux/counterSlice'

const store = configureStore({
  reducer:{
    counter: counterReducer
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

// App.js
import './App.css';
import Counter from './components/Counter';

function App() {
  return (
    <div className="App">
      <Counter></Counter>
    </div>
  );
}

export default App;

// ./src/redux/counterSlice.js
import { createSlice } from "@reduxjs/toolkit";

export const counterSlice = createSlice({
    name: 'counter',
    initialState: {
        value: 0, 
        showCounter: true
    },
    reducers: {
        increment: (state) => {
            state.value += 1
            state.showCounter = true
        },
        decrement: (state) => {
            state.value -= 1
            state.showCounter = true
        },
        incrementByAmount: (state, action) => {
            state.value += action.payload
            state.showCounter = true
        },
        toggle: (state) => {
            state.showCounter = !state.showCounter
        }
    }
})

export const { increment, decrement, incrementByAmount, toggle} = counterSlice.actions
export default counterSlice.reducer

// .src/components/Counter.js
import React, { useState } from "react";
import './Counter.css'
import { useDispatch, useSelector } from "react-redux";
import { decrement, increment, incrementByAmount, toggle } from "../redux/counterSlice";

const Counter = () => {
    const count = useSelector((state) => state.counter.value)
    const show = useSelector((state) => state.counter.showCounter)
    const dispatch = useDispatch();
    const [incrementAmount, setIncrementAmount] = useState('2')
    return (
        <div className="counter-main">
            <div className="header">Counter</div>
            <div className="counter-div">
                <button 
                    className="button" 
                    onClick={() => dispatch(increment())}
                >+</button>
                {show && <div className="value">{count}</div>}
                <button 
                    className="button" 
                    onClick={() => dispatch(decrement())}
                >-</button>
            </div>
            <div className="counter-div">
                <input 
                    value={incrementAmount}
                    className="increment-amount"
                    onChange={(e) => setIncrementAmount(e.target.value)}
                ></input>
                <button 
                    className="button" 
                    onClick={() => dispatch(incrementByAmount(Number(incrementAmount) || 0))}
                >Add amount</button>
            </div>
            <div>
                <button
                    className="button"
                    onClick={() => dispatch(toggle())}
                >Counter Toggle</button>
            </div>
        </div>
    )
}

export default Counter

// ./src/components/Counter.css
.counter-main {
    width: 500px;
    border-radius: 4px;
    background-color: #e5e5b5;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 16px;
}

.counter-div {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    padding: 8px;
}
.header {
    font-size: xx-large;
    font-weight: bold;
}
.button {
    font-size: 24px;
    cursor: pointer;
    border-radius: 4px;
    border: none;
    background-color: #a881af;
    color: white;
    padding-left: 12px;
    padding-right: 12px;
    outline: none;
    border: 2px solid transparent;
    padding-bottom: 4px;
    margin: 2px;
}
.button:hover{
    background-color: #80669d;
}
.button:active, .increment-amount:active {
    background-color: #a86db2;
}

.value {
    font-size: x-large;
    color: purple;
}
.increment-amount {
    font-size: 16px;
    padding: 8px;
    width: 32px;
    text-align: center;
    margin-right: 8px;
    background-color: #a881af;
    border: none;
    color: white;
    border-radius: 4px;
}
```