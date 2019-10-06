
# 5. สร้าง และใช้งานหน้า HomePage

## 1. สร้าง HomePage.js

> ใช้ snippet rnce ได้

```jsx
// pages/home-page/HomePage.js
import React, { Component } from 'react'
import { Text, View } from 'react-native'

export class HomePage extends Component {
    render() {
        return (
            <View>
                <Text> textInComponent </Text>
            </View>
        )
    }
}

export default HomePage
```

## 2. นำมาใช้ใน App.js

```jsx
// App.js
import HomePage from './pages/home-page/HomePage';;

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
                      <Title>Home</Title>
                    </Body>
                </Header>
                <Content>
                    
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
import React from 'react';
import { AppLoading } from 'expo';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';
import HomePage from './pages/home-page/HomePage';;

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
      <HomePage/>
    );
  }
}
```

## B. ไฟล์เต็ม `pages/home-page/HomePage.js`

```jsx
import React, { Component } from 'react'
import { View } from 'react-native'
import { Container, Header, Title, Content, List, ListItem, Text, Body  } from 'native-base';

export default class HomePage extends Component {

    render() {
        return (
            <Container>
                <Header>
                    <Body>
                      <Title>Home</Title>
                    </Body>
                </Header>
                <Content>
                    
                </Content>
            </Container>

        )
    }
}

```