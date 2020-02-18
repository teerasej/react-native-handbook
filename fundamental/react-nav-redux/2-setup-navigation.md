
# 2. สร้างระบบ Navigation

> [ดูการใช้งาน React Navigation เพิ่มเติมได้จากที่นี่ ](https://reactnavigation.org/)

ถ้ายังไม่ได้ติดตั้ง module `@react-navigation/native`

ให้รันคำสั่งติดตั้ง 

```bash
npm install @react-navigation/native @react-navigation/stack
```
หรือ
```bash
yarn add @react-navigation/native @react-navigation/stack
```

และสุดท้ายอย่าลืม

```bash
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

## 1. import `react-native-gesture-handler`

เปิดไฟล์ `App.js` และใช้คำสั่ง import `react-native-gesture-handler` เพื่อป้องกันแอพพัง ([อ้างอิงจาก React Navigation 5](https://reactnavigation.org/docs/en/getting-started.html#installing-dependencies-into-a-bare-react-native-project)) 

```js
import 'react-native-gesture-handler';
```
 
## 2. import `NavigationContainer` และใช้กำหนด Route ของแอพพลิเคชั่น

เปิดไฟล์ `App.js`

import module สำหรับการทำ Navigation 

```js
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
```

จากนั้นสร้าง component จาก `createStackNavigator` function 

```js
const Stack = createStackNavigator();
```

ในส่วนของ App component เราจะกำหนดใช้ `<NavigationContainer>` ใน JSX

```jsx
return (
    <NavigationContainer>
      {/* กำหนด Stack */}
      <Stack.Navigator>
        {/* กำหนดหน้าแอพแรก ชื่อว่า Home และเลือก component HomePage เป็นตัว User Interface */}
        <Stack.Screen name="Home" component={HomePage} />
      </Stack.Navigator>
    </NavigationContainer>
);
```

## 3. ปรับแต่ง Screen แต่ละหน้าด้วย props `option` ของ Stack.Screen

> ดูเพิ่มเติมได้ที่ [Configuring Header bar](https://reactnavigation.org/docs/en/headers.html)

เราสามารถกำหนดส่วน Header โดยใช้ `option` prop ของ Stack.Screen 

เช่น การกำหนด title ของ header

```jsx
<Stack.Navigator>
  <Stack.Screen 
    name="Home" 
    component={HomePage} 
    options={{ title: 'หน้าแรก' }}
  />
  <Stack.Screen name="CreateNote" component={NewNotePage} 
    options={{ title: 'New Note' }}
    />
</Stack.Navigator>
```

## 4. กำหนดปุ่มใน header ผ่าน props `options` ของ Stack.Screen

เรายังสามารถกำหนด function ที่ return ค่าเป็น object ที่มีค่าต่างๆ สำหรับ Stack.Screen ได้ด้วย

```jsx
<Stack.Navigator>
    <Stack.Screen name="Home" component={HomePage}
      options={{
          headerTitle: <Text>Home</Text>,
          headerRight: () => (
            <Button transparent>
              <Icon name='add' />
            </Button>
          ),
        }}
    />
  <Stack.Screen name="CreateNote" component={NewNotePage} 
    options={{ title: 'New Note' }}
    />
</Stack.Navigator>
```

และยังสามารถส่งผ่าน props ที่มี navigation มาเพื่อใช้ในปุ่มของ header ได้ด้วย

```jsx
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
    options={{ title: 'New Note' }}
    />
</Stack.Navigator>
```

ตอนนี้ถ้ามี error อย่าเพิ่งตกใจ เราต้อง setup redux ขึ้นมาในระบบก่อน

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

import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();


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
    );
  }
}
```

