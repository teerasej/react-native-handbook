
# สร้าง และใช้งานหน้า HomePage

## 1. สร้าง HomePage.js

> ใช้ snippet rnredux ได้

```jsx
// pages/home-page/HomePage.js
// snippet: rnf
import React from 'react'
import { View, Text } from 'react-native'

export default function HomePage() {
    return (
        <View>
            <Text>Home Page</Text>
        </View>
    )
}

```

## 2. นำมาใช้ใน App.js

```jsx
// App.js
import HomePage from './pages/home-page/HomePage';

render() {
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    return (
      <HomePage/>
    );
  }
```

## 3. สร้าง UI ของหน้า HomePage

เริ่มจากจัดการ import 

```js
// แก้จาก
import { View, Text } from 'react-native'
// เปลี่ยนเป็น
import { View } from 'react-native'

// import UI component ของ Nativebase มาใช้งาน
import { Container, Header, Title, Content, List, ListItem, Text, Body  } from 'native-base';
```

และตามด้วย Component ใน `render()`

```jsx
    render() {
        return (
            <Container>
                <Header>
                    <Body>
                      <Title>Note</Title>
                    </Body>
                </Header>
                <Content>
                    <List>
                        <ListItem>
                            <Text>Simon Mignolet</Text>
                        </ListItem>
                        <ListItem>
                            <Text>Nathaniel Clyne</Text>
                        </ListItem>
                        <ListItem>
                            <Text>Dejan Lovren</Text>
                        </ListItem>
                    </List>
                </Content>
            </Container>

        )
    }
```

## 4. แก้ปัญหา Overlap status bar ใน Android

เพิ่มส่วนนี้ลงไปใน Expo setting ของไฟล์ด้านล่าง

```json
// app.json
"androidStatusBar": {
      "barStyle": "light-content",
      "backgroundColor": "#C2185B"
}
```

## A. ไฟล์เต็ม App.js

```jsx
// App.js
import React, { useState } from 'react';
import AppLoading from 'expo-app-loading';
import { View, Text } from 'react-native';
import {useEffectAsync} from 'useeffectasync'

// import Font มาจาก package expo-font
import * as Font from 'expo-font';
// import Icon มาใช้งาน (ถ้าต้องการ)
import Ionicons from '@expo/vector-icons/Ionicons';

// แปลง Function component 
// เป็น Class component 
export default function App() {

  const [isReady, setIsReady] = useState(false)

  // ทำงานหลังจาก App component ถูกสร้างขึ้นแสดงบนหน้าแอพแล้ว
  useEffectAsync( async () => {
    // สั่งให้ Load font เพื่อใช้งานใน UI Component ที่สร้างด้วย Native base
    await Font.loadAsync({
      Roboto: require('native-base/Fonts/Roboto.ttf'),
      Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
      ...Ionicons.font,
    })

    // ตั้งค่า State ใหม่ เพื่อให้ App component ทำการ render ตัวเองอีกครั้ง
    setIsReady(true)
  }, [])


    // แสดงตัว Loading ถ้า state ไม่พร้อม 
    // เพื่อป้องกันการ error เวลาที่ load font ให้กับ Native base UI ไม่เสร็จ
    if (!isReady) {
      return <AppLoading/>;
    }

    // แสดง User Interface ที่แท้จริงของแอพ
    return (
      <HomePage/>
    );
  
}
```

## B. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import { Body, Container, Content, Header, List, ListItem, Title } from 'native-base'
import React from 'react'
import { View, Text } from 'react-native'

export default function HomePage() {
    return (
        <Container>
            <Header>
                <Body>
                    <Title>Note</Title>
                </Body>
            </Header>
            <Content>
                <List>
                    <ListItem>
                        <Text>Simon Mignolet</Text>
                    </ListItem>
                    <ListItem>
                        <Text>Nathaniel Clyne</Text>
                    </ListItem>
                    <ListItem>
                        <Text>Dejan Lovren</Text>
                    </ListItem>
                </List>
            </Content>
        </Container>
    )
}

```
