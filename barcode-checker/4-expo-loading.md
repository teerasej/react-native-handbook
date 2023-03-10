
# 4. ปรับใช้ Expo Loading

## 1. ติดตั้ง Expo Font 

รันคำสั่ง ติดตั้ง Expo Font ใน Terminal 

```bash
npx expo install expo-font
```

## 2. เขียน Logic ให้แน่ใจว่าตัว App โหลด font ของ Native base UI พร้อมใช้งานได้แน่นอน

```jsx
// App.js
import React from 'react';
import AppLoading from 'expo-app-loading';
import { View, Text } from 'react-native';

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


