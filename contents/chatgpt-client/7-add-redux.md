
# 7. เพิ่มระบบ Redux

- [Redux for React](https://redux.js.org/basics/usage-with-react)

## 0. ติดตั้ง package สำหรับ Redux

ให้ทำการติดตั้ง package สำหรับ Redux ด้วยคำสั่ง

```bash
npm install @reduxjs/toolkit react-redux axios
```

## 1. สร้าง Redux Store 

ให้สร้างไฟล์ `redux/store.ts`

```jsx
// redux/store.ts
import { configureStore } from '@reduxjs/toolkit'

// ใช้ function configureStore สร้าง store เปล่าๆ ไม่มี reducer 
export const store = configureStore({
  reducer: {}
})

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch

```

## 2. นำ Redux Store มาใช้กับ App ผ่าน Provider component

เราจะทำการ import ทั้ง store ที่เตรียมไว้ และ **Provider** มาใช้กับ **App** ของเรา

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, KeyboardAvoidingView } from "react-native";
import { Link } from "expo-router";
import { VStack } from "@/components/ui/vstack";
import ChatHistory from "./components/ChatHistoryComponent";
import ChatBoxComponent from "./components/ChatBoxComponent";

// import Provider component จาก react-redux
import { Provider } from "react-redux";
// import store จาก redux/store.ts
import { store } from "@/redux/store";

export default function Home() {
  return (
    // ใส่ Provider รอบ component ที่เราต้องการให้เข้าถึง store ได้
    <Provider store={store}>
      <SafeAreaView style={{ flex: 1 }}>
        <KeyboardAvoidingView style={{ flex: 1 }} behavior="padding">
          <Box className="flex-1 bg-white h-[100vh] p-4">
            <VStack space="md" className="flex-1">

              <ChatHistory style={{ flex: 1 }} />
              <ChatBoxComponent />
            </VStack>
          </Box>
        </KeyboardAvoidingView>
      </SafeAreaView>
    </Provider>
  );
}

```

### Error? 

ตอนนี้จะมี error ด้านล่าง แสดงใน console ก็ไม่ต้องตกใจ เป็นเพราะเรายังไม่มี reducer ใน store

```bash
 ERROR  Store does not have a valid reducer. Make sure the argument passed to combineReducers is an object whose values are reducers.
```

