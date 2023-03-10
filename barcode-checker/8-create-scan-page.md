
# 8. ทำหน้า Scan PopUp

- [ดาวน์โหลดโปรเจคเริ่มต้น](https://www.dropbox.com/s/rgmdzn0wup7ck3u/react-native-barcode-checker-ban-pu-starter.zip?dl=0)
- [ดาวน์โหลดโปรเจคสมบูรณ์](react-native-barcode-checker-ban-pu-finisher.zip)

## 1. ใส่ปุ่มแสกนเข้าไปที่ส่วนหัวด้านขวา ของ HomePage

เปิดไฟล์ `pages/home-page/HomePage.js`

```js
export class HomePage extends Component {

    static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => navigation.navigate('ScanPopup')}
                >
                    <Icon name='barcode' />
                </Button>
            )
        }
    };
```

กดปุ่มจะเห็น pop up แสดงขึ้นมา (แต่นี่ไม่ใช่ pop up ที่เราต้องการนะ)

## 2. สร้างหน้าสำหรับแสดงตัวแสกน

สร้างไฟล์ `pages/scan-page/ScanPage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Container, Header, Title, Content, List, ListItem, Text, Body  } from 'native-base';

export default class ScanPage extends Component {

    render() {
        return (
            <Container>
                <Header>
                    <Body>
                      <Title>Scanner</Title>
                    </Body>
                </Header>
                <View
                    style={{
                        flex: 1
                    }}>
                    
                </View>
            </Container>
        )
    }
}

```

## 3. จัดทำระบบ Navigation ที่รองรับ การทำ Popup แบบ Modal

> [React Natigation Model](https://reactnavigation.org/docs/en/modal.html)

เปิดไฟล์​ `App.js`

จากนั้นเราจะสร้าง StackNavigator ตัวที่ 2 (เรามีตัวแรกอยู่แล้ว ชื่อ **AppNavigator**)

```js
// เริ่มจากการ import ScanPage เข้ามาก่อน 
import ScanPage from './pages/scan-page/ScanPage';

// ตัวเดิม
const AppNavigator = createStackNavigator({
  Home: { screen: HomePage }
});

// ตัวใหม่ สร้างขึ้นเพื่อตั้งค่าให้ทำงานกับโหมด Modal ได้
// สังเกตว่าเราตั้งส่วนของ Main เป็น AppNavigator
const RootNavigator = createStackNavigator(
  {
    Main: {
      screen: AppNavigator
    },
    ScanPopup: {
      screen: ScanPage,
    },
  },
  {
    mode: 'modal',
    headerMode: 'none',
  });

// นำ Root Navigator มาใช้กับ App Container แทน
const AppContainer = createAppContainer(RootNavigator);
```

## 4. ทำปุ่มปิดหน้า Scan 

เปิดไฟล์ `pages/scan-page/ScanPage.js`

```js
// import component ที่จำเป็น 
import { Container, Header, Title, Content, Right, Left, Button, Icon, Text, Body } from 'native-base';

export default class ScanPage extends Component {

    // สร้าง function สำหรับย้อนกลับไปหน้าที่เปิด popup ต้นทาง
    closePopUp = () => {
        this.props.navigation.goBack();
    }


    render() {
        return (
            <Container>
                <Header>
                    <Left>

                    </Left>
                    <Body>
                        <Title>Scanner</Title>
                    </Body>
                    <Right>
                        {/* เรียกใช้ function เมื่อกดปุ่ม */}
                        <Button transparent onPress={this.closePopUp}>
                            <Icon name='close' />
                        </Button>
                    </Right>
                </Header>
                <Content>

                </Content>
            </Container>
        )
    }
}
```

## A. ไฟล์เต็ม App.js

```js
import React from 'react';
import AppLoading from 'expo-app-loading';
import { View } from 'react-native';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import Ionicons from '@expo/vector-icons/Ionicons';
import HomePage from "./pages/home-page/HomePage";

// config ส่วน redux
import { Provider } from 'react-redux';
import configureStore from "./redux/store";
const store = configureStore();

// config ส่วน navigation
import { createAppContainer } from 'react-navigation';
import { createStackNavigator } from 'react-navigation-stack';
import ScanPage from './pages/scan-page/ScanPage';

const AppNavigator = createStackNavigator({
  Home: { screen: HomePage }
});

const RootNavigator = createStackNavigator(
  {
    Main: {
      screen: AppNavigator
    },
    ScanPopup: {
      screen: ScanPage,
    },
  },
  {
    mode: 'modal',
    headerMode: 'none',
  }
);


const AppContainer = createAppContainer(RootNavigator);


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

## B. ไฟล์เต็ม `pages/scan-page/ScanPage.js`

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Container, Header, Title, Content, Right, Left, Button, Icon, Text, Body } from 'native-base';


export default class ScanPage extends Component {

    closePopUp = () => {
        this.props.navigation.goBack();
    }


    render() {
        return (
            <Container>
                <Header>
                    <Left>

                    </Left>
                    <Body>
                        <Title>Scanner</Title>
                    </Body>
                    <Right>
                        <Button transparent onPress={this.closePopUp}>
                            <Icon name='close' />
                        </Button>
                    </Right>
                </Header>
                <Content>

                </Content>
            </Container>
        )
    }
}

```

## C. ไฟล์เต็ม HomePage.js

```js
import React, { Component } from 'react'
import { View } from 'react-native'
import { Content, List, ListItem, Text, Body  } from 'native-base';

export default class HomePage extends Component {

     static navigationOptions = ({ navigation }) => {
        return {
            title: 'Home',
            headerRight: (
                <Button transparent
                    onPress={() => alert('open scan page')}
                >
                    <Icon name='barcode' />
                </Button>
            )
        }
    };

    render() {
        return (
            
                <Content>
                </Content>
            
        )
    }
}

```