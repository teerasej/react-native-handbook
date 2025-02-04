
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
// app/_layout.tsx

import FontAwesome from "@expo/vector-icons/FontAwesome";
import {
  DarkTheme,
  DefaultTheme,
  ThemeProvider,
} from "@react-navigation/native";
import { useFonts } from "expo-font";
import * as SplashScreen from "expo-splash-screen";
import { useEffect, useState } from "react";
import { GluestackUIProvider } from "@/components/ui/gluestack-ui-provider";
import { useColorScheme } from "@/components/useColorScheme";
import { Slot, Stack } from "expo-router";

import "../global.css";
import { Provider } from "react-redux";
import { store } from "@/redux/store";



export {
  // Catch any errors thrown by the Layout component.
  ErrorBoundary,
} from "expo-router";

// export const unstable_settings = {
//   // Ensure that reloading on `/modal` keeps a back button present.
//   initialRouteName: "gluestack",
// };

// Prevent the splash screen from auto-hiding before asset loading is complete.
SplashScreen.preventAutoHideAsync();

export default function RootLayout() {
  const [loaded, error] = useFonts({
    SpaceMono: require("../assets/fonts/SpaceMono-Regular.ttf"),
    ...FontAwesome.font,
  });

  const [styleLoaded, setStyleLoaded] = useState(false);
  // Expo Router uses Error Boundaries to catch errors in the navigation tree.
  useEffect(() => {
    if (error) throw error;
  }, [error]);

  useEffect(() => {
    if (loaded) {
      SplashScreen.hideAsync();
    }
  }, [loaded]);

  // useLayoutEffect(() => {
  //   setStyleLoaded(true);
  // }, [styleLoaded]);

  // if (!loaded || !styleLoaded) {
  //   return null;
  // }

  return <RootLayoutNav />;
}

function RootLayoutNav() {
  const colorScheme = useColorScheme();

  // กำหนด Provider และเชื่อมต่อ store กับ App ของเรา
  return (
    <GluestackUIProvider mode={colorScheme === "dark" ? "dark" : "light"}>
      <ThemeProvider value={colorScheme === "dark" ? DarkTheme : DefaultTheme}>
      
        <Provider store={store}>
          <Slot/>
        </Provider>

      </ThemeProvider>
    </GluestackUIProvider>
  );
}


```

### Error? 

ตอนนี้จะมี error ด้านล่าง แสดงใน console ก็ไม่ต้องตกใจ เป็นเพราะเรายังไม่มี reducer ใน store

```bash
 ERROR  Store does not have a valid reducer. Make sure the argument passed to combineReducers is an object whose values are reducers.
```

