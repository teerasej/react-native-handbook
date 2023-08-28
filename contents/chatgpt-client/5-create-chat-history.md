
# 5. สร้าง Chat history UI

## 1. สร้างไฟล์ `components/ChatHistory.js`

ใช้ snippet rnfe ได้

```jsx
import { View, Text } from 'react-native'
import React from 'react'
import { FlatList } from 'native-base'

const ChatHistoryComp = () => {
  return (
    <>
        {/* ใส่ FlatList สำหรับแสดง Chat history */}
        <FlatList></FlatList>
    </>
  )
}

export default ChatHistoryComp
```

## 2. นำ ChatHistory มาแสดงใน App.js

```jsx
// App.js

import { StatusBar } from 'expo-status-bar';

// Import component เพิ่มเติม
import { NativeBaseProvider, Box, HStack, Text, VStack } from "native-base";

// import component chat history
import ChatHistoryComp from './components/ChatHistoryComp';

export default function App() {

  return (
    <NativeBaseProvider>
     
        <Box safeAreaTop bg Color="violet.800" />
        <HStack bg="violet.800" px="3" py="3" w="100%">
          <Text color="white" fontSize="20" fontWeight="bold">
            Home
          </Text>
        </HStack>

        <VStack w="100%" flex={1}>
          
          {/* แสดง chathistory โดยกำหนดกินพื้นที่ 1 ช่องจากทั้งหมด */}
          <ChatHistoryComp flex={1}/>

        </VStack>
      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}
```