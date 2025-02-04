
# ใช้งาน Stack Navigation ใน 

## 1. ปรับ Layout ใน `app/_layout.tsx` เป็น Stack

ให้ทำการปรับ Layout ใน `app/_layout.tsx` ให้เป็น Stack ดังนี้

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

  // เปลี่ยนจาก <Slot/> เป็น <Stack/>
  return (
    <GluestackUIProvider mode={colorScheme === "dark" ? "dark" : "light"}>
      <ThemeProvider value={colorScheme === "dark" ? DarkTheme : DefaultTheme}>
        <Provider store={store}>
          <Stack/>
        </Provider>
      </ThemeProvider>
    </GluestackUIProvider>
  );
}
```

## 2. สร้างหน้า chatroom ใน `app/chat.tsx`

สร้างหน้า chatroom ใน `app/chat.tsx` ดังนี้

```tsx

// app/chat.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, KeyboardAvoidingView } from "react-native";
import { VStack } from "@/components/ui/vstack";
import ChatHistory from "./components/ChatHistoryComponent";
import ChatBoxComponent from "./components/ChatBoxComponent";


import { Stack } from "expo-router";

export default function ChatPage() {

  // ใช้ Stack.Screen ในการกำหนด title ของหน้าแอพ
  return (
    
    <>
      <Stack.Screen 
        options={{
          title: "Chat with AI"
        }}
      />
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
    </>
  );
}
```

## 3. สร้างเมนูใน `app/index.tsx` 

เปลี่ยน layout ของ index.tsx และสร้างเมนูใน `app/index.tsx` ดังนี้

```tsx
// app/index.tsx

import React from "react";
import { Link } from "expo-router";
import { VStack } from "@/components/ui/vstack";
import { Stack } from "expo-router";
import { Button, ButtonText } from "@/components/ui/button";

export default function Home() {

  // ใช้ Stack.Screen ในการกำหนด title ของหน้าแอพ
  // ใช้ <Link> ในการกำหนด URL ของการเปลี่ยนหน้าแอพ โดยการกำหนด url ตามที่อยู่และชื่อไฟล์ tsx ใน /app โดยตรง
  // ใช้ Props `asChild` ในการส่งคุณสมบัติของ link ไปที่ Button หรือ Component ที่อยู่ใน link ได้
  return (
    <>
      <Stack.Screen
        options={{
          title: "Home"
        }}
      />

      <VStack className="flex-1 p-2" space="md">
        <Link href="/chat" asChild>
          <Button>
            <ButtonText>Ask AI</ButtonText>
          </Button>
        </Link>
      </VStack>

    </>
  );
}
```

## 4. บันทึกไฟล์ และทดสอบการใช้งาน Stack Navigation

จะเห็นว่าหน้าแอพจะมีเมนู Ask AI และหน้า Chat with AI ที่สามารถเข้าไปใช้งานได้



