
# 3. Setup NativeBase

ในที่นี้เราใช้ UI Framework ที่ชื่อ [Nativebase](https://nativebase.io/) 

ทำการเขียนส่วนของ App.js ใหม่ โดยใช้ `NativeBaseProvider` component 

```jsx
// App.js
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View } from 'react-native';
import { NativeBaseProvider, Box } from "native-base";

export default function App() {
  return (
    <NativeBaseProvider>
        <Box>
          Hello
        </Box>
      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}
```


