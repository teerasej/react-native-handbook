
# 5. Setup redux store เข้ากับ navigation ผ่าน Provider

เปิดไฟล์ `App.js`

เราจะ import ฟังก์ชั่น `configureStore` ที่เราสร้างไว้ มาเรียกใช้ใน App Component เพื่อสร้างตัวแปร `store` ใช้งาน

รวมถึง import `Provider` เป็น component เพื่อเชื่อมต่อ store เข้ากับ Navigation

```js

import configureStore from "./redux/store";
import { Provider } from "react-redux";

const store = configureStore();
```

จากนั้นเอา Provider มาครอบ AppContainer ของเราใน JSX พร้อมกำหนดตัวแปร store ลงไป ถือว่าเรียบร้อย

```jsx
render() {  
    return (
      <Provider store={store}>
        <NavigationContainer>
          ...
        </NavigationContainer>
      </Provider>
    );
}
```

## A. ไฟล์เต็ม App.js

```jsx
import React from 'react';
import 'react-native-gesture-handler';
import { AppLoading } from 'expo';
import { Container, Text, Button, Icon } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';
import { Provider } from 'react-redux';
import createStore from "./redux/store";
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();
const store = createStore();

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
      <NavigationContainer>
        <Stack.Navigator>
          <Stack.Screen name="Home" component={HomePage}
            options={(props) => {
              return {
                headerTitle: <Text>Home</Text>,
                headerRight: () => (
                  <Button transparent
                    onPress={() => props.navigation.navigate('CreateNote')}
                  >
                    <Icon name='add' />
                  </Button>
                ),
              }
            }}
          />
          <Stack.Screen name="CreateNote" component={NewNotePage} 
            options={{
              title: 'New Note'
            }}
          />
        </Stack.Navigator>
      </NavigationContainer>
      </Provider>
    );
  }
}
```