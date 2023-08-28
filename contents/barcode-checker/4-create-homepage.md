
# 4. สร้าง และใช้งานหน้า HomePage

## 1. สร้าง pages/home-page/HomePage.js

> ใช้ snippet rnfes ได้

```jsx
// pages/home-page/HomePage.js
import { StyleSheet, Text, View } from 'react-native'
import React from 'react'

const HomePage = () => {
  return (
    <View>
      <Text>HomePage</Text>
    </View>
  )
}

export default HomePage

const styles = StyleSheet.create({})
```

## 2. นำมาใช้ใน App.js

```jsx
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import { NativeBaseProvider, Box } from "native-base";
import HomePage from './pages/home-page/HomePage';

export default function App() {
  return (
    <NativeBaseProvider>
      <SafeAreaView>

        {/* แทรก HomePage component */}
        <HomePage/>

      </SafeAreaView>
      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}

```

## 3. สร้าง UI ของหน้า HomePage

เริ่มจากจัดการ import 

```js
// pages/home-page/HomePage.js

import { StyleSheet, View } from 'react-native'
import React from 'react'
// import component ที่จำเป็น
import { Box, HStack, Text } from 'native-base'

const HomePage = () => {
    return (
        <>
            {/* กำหนดพื้นที่ safe area และสี */}
            <Box safeAreaTop bgColor="violet.800" />
            {/* กำหนดส่วนที่เป็น  header */}
            <HStack bg="violet.800" px="1" py="3" justifyContent="space-between" w="100%">
                {/* กำหนดข้อความ */}
                <Text color="white" fontSize="20" fontWeight="bold">
                    Home
                </Text>
            </HStack>
        </>
    )
}

export default HomePage

const styles = StyleSheet.create({})
```

### ในกรณีที่ต้องการแก้ปัญหา Overlap status bar ใน Android

เพิ่มส่วนนี้ลงไปใน Expo setting ของไฟล์ด้านล่าง

```json
// app.json
"androidStatusBar": {
      "barStyle": "light-content",
      "backgroundColor": "#C2185B"
}
```
