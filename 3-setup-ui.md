
# 3. ติดตั้ง UI Framework

- [Nativebase UI](https://docs.nativebase.io/Components.html#Components)

## 1. ติดตั้ง VSCode Extension

- [nativebase snippet](https://marketplace.visualstudio.com/items?itemName=GeekyAnts.nativebase-snippets)

## 2. ติดตั้ง Nativebase

```bash
yarn add native-base --save
```

เสร็จแล้วรันคำสั่ง 

```bash
expo install expo-font
```

## 3. ติดตั้ง 

### วิธีแก้ปัญหา `Command `link` unrecognized.`

ถ้าเจอ Error แบบด้านล่าง ให้รันคำสั่ง `npm install` อีก 1 ครั้ง

```bash
Command `link` unrecognized. Make sure that you have run `npm install` and that you are inside a react-native project.
```

## 2. เขียนให้แน่ใจว่าตัว App โหลด font ได้แน่นอน

```jsx
// App.js
import React from 'react';
import { AppLoading } from 'expo';
import { Container, Text } from 'native-base';
import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';


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
      <View></View>
    );
  }
}
```


### วิธีแก้ปัญหา `Cannot find module metro-react-native-babel-transformer`

หากอัพเดตโค้ดแล้วเจอปัญหา แบบด้านล่าง

```bash
node_modules/expo/AppEntry.js: Cannot find module 'APP_FOLDER/node_modules/@react-native-community/cli/node_modules/metro-react-native-babel-transformer/src/index.js’
Cannot read property ‘status’ of undefined
```

ให้ถอด expo-cli ออก และลงใหม่ ส่วนใหญ่จะแก้ปัญหานี้ได้

```bash
npm remove -g expo-cli
npm i -g expo-cli
```

อ้างอิง - [Expo Forum](https://forums.expo.io/t/upgrade-expo-to-v33/23568)