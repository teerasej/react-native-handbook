
# 7. เพิ่มระบบ Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

## 1. สร้าง Reducer 

เริ่มจาก app reducer สำหรับจัดการ Action ที่เกิดขึ้นในระบบ

```jsx
// redux/reducers/app.reducer.js

const initialState = {

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

สังเกตว่าเราจะไม่กำหนด case ชื่อ action ใดๆ ตอนนี้ 

## 2. สร้าง Root Reducer 

ส่วนต่อไปเราจะสร้าง Root Reducer สำหรับการรวม Reducer ต่างๆ เข้าด้วยกัน

```jsx
// redux/reducers/root.reducer.js

import { combineReducers } from 'redux'
import appReducer from './app.reducer';

export default () => combineReducers({
  app: appReducer
})  
```

## 3. สร้าง Redux Store 

สุดท้ายคือ Store ในการ setup ตัว reducer และ middleware เข้าด้วยกัน

ให้สร้างไฟล์ `redux/store.js`

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

## 4. นำ Redux Store มาใช้กับ App ผ่าน Provider component

เราจะทำการ import ทั้ง store ที่เตรียมไว้ และ **Provider** มาใช้กับ **AppContainer** ของเรา

```jsx
// App.js
import { Provider } from 'react-redux';
import configureStore from "./redux/store";

const store = configureStore();

//...

return (
      <Provider store={store}>
        <AppContainer/>
      </Provider>
    );
```

## 5. ปรับส่วน UI ที่ไม่จำเป็นออกจากหน้า Home Page

```js
render() {
    return (
            
        <Content>
        </Content>
            
    )
}
```

## A. ไฟล์เต็ม App.js 

```jsx
import React from 'react';
import { AppLoading } from 'expo';
import { View } from 'react-native';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from "./pages/home-page/HomePage";

// config ส่วน redux
import { Provider } from 'react-redux';
import configureStore from "./redux/store";
const store = configureStore();

// config ส่วน navigation
import { createAppContainer } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';

const AppNavigator = createStackNavigator({
  Home: {screen: HomePage}
});

const AppContainer = createAppContainer(AppNavigator);


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
        <AppContainer/>
      </Provider>
    );
  }
}
```

## B. ไฟล์เต็ม HomePage.js

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body  } from 'native-base';

export class HomePage extends Component {
    static navigationOptions = {
        title: 'Home'
    };

    render() {
        return (
            
                <Content>
                </Content>
            
        )
    }
}

```
