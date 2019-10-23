
# 3. ติดตั้ง UI Framework

- [ดูเพิ่มเติมเกี่ยวกับ Nativebase UI](https://docs.nativebase.io/Components.html#Components)

## 1. ติดตั้ง VSCode Extension

- [Nativebase snippet](https://marketplace.visualstudio.com/items?itemName=GeekyAnts.nativebase-snippets)

## 2. สร้างโปรเจค และติดตั้ง Nativebase

```bash
expo init nextflow-note
cd nextflow-note
npm install native-base
expo install expo-font
```

หรือ

```bash
expo init nextflow-note
cd nextflow-note
yarn add native-base
expo install expo-font
```

## 3. ติดตั้ง 

### วิธีแก้ปัญหา `Command `link` unrecognized.`

ถ้าเจอ Error แบบด้านล่าง ให้รันคำสั่ง `npm install` อีก 1 ครั้ง

```bash
Command `link` unrecognized. Make sure that you have run `npm install` and that you are inside a react-native project.
```

## 4. เขียน Logic ให้แน่ใจว่าตัว App โหลด font ของ Native base UI พร้อมใช้งานได้แน่นอน

```jsx
// App.js
import React from 'react';
import { AppLoading } from 'expo';
import { View } from 'react-native';

// import Font มาจาก package expo-font
import * as Font from 'expo-font';
// import Icon มาใช้งาน (ถ้าต้องการ)
import { Ionicons } from '@expo/vector-icons';

// แปลง Function component 
// เป็น Class component 
export default class App extends React.Component {

  // Constructor method, ทำงานเป็นตัวแรกตอน class App ถูกสร้างขึ้นมาใช้งานในระบบ
  constructor(props) {
    super(props);
    this.state = {
      isReady: false,
    };
  }

  // Life cycle method `componentDidMount()` 
  // ทำงานหลังจาก App component ถูกสร้างขึ้นแสดงบนหน้าแอพแล้ว
  async componentDidMount() {

    // สั่งให้ Load font เพื่อใช้งานใน UI Component ที่สร้างด้วย Native base
    await Font.loadAsync({
      Roboto: require('native-base/Fonts/Roboto.ttf'),
      Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
      ...Ionicons.font,
    });

    // ตั้งค่า State ใหม่ เพื่อให้ App component ทำการ render ตัวเองอีกครั้ง
    this.setState({ isReady: true });
  }

  render() {
    
    // แสดงตัว Loading ถ้า state ไม่พร้อม 
    // เพื่อป้องกันการ error เวลาที่ load font ให้กับ Native base UI ไม่เสร็จ
    if (!this.state.isReady) {
      return <AppLoading />;
    }

    // แสดง User Interface ที่แท้จริงของแอพ
    return (
      <View>
        <Text>
            Woohoooo...
        </Text>
      </View>
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
