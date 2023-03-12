
# 4. สร้าง UI หน้า HomePage


เปิดไฟล์ `App.js` และสร้าง UI ตามด้านล่าง

```jsx
// App.js

import { StatusBar } from 'expo-status-bar';
import { StyleSheet, View } from 'react-native';
// Import component เพิ่มเติม
import { NativeBaseProvider, Box, HStack, Text } from "native-base";

export default function App() {
  return (
    <NativeBaseProvider>
          {/* กำหนดพื้นที่ safe area และสี */}
          <Box safeAreaTop bgColor="violet.800"/>
            {/* กำหนดส่วนที่เป็น  header */}
            <HStack bg="violet.800" px="3" py="3" w="100%">
                {/* กำหนดข้อความ */}
                <Text color="white" fontSize="20" fontWeight="bold">
                    Home
                </Text>
            </HStack>
      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}
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
