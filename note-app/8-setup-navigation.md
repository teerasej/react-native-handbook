
# 9. ติดตั้งใช้งาน react-navigation

- [React Navigation](https://reactnavigation.org/)

React Navigation เป็น module ด้าน UI ตัวหนึ่ง ที่ทำให้เรานำ Component ต่างๆ ที่เราสร้างขึ้น มาเชื่อมต่อ และเปิดไปมาเหมือนเป็นแอพเดียวกันได้ 

![Paper React_React_Native 84](https://user-images.githubusercontent.com/85179/98916673-05a62600-24fe-11eb-9600-a825a51f314e.png)


## 1. ติดตั้ง navigation สำหรับ expo

รันคำสั่งด้านล่าง ใน Terminal

```bash
expo install react-native-gesture-handler react-native-reanimated
```

### ถ้าเจอปัญหาเกี่ยวกับ react-native-gesture-handler

ให้ลองไปใช้เวอร์ชั่นล่าสุด ([ดูจากที่นี่](https://github.com/kmagiera/react-native-gesture-handler/releases))

เช่น เวอร์ชั่นล่าสุดเป็น 1.4.1 ให้ไปแก้ในไฟล์ **package.json**

```json
\\ package.json
"dependencies": {
    "react-native-gesture-handler": "1.4.1",
}
```

## 2. เพิ่มข้อมูลสำหรับ Navigation เข้าไปในแต่ละเพจ

```js
// pages/home-page/HomePage.js

export class HomePage extends Component {

    static navigationOptions = {
        title: 'Home'
    };

}
```

```js
// pages/new-note-page/NewNotePage.js

export class NewNotePage extends Component {

    static navigationOptions = {
        title: 'New Note'
    };

}
```

## 3. กำหนด Page Component ลง Navigation

เปิดไฟล์ `App.js`

เราจะทำการสร้าง Navigator ขึ้นมา โดยตั้งชื่อให้กับ Page Component ต่างๆ ไว้

```js
import { createStackNavigator, createAppContainer } from 'react-navigation';

const AppNavigator = createStackNavigator({
  Home: {screen: HomePage},
  CreateNote: {screen: NewNotePage }
});

const AppContainer = createAppContainer(AppNavigator);
```

จากนั้นเราจะใช้ `AppContainer` ใส่ลงไปใน `<Provider>` แทน

```jsx
render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
        <Provider store={store}>
            <AppContainer />
        </Provider>
    );
  }
```

## 3.A ไฟล์เต็ม App.js

```jsx
import React from 'react';
import { AppLoading } from 'expo';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';

import { Provider } from 'react-redux'
import configureStore from "./redux/store";

import { createStackNavigator, createAppContainer } from 'react-navigation';

const store = configureStore();

const AppNavigator = createStackNavigator({
  Home: {screen: HomePage},
  CreateNote: {screen: NewNotePage }
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
        <AppContainer />
      </Provider>
    );
  }
}
```

## 4. นำส่วนของ Header ออกเพราะถูกแทนที่ด้วย Navigation

หลังจากใช้ Navigation แล้ว จะเห็นว่าเราไม่จำเป็นต้องใช้ Header อีกต่อไป ให้เราเอาออกจาก Page ที่เรามีอยู่

```jsx
// pages/home-page/HomePage.js จะเหลือแค่นี้
render() {
        return (
           
                <Content>
                    <List>
                        {
                            this.props.notes.map((item, index) => {
                                return (
                                    <ListItem key={index}>
                                        <Text>{item.title}</Text>
                                    </ListItem>
                                )
                            })
                        }
                    </List>
                </Content>
           
        )
    }
```

```jsx
// pages/new-note-page/NewNotePage.js จะเหลือแค่นี้
render() {
        return (
           
                <Content padder>
                    <Item inlineLabel >
                        <Label>Message: </Label>
                    <Field name="message" component={this.renderInput} />
                    </Item>
                    <Button block primary >
                        <Text>Save</Text>
                    </Button>
                </Content>
        
        )
}
```

## 5. ปรับให้ header มีปุ่มด้านขวาผ่าน navigationOptions

เริ่มจากนำ component ที่ต้องใช้เข้ามา

```jsx
// pages/home-page/HomePage.js

import { Container, Header, Title, Content,  Text, Button, Icon } from 'native-base';

```

และเปลี่ยนจาก 

```js
static navigationOptions = {
    title: 'Home'
};
```

เป็น 

```jsx
// pages/home-page/HomePage.js

static navigationOptions = ({ navigation }) => {
        return {
          headerTitle: <Text>Home</Text>,
          headerRight: (
            <Button transparent>
              <Icon name='add' />
            </Button>
          ),
        };
      };

```

## 6. ใส่ function สำหรับเปิดไปหน้า new note

```jsx
// pages/home-page/HomePage.js

static navigationOptions = ({ navigation }) => {
        return {
          headerTitle: <Text>Home</Text>,
          headerRight: (
            <Button transparent onPress={() => {
                console.log('ok')
                navigation.navigate('CreateNote')
            }}>
              <Icon name='add'/>
            </Button>
          ),
        };
      };

```

## 6.A ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Left, Right, Body, Button, Icon } from 'native-base';

export class HomePage extends Component {
    static propTypes = {
        notes: PropTypes.array
    }

    static navigationOptions = ({ navigation }) => {
        return {
          headerTitle: <Text>Home</Text>,
          headerRight: (
            <Button transparent onPress={() => {
                console.log('ok')
                navigation.navigate('CreateNote')
            }}>
              <Icon name='add'/>
            </Button>
          ),
        };
      };

    static defaultProps = {
        notes: [
            { title: 'a' },
            { title: 'b' },
            { title: 'c' }
        ]
    }


    render() {
        return (
           
                <Content>
                    <List>
                        {
                            this.props.notes.map((item, index) => {
                                return (
                                    <ListItem key={index}>
                                        <Text>{item.title}</Text>
                                    </ListItem>
                                )
                            })
                        }
                    </List>
                </Content>
           
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```

## 7. ถ้ากดปุ่ม save ใน NewNoteForm จะเป็นการบันทึกและเปิดกลับไปหน้าแรก

```js
// pages/new-note-page/NewNotePage.js

export class NewNotePage extends Component {

  onFormSave = (values) => {
    console.log(values);
    this.props.navigation.goBack();
  }

}
```
