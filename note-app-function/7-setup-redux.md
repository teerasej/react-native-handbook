
# 7. Setup Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

> Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
1. กลุ่ม Actions (type และ action object)
2. กลุ่ม Reducer
3. กลุ่ม Store

![Paper React   React Native 27](https://user-images.githubusercontent.com/85179/63178797-f921ec00-c074-11e9-9781-48541785d151.png)


## 1. สร้าง Action 

```js
// redux/actions.js

const type = {
    SAVE_NEW_NOTE: 'SAVE_NEW_NOTE'
}

export default type
```



## 2. สร้าง Reducer

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
สังเกตว่า เราเอา parameter ชื่อ type มาเทียบชื่อ action เพื่อจัดการข้อมูลที่มากับ Action ได้

```jsx
// redux/reducer.js

import actions from './actions'

// ค่าเริ่มต้นของ redux state ที่ส่งให้ component ใช้งานตอนเริ่มต้นโปรเจค
const initialState = {
    notes: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

      // นำชื่อ action มาเทียบ กับ type ของ action ที่จะถูกส่งมาจาก component
    case actions.SAVE_NEW_NOTE:
        return { ...state, ...payload }

    default:
        return state
    }
}
```


## 3. สร้าง Store

### Redux Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย

### เริ่มสร้าง Store

สุดท้ายคือ Store ในการ setup ตัว reducer และ middleware เข้าด้วยกัน

```jsx
// redux/store.js


import { createStore } from 'redux'
import reducer from './reducer'

// สร้าง function ที่เตรียม store object ไว้ไปใช้กับ component ในไฟล์อื่น
export default function configureStore() {
    
    // ใช้ function createStore() สร้าง store object โดยกำหนด reducer ที่เราสร้างไว้ลงไป
    // ในที่นี้ store จะสามารถใช้ state object จาก reducer ได้ และสามารถส่ง action ให้ reducer ได้เช่นกัน
    let store = createStore(reducer)
    return store
}
```

## 3. นำ store ที่สร้างไว้ มากำหนดให้กับ Provider component 

![Paper React   React Native 29](https://user-images.githubusercontent.com/85179/63178875-1b1b6e80-c075-11e9-82a6-d187cfcc7606.png)

- Provider Component เป็น JSX Component ตัวหนึ่ง
- จุดประสงค์เพื่อเชื่อม Component อื่นๆ เข้ากับ Redux store
- ดังนั้นมักจะเห็นการใช้ Provider component ครอบด้านนอกสุดของทุก Component เพื่อให้สามารถเข้าถึง Component อื่นๆ ภายในตัวมันได้

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
import React, { useState } from 'react';
import AppLoading from 'expo-app-loading';
import { View, Text } from 'react-native';
import {useEffectAsync} from 'useeffectasync'


import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';

import { Provider } from 'react-redux';
import configureStore from "./redux/store";
const store = configureStore();

export default function App() {

  const [isReady, setIsReady] = useState(false)

  
  useEffectAsync( async () => {
   
    await Font.loadAsync({
      Roboto: require('native-base/Fonts/Roboto.ttf'),
      Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
      ...Ionicons.font,
    })

  
    setIsReady(true)
  }, [])


   
    if (!isReady) {
      return <View></View>;

    }

    return (
      <Provider store={store}>
        <NewNotePage/>
      </Provider>
    );
  
}
```
