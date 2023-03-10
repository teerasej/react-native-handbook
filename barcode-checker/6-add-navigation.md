
# 6. เพิ่มระบบ Navigation

## ติดตั้ง navigation สำหรับ expo

รันคำสั่งด้านล่าง ใน Terminal

```bash
expo install react-native-gesture-handler react-native-reanimated
```

## 1. กำหนดข้อมูลให้หน้า Home Page

เปิดไฟล์ `pages/home-page/HomePage.js`

เราจะเพิ่ม property `navigationOptions` เข้าไปใน **HomePage** เพื่อใช้สำหรับระบบ React Navigation

```js

export default class HomePage extends Component {

    static navigationOptions = {
        // กำหนด title ของส่วนนี้เป็น Home
        title: 'Home'
    };

    //...
}
```

## 2. สร้างระบบ Navigation 

เปิดไฟล์ `App.js`

```js
import { createAppContainer } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';

const AppNavigator = createStackNavigator({
  Home: { screen: HomePage }
});

const AppContainer = createAppContainer(AppNavigator);
```

**AppContainer** คือระบบที่เราสามารถเอาไปใช้กับ Redux Provider ได้ 

## 3. ถ้ามีการเอา AppContainer ไปใช้ตอนนี้... จะ error 

ถ้าใช้ `AppContainer` ใส่ลงไปใน `<HomePage>` แทนแบบนี้

```jsx
render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
        <AppContainer />
    );
  }
```

ดังนั้นตอนนี้ถ้าเจอ Error ก็ไม่ใช่เรื่องผิดปกติอะไร เพราะปกติ **AppContainer** ต้องเอาไปใช้กับ Redux Provider นั่นเอง


## A. ไฟล์เต็ม `App.js`

```js
import React from 'react';
import AppLoading from 'expo-app-loading';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import Ionicons from '@expo/vector-icons/Ionicons';
import HomePage from './pages/home-page/HomePage';;

import { createAppContainer } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';

const AppNavigator = createStackNavigator({
  Home: { screen: HomePage }
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
      <AppContainer />
    );
  }
}
```

## B. ไฟล์เต็ม `pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Container, Header, Title, Content, List, ListItem, Text, Body  } from 'native-base';


export default class HomePage extends Component {

    static navigationOptions = {
        // กำหนด title ของส่วนนี้เป็น Home
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

