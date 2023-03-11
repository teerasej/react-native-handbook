
# 7. ทำหน้า Scan PopUp

## 1. สร้างหน้าสำหรับแสดงตัวแสกน

สร้างไฟล์ `pages/scan-page/ScanPage.js`

```js
// pages/scan-page/ScanPage.js

import { StyleSheet, View } from 'react-native'
import React from 'react'
// import component ที่จำเป็น
import { Box, HStack, Text } from 'native-base'

const ScanPage = () => {
    return (
        <>
            
        </>
    )
}

export default ScanPage

const styles = StyleSheet.create({})
```

## 2. เพิ่ม Scan Page เข้าไปใน Navigation 

```js
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import HomePage from './pages/home-page/HomePage';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

{/* Import ScanPage */}
import ScanPage from './pages/scan-page/ScanPage';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NativeBaseProvider>
      <NavigationContainer>
        <Stack.Navigator>
          <Stack.Screen name="Home" component={HomePage} />

          {/* กำหนด Scan page เข้าไปใน Navigation และตั้งชื่อ */}
          <Stack.Screen name="Scan" component={ScanPage} />

        </Stack.Navigator>
        <StatusBar style="auto" />
      </NavigationContainer>
    </NativeBaseProvider>
  );
}


```

## 3. ใส่ปุ่มแสกนเข้าไปที่ส่วนหัวด้านขวา ของ HomePage Header


```js
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';

{/* Import Icon และ IconButton ของ Native base */}
import { NativeBaseProvider, Box, IconButton, Icon } from "native-base";

import HomePage from './pages/home-page/HomePage';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

{/* Import FontAwesome Icon */}
import { FontAwesome } from '@expo/vector-icons';

import ScanPage from './pages/scan-page/ScanPage';

const Stack = createNativeStackNavigator();

export default function App() {
  return (
    <NativeBaseProvider>
      <NavigationContainer>
        <Stack.Navigator>
          <Stack.Screen name="Home" 
          component={HomePage} 
          options={() => ({ 
            title: 'My home',

            {/* กำหนด function ที่จะ return component ที่จะแสดงเป็นปุ่มด้านขวาของ react navigation header */}
            headerRight: () => (
              <IconButton 
                icon={<Icon as={FontAwesome} name="qrcode"/>}
                borderRadius="full"
              />
            ) 
          })} 
          />

          <Stack.Screen name="Scan" component={ScanPage} />
        </Stack.Navigator>
        <StatusBar style="auto" />
      </NavigationContainer>
    </NativeBaseProvider>
  );
}
```



## 4. เรียกใช้คำสั่งเปิดไปหน้า Scan Page


เปิดไฟล์​ `App.js`


```js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import { NativeBaseProvider, Box, IconButton, Icon } from "native-base";
import HomePage from './pages/home-page/HomePage';
import { FontAwesome } from '@expo/vector-icons';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import ScanPage from './pages/scan-page/ScanPage';

const Stack = createNativeStackNavigator();



export default function App() {
  return (
    <NativeBaseProvider>
      <NavigationContainer>
        <Stack.Navigator>
          <Stack.Screen name="Home" 
          component={HomePage} 
          
          {/* เพิ่ม object navigation เพื่อเรียกใช้ในคำสั่งของ onPress ของ IconButton  */}
          options={({ navigation }) => ({ 
            title: 'My home',
            headerRight: () => (
               {/* เรียกใช้ navigate function เพื่อเปิดไปยัง Screen ที่ชื่อว่า Scan */}
              <IconButton 
                icon={<Icon as={FontAwesome} name="qrcode"/>}
                borderRadius="full"
                onPress={() => navigation.navigate('Scan')}
              />
            ) 
          })} 
          />

          <Stack.Screen name="Scan" component={ScanPage} />

        </Stack.Navigator>
        <StatusBar style="auto" />
      </NavigationContainer>
    </NativeBaseProvider>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});

```

