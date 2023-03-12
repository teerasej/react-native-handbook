
# 9. เพิ่มระบบ Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

## 1. สร้าง Redux Store 

ให้สร้างไฟล์ `redux/store.js`

```jsx
// redux/store.js
import { configureStore } from '@reduxjs/toolkit'

// ใช้ function configureStore สร้าง store เปล่าๆ ไม่มี reducer 
export default configureStore({
  reducer: {}
})

```

## 2. นำ Redux Store มาใช้กับ App ผ่าน Provider component

เราจะทำการ import ทั้ง store ที่เตรียมไว้ และ **Provider** มาใช้กับ **App** ของเรา

```jsx
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, SafeAreaView } from 'react-native';
import { NativeBaseProvider, Box, IconButton, Icon } from "native-base";
import HomePage from './pages/home-page/HomePage';
import { FontAwesome } from '@expo/vector-icons';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import ScanPage from './pages/scan-page/ScanPage';

// เรียกใช้ provider และ store
import { Provider } from 'react-redux';
import store from "./redux/store";

const Stack = createNativeStackNavigator();



export default function App() {
  // ครอบ component ทั้งหมดด้วย Provider ที่มีการใส่ store ลงไปใช้งาน
  return (
    <Provider store={store}>
      <NativeBaseProvider>
        <NavigationContainer>
          <Stack.Navigator>
            <Stack.Screen name="Home"
              component={HomePage}
              options={({ navigation }) => ({
                title: 'My home',
                headerRight: () => (
                  <IconButton
                    icon={<Icon as={FontAwesome} name="qrcode" />}
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
    </Provider>
  );
}
```

### Error? 

ตอนนี้จะมี error ด้านล่าง แสดงใน console ก็ไม่ต้องตกใจ เป็นเพราะเรายังไม่มี reducer ใน store

```bash
 ERROR  Store does not have a valid reducer. Make sure the argument passed to combineReducers is an object whose values are reducers.
```

## 3. สร้าง Reducer Slice

สร้างไฟล์ `redux/barcodeDataSlice.js`

```jsx
// redux/barcodeDataSlice.js
// ใช้ snippet rxslice

import { createSlice } from '@reduxjs/toolkit'

// กำหนดค่าเริ่มต้นของข้อมูลที่ชื่อ barcodeData เป็นค่า undefined
const initialState = {
    barcodeData: undefined
}

// สร้าง slice จาก function 
const barcodeDataSlice = createSlice({
  // กำหนดชื่อของ slice
  name: 'barcodeData',
  // กำหนด state เริ่มต้นของ slice 
  initialState,

  // กำหนด reducer ที่ตอนนี้ยังไม่มีการกำหนด action อะไร
  reducers: {}
});

// กำหนด action สำหรับส่งไปเรียกใช้ที่ส่วนอื่นของแอพ
export const {} = barcodeDataSlice.actions

export default barcodeDataSlice.reducer
```


## 4. เพิ่ม slice เข้าเป็น Reducer ของ Store

```jsx
// redux/store.js

import { configureStore } from '@reduxjs/toolkit'
// import slice ที่ต้องการ
import barcodeDataSlice from './barcodeDataSlice'


export default configureStore({
  reducer: {
    // กำหนด slice ให้เป็น reducer ของ store 
    barcode: barcodeDataSlice
  }
})
```
