
# 8. ติดตั้งใช้งาน react-navigation

- [React Navigation](https://reactnavigation.org/)

React Navigation เป็น module ด้าน UI ตัวหนึ่ง ที่ทำให้เรานำ Component ต่างๆ ที่เราสร้างขึ้น มาเชื่อมต่อ และเปิดไปมาเหมือนเป็นแอพเดียวกันได้ 

![Paper React_React_Native 84](https://user-images.githubusercontent.com/85179/98916673-05a62600-24fe-11eb-9600-a825a51f314e.png)


## 1. ติดตั้ง navigation สำหรับ expo

ให้ดู[การติดตั้ง package ที่จำเป็นทั้งหมดในส่วนการติดตั้ง](1-setup-expo.md)

### ถ้าเจอปัญหาเกี่ยวกับ react-native-gesture-handler

ให้ลองไปใช้เวอร์ชั่นล่าสุด ([ดูจากที่นี่](https://github.com/kmagiera/react-native-gesture-handler/releases))

เช่น เวอร์ชั่นล่าสุดเป็น 1.4.1 ให้ไปแก้ในไฟล์ **package.json**

```json
\\ package.json
"dependencies": {
    "react-native-gesture-handler": "1.4.1",
}
```


## 2. กำหนด Page Component ลง Navigation

เปิดไฟล์ `App.js`

เราจะทำการสร้าง Navigator ขึ้นมา โดยตั้งชื่อให้กับ Page Component ต่างๆ ไว้

```js
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
const Stack = createStackNavigator();
```

จากนั้นเราจะใช้ `NavigationContainer` ใส่ลงไปด้านใน `<Provider>` 

เราสามารถตั้งค่าผ่าน JSX ชื่อ `<Stack.Navigator>` 

เช่น 

```jsx
return (
  <Provider store={store}>
      <NavigationContainer>
        <Stack.Navigator
          screenOptions={{
            headerStyle: {
              backgroundColor: 'green',
            },
            headerTintColor: '#fff',
          }}
        >
          <Stack.Screen name="Note App" component={HomePage} />
          <Stack.Screen name="New Note Page" component={NewNotePage} />
        </Stack.Navigator>
      </NavigationContainer>
    </Provider>
)
```

- สามารถ[ดูวิธีปรับแต่ง Stack.Navigator ได้ที่นี่](https://reactnavigation.org/docs/headers#sharing-common-options-across-screens) 
- และ[ดูวิธีปรับแต่ง Stack.Screen ได้ที่นี่](https://reactnavigation.org/docs/headers#adjusting-header-styles)

```jsx
render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
        <Provider store={store}>
            <NavigationContainer>
              <Stack.Navigator>
                <Stack.Screen name="New Note Page" component={NewNotePage} />
              </Stack.Navigator>
            </NavigationContainer>
        </Provider>
    );
  }
```

## 2.A ไฟล์เต็ม App.js

```jsx
import React, { useState } from 'react';
import AppLoading from 'expo-app-loading';
import { View, Text } from 'react-native';
import { useEffectAsync } from 'useeffectasync'

import * as Font from 'expo-font';
import Ionicons from '@expo/vector-icons/Ionicons';
import HomePage from './pages/home-page/HomePage';
import NewNotePage from './pages/new-note-page/NewNotePage';

import { Provider } from 'react-redux';
import configureStore from "./redux/store";

import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();
const store = configureStore();

export default function App() {

  const [isReady, setIsReady] = useState(false)

  
  useEffectAsync(async () => {
   
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
      <NavigationContainer>
        <Stack.Navigator
          screenOptions={{
            headerStyle: {
              backgroundColor: 'green',
            },
            headerTintColor: '#fff',
          }}
        >
          <Stack.Screen name="Note App" component={HomePage} />
          <Stack.Screen name="New Note Page" component={NewNotePage} />
        </Stack.Navigator>
      </NavigationContainer>
    </Provider>
  );

}
```

## 4. นำส่วนของ Header ออกเพราะถูกแทนที่ด้วย Navigation

หลังจากใช้ Navigation แล้ว จะเห็นว่าเราไม่จำเป็นต้องใช้ Header อีกต่อไป ให้เราเอาออกจาก Page ที่เรามีอยู่

```jsx
// pages/home-page/HomePage.js จะเหลือแค่นี้

//...

export default function HomePage() {

    //...

    return (
        <Container>
            {/* ส่วนที่เอาออก */}
            {/* 
            <Header>
                <Body>
                    <Title>Note App</Title>
                </Body>
            </Header>
            */}
            <Content>
                <List>
                    {
                      //...
                    }
                </List>
            </Content>
        </Container>
    )
}

```

```jsx
// pages/new-note-page/NewNotePage.js จะเหลือแค่นี้

//...

export default function NewNotePage() {

    //...


    return (
        <Container>
            
            {/* ส่วนที่เอาออก */}
            {/* 
            <Header>
                <Body>
                    <Title>New Note</Title>
                </Body>
            </Header>
            */}
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()
                    }}
                >
                    ...
                </Formik>
            </Content>
        </Container>
    )
}

```

## 5. ปรับให้ header มีปุ่มด้านขวาผ่าน navigationOptions

เริ่มจาก import Component และ hook ที่ต้องใช้เข้ามาในไฟล์ `pages/home-page/HomePage.js`

```jsx
// pages/home-page/HomePage.js

// นำ component `Button` และ `Icon` ที่ต้องใช้ในการสร้างปุ่ม
import { Body, Container, Content, Header, List, ListItem, Title,  Button, Icon } from 'native-base'

// useLayoutEffect เป็น hook ที่จะทำงานหลังจาก component render ไปแล้วใช้ในการ re-render component ที่ต้องการ
import React, { useLayoutEffect } from 'react'
```

และเรียกใช้ `useLayoutEffect()` เพื่อทำการ set ค่าให้ header ของหน้า home page

```jsx

//..

// ดึงค่าตัวแปร navigation ออกมาจาก props ที่ส่งเข้ามาใน HomePage component
// ถ้าไม่ใช้วิธีนี้ สามารถกำหนดเป็น HomePage(props) ได้ แต่ตอนเรียกใช้ navigation ต้องเรียกผ่าน props.navigation
export default function HomePage({navigation}) {

    useLayoutEffect(() => {
        navigation.setOptions({
          headerRight: () => (
            <Button transparent>
                <Icon 
                    name='add' 
                    style={
                        {color: 'white'}
                    }
                />
            </Button>
          ),
        });
      }, [navigation]);

  //..

}
```

- [ดูเพิ่มเติม การใส่ปุ่มใน Header](https://reactnavigation.org/docs/header-buttons)
- [ดูเพิ่มเติม `useLayoutEffect`](https://reactjs.org/docs/hooks-reference.html#uselayouteffect)

## 6. ใส่ function สำหรับเปิดไปหน้า new note

หลังจากเราเพิ่มปุ่มเข้าไปใน header ของหน้า HomePage ได้แล้ว ก็จะมาใส่ event `onPress` ให้ปุ่มกัน

```jsx
// pages/home-page/HomePage.js

export default function HomePage({navigation}) {

    useLayoutEffect(() => {
        navigation.setOptions({
          headerRight: () => (
            {/* เพิ่ม function ให้กับ props onPress ของปุ่ม */}
            <Button transparent onPress={onPlusButtonPress}>
                <Icon 
                    name='add' 
                    style={
                        {color: 'white'}
                    }
                />
            </Button>
          ),
        });
      }, [navigation]);

    // เขียน function เปิดไปยัง component NewNotePage
    const onPlusButtonPress = () => {
        navigation.navigate('New Note Page')
    }

  //..

}
```


## 7. ถ้ากดปุ่ม save ใน NewNoteForm จะเป็นการบันทึกและเปิดกลับไปหน้าแรก

```js
// pages/new-note-page/NewNotePage.js

//..

// ดึงค่าตัวแปร navigation ออกมาจาก props ที่ส่งเข้ามาใน NewNotePage component
// ถ้าไม่ใช้วิธีนี้ สามารถกำหนดเป็น NewNotePage(props) ได้ แต่ตอนเรียกใช้ navigation ต้องเรียกผ่าน props.navigation
export default function NewNotePage({ navigation }) {

  //..

  return (
        <Container>
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()

                        // สั่ง navigation ให้ย้อนกลับไป
                       navigation.goBack()
                    }}
                >

}
```

## A. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import { Body, Container, Content, Header, List, ListItem, Title, Button, Icon } from 'native-base'
import React, { useLayoutEffect } from 'react'
import { View, Text } from 'react-native'

export default function HomePage({ navigation }) {

    useLayoutEffect(() => {
        navigation.setOptions({
            headerRight: () => (
                <Button transparent onPress={onPlusButtonPress}>
                    <Icon
                        name='add'
                        style={
                            { color: 'white' }
                        }
                    />
                </Button>
            ),
        });
    }, [navigation]);

    const onPlusButtonPress = () => {
        navigation.navigate('New Note Page')
    }

    let notes = [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]

    return (
        <Container>
            <Content>
                <List>
                    {
                        notes.map((item, index) => {
                            return (
                                <ListItem key={index}>
                                    <Text>{item.title}</Text>
                                </ListItem>
                            )
                        })
                    }
                </List>
            </Content>
        </Container>
    )
}

```

## B. ไฟล์เต็ม `pages/new-note-page/NewNotePage.js`

```jsx
import { Body, Header, Button, Container, Content, Input, Item, Label, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'
import * as yup from 'yup'
import { Field, Formik } from "formik";

export default function NewNotePage({ navigation }) {

    const validationSchema = yup.object({
        message: yup.string()
            .required('Please fill something')
            .min(3, 'fill something more...')
    })


    return (
        <Container>
            <Content padder>
                <Formik
                    initialValues={{
                        message: '',
                    }}
                    validationSchema={validationSchema}
                    onSubmit={(values, { resetForm  }) => {
                        console.log(values)
                        resetForm()

                        navigation.goBack()
                    }}
                >
                    {
                        ({ handleChange, handleBlur, handleSubmit, values, errors }) => {
                            return (
                                <View>
                                    <Item stackedLabel >
                                        <Label>Message: </Label>
                                        <Field
                                            id="message"
                                            name="message"
                                            component={Input}
                                            value={values.message || ''}
                                            onChangeText={handleChange('message')}
                                            onBlur={handleBlur('message')}
                                        />
                                        {Boolean(errors.message) ?
                                            (<Text>{errors.message}</Text>) : null
                                        }
                                    </Item>
                                    <Button
                                        block
                                        primary
                                        onPress={handleSubmit}
                                    >
                                        <Text>Save</Text>
                                    </Button>
                                </View>
                            )
                        }
                    }
                </Formik>
            </Content>
        </Container>
    )
}

```