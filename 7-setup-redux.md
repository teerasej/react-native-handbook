
# 7. Setup Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

## 1. สร้าง store และ reducer

เริ่มจาก note reducer สำหรับจัดการข้อมูลที่เป็น note

```jsx
// redux/reducers/note.reducer.js

const initialState = {
    notes: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case 'typeName':
        return { ...state, ...payload }

    default:
        return state
    }
}
```

ส่วนต่อไปเราจะสร้าง Root Reducer สำหรับการรวม Reducer ต่างๆ เข้าด้วยกัน

จะเห็นว่าในที่นี้เรามี Form Reducer ที่เอาไว้จัดการข้อมูลจาก Form ด้วย

```jsx
// redux/reducers/root.reducer.js

import { combineReducers } from 'redux'
import noteReducer from './note.reducer';
import { reducer as formReducer } from 'redux-form';

export default () => combineReducers({
  note: noteReducer,
  form: formReducer
})  
```

สุดท้ายคือ Store ในการ setup ตัว reducer และ middleware เข้าด้วยกัน

```jsx
// redux/store.js

import { createStore, applyMiddleware, compose } from 'redux';
import { logger } from 'redux-logger';

import createRootReducer from "./reducers/root.reducer";

export default function configureStore() {
    const store = createStore(
        createRootReducer(),
        compose(
            applyMiddleware(
                logger
            ),
        ),
    );

    return store;
} 
```

## 2. ใส่ Provider และ store เข้า app

```jsx
// App.js
import { Provider, connect } from 'react-redux';
import configureStore from "./redux/store";

const store = configureStore();

//...

return (
      <Provider store={store}>
        <NewNotePage/>
      </Provider>
    );
```

## A. ไฟล์เต็ม App.js 

```jsx
import React from 'react';
import { AppLoading } from 'expo';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import { HomePage } from './pages/home-page/HomePage';
import { NewNotePage } from './pages/new-note-page/NewNotePage';

import { Provider, connect } from 'react-redux';
import configureStore from "./redux/store";

const store = configureStore();

export default class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isReady: false,
    };
  }

  async componentDidMount() {
    await Font.loadAsync({
      Roboto: require('native-base/Fonts/Roboto.ttf'),
      Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
      ...Ionicons.font,
    });
    this.setState({ isReady: true });
  }

  render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
      <Provider store={store}>
        <NewNotePage/>
      </Provider>
    );
  }
}
```