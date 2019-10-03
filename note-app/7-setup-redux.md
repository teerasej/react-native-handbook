
# 7. Setup Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
1. กลุ่ม Actions (type และ action object)
2. กลุ่ม Reducer
3. กลุ่ม Store

![Paper React   React Native 27](https://user-images.githubusercontent.com/85179/63178797-f921ec00-c074-11e9-9781-48541785d151.png)

## 1. สร้าง store และ reducer

### Reducer

_เปรียบได้คล้ายๆ กับ Controller ใน MVC แต่ Reducer จะส่ง State ให้กับ Component_

_มองว่า State ตอนนี้ ถูกใช้แทน Model ของ MVC ก็ได้_

Reducer เป็นส่วนที่จะจัดการรับ Action ที่ส่งมาจากส่วนต่างๆ ของ Redux, ทำตาม business logic และอัพเดต State กลับไปที่ Component ที่ใช้งาน

reducer ต้องการ ข้อมูลเริ่มต้น สำหรับใช้ส่งไปให้ Component ต่างๆ เสมอ เรามักเรียกส่วนนี้ว่า **initialState**

**initialState** ก็คือ object ตัวหนึ่งที่ reducer จะส่งไปให้กับ Component ต่างๆ ตอนเริ่มทำงานนั่นเอง

เราสามารถกำหนดอะไรลงไปใน **initialState** ก็ได้ เหมือนเป็น object ของ JavaScript ทั่วไป

### เริ่มสร้าง Reducer

_ใช่้ snippet `rxreducer` ได้_

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

## 2. สร้าง Redux Store

### Redux Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย

### เริ่มสร้าง Store

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

## 3. นำ store ที่สร้างไว้ ในกำหนดให้กับ Provider component ที่จะส่งผ่าน store ให้กับทุก component ที่อยู่ด้านใน

![Paper React   React Native 29](https://user-images.githubusercontent.com/85179/63178875-1b1b6e80-c075-11e9-82a6-d187cfcc7606.png)

เริ่มจาก import `Provider` component จาก `react-redux` และตัว store ที่เราสร้างไว้ 

```jsx
// App.js
import { Provider } from 'react-redux';
import configureStore from "./redux/store";

// เรียกใช้งาน `configureStore()` เพื่อสร้าง store object
const store = configureStore();

//...

// และกำหนด `store` ที่ได้ให้กับ `<Provider>` สังเกตว่าเราจะครอบทุกส่วนของ Application 
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
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';

import { Provider } from 'react-redux';
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
