
# 2. สร้างระบบ Navigation

> [ดูการใช้งาน React Navigation เพิ่มเติมได้จากที่นี่ ](https://reactnavigation.org/)

## 1. import function มาจาก 2 module ของ React Navigation

เปิดไฟล์ `App.js`
 
เราจะ import 2 function มาจาก module ชื่อ `react-navigation` และ `react-navigation-stack`

```js
import { createAppContainer } from "react-navigation";
import { createStackNavigator } from "react-navigation-stack";
```

## 2. ตั้งค่า navigator, app container และเอามาใช้ใน JSX

เราจะกำหนดค่า Page Home และ CreateNote ด้วย component ที่เราเตรียมไว้

```js
import { createAppContainer } from "react-navigation";
import { createStackNavigator } from "react-navigation-stack";

// กำหนดชื่อ Home ด้วย HomePage Component
// และกำหนดชื่อ CreateNote ด้วย NewNotePage component
const AppNavigator = createStackNavigator({
  Home: { screen: HomePage },
  CreateNote: { screen: NewNotePage }
})

// นำ AppNavigator ที่ config พร้อมแล้วมาสร้างเป็น AppContainer
const AppContainer = createAppContainer(AppNavigator);
```

และนำมาใช้ใน `render()` ของ App component 

```jsx
export default class App extends React.Component {

    //...

    return (
      <AppContainer/>
    );
```

## 3. กำหนดส่วน Title ของ HomePage และ NewNotePage

เปิดไฟล์ `pages/home-page/HomePage.js`

เราจะเพิ่ม property ชื่อ `navigationOptions` และกำหนดค่า `title`

```js
export class HomePage extends Component {

    static navigationOptions = {
        title: 'Home'
    };

```

เปิดไฟล์ `pages/new-note-page/NewNotePage.js`

เราจะเพิ่ม property ชื่อ `navigationOptions` และกำหนดค่า `title`

```js
export class NewNotePage extends Component {

  static navigationOptions = {
    title: 'New Note'
  };

```


## 4. เพิ่มปุ่มเปิดหน้า Create Note ใน header ของ Home Page

เปิดไฟล์ `pages/home-page/HomePage.js`


```jsx
export class HomePage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => navigation.navigate('CreateNote')}
                >
                    <Icon name='add' />
                </Button>
            )
        }
    };
```

ตอนนี้ถ้ามี error อย่าเพิ่งตกใจ เราต้อง setup redux ขึ้นมาในระบบก่อน

## A. ไฟล์เต็ม App.js

```jsx
import React from 'react';
import { AppLoading } from 'expo';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';

import { createAppContainer } from "react-navigation";
import { createStackNavigator } from "react-navigation-stack";

const AppNavigator = createStackNavigator({
  Home: { screen: HomePage },
  CreateNote: { screen: NewNotePage }
})

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
      <AppContainer/>
    );
  }
}
```


## B. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Icon } from 'native-base';

export class HomePage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => navigation.navigate('CreateNote')}
                >
                    <Icon name='add' />
                </Button>
            )
        }
    };

    render() {
        return (

            <Content>
                <List>
                    <ListItem>
                        <Text>A</Text>
                    </ListItem>
                    <ListItem>
                        <Text>B</Text>
                    </ListItem>
                </List>
            </Content>

        )
    }
}


export default HomePage
```

## C. ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Item, Input, Label } from 'native-base';

import NewNoteForm from './NewNoteForm';

export class NewNotePage extends Component {
  
  static navigationOptions = {
    title: 'New Note'
  };

  onFormSave = (values) => {
    console.log(values);
  }


  render() {
    return (
      <Content padder>
        <NewNoteForm onSubmit={this.onFormSave}/>
      </Content>
    )
  }
}

export default NewNotePage
```