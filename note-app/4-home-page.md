
# 4. สร้าง และใช้งานหน้า HomePage

## 1. สร้าง HomePage.js

> ใช้ snippet rnredux ได้

```jsx
// pages/home-page/HomePage.js
import React, { Component } from 'react'
import { View, Text } from 'react-native'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

export class HomePage extends Component {
    static propTypes = {
      // ลบส่วนนี้ออกจาก snippet ถ้ายังไม่ใช้
    }

    render() {
        return (
            <View>
                <Text> prop </Text>
            </View>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
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
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Container, Header, Title, Content, List, ListItem, Text, Body  } from 'native-base';

export class HomePage extends Component {
    static propTypes = {
       
    }

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
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(HomePage)
```
