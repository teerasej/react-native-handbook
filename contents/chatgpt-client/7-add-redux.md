
# 7. เพิ่มระบบ Redux

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

import { NativeBaseProvider, Box, HStack, Text, VStack } from "native-base";
import ChatHistoryComp from './components/ChatHistoryComp';
import ChatBox from './components/ChatBox';

// เรียกใช้ provider และ store
import { Provider } from 'react-redux';
import store from "./redux/store";

export default function App() {

  return (
    // ครอบ component ทั้งหมดด้วย Provider ที่มีการใส่ store ลงไปใช้งาน
    <Provider store={store}>
      <NativeBaseProvider>
        <Box safeAreaTop bg Color="violet.800" />
        <HStack bg="violet.800" px="3" py="3" w="100%">
          <Text color="white" fontSize="20" fontWeight="bold">
            Home
          </Text>
        </HStack>

        <VStack w="100%" flex={1}>
          <ChatHistoryComp flex={1} />
          <ChatBox />
        </VStack>
        <StatusBar style="auto" />
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

