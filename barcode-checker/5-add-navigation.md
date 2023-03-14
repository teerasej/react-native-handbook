
# 6. เพิ่มระบบ Navigation

ถ้าต้องการ สามารถ clone โปรเจคได้จากคำสั่งด้านล่าง

```bash
git clone -b starter-with-redux https://github.com/teerasej/nextflow-react-native-barcode-checker-2
```

## 1. สร้างระบบ Navigation 

เปิดไฟล์ `App.js`

```jsx
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import { NativeBaseProvider, Box } from "native-base";
import HomePage from './pages/home-page/HomePage';

// Import NavigationContainer ที่ต้องครอบ App ทั้งหมดของเรา
import { NavigationContainer } from '@react-navigation/native';
// สร้าง Navigation แบบ Stack
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NativeBaseProvider>
      {/* ครอบ Navigation Container เพื่อสร้างระบบ Navigation */}
      <NavigationContainer>

        {/* สร้าง Stack Navigator โดยใช้ JSX */}
        <Stack.Navigator>

          {/* กำหนด Screen พร้อมทั้ง name และ component ที่ต้องการ */}
          <Stack.Screen name="Home" component={HomePage} />
        </Stack.Navigator>
        <StatusBar style="auto" />
      </NavigationContainer>
    </NativeBaseProvider>
  );
}

```

## 2. ลบ Header ออกจากหน้า Home Page

เปิดไฟล์ `pages/home-page/HomePage.js`

ในที่นี้เราไม่จำเป็นต้องใช้ header ของ Home page แล้ว เราจะไปใช้ของ React Navigator เอง

```jsx
// pages/home-page/HomePage.js
import { StyleSheet, View } from 'react-native'
import React from 'react'
import { Box, HStack, Text } from 'native-base'

const HomePage = ({ navigator }) => {
    return (
        <>
            {/* ลบส่วนนี้ออก */}
        </>
    )
}

export default HomePage

const styles = StyleSheet.create({})
```


