
# 6. สร้าง Chat box UI


## 1. สร้างไฟล์ `components/ChatBoxComponent.js`

ใช้ snippet `rnfe` ได้

```jsx
import { View, Text } from 'react-native'
import React from 'react'

// Import component ที่จำเป็น
import { HStack, Icon, IconButton, Input } from 'native-base'

// Import icon จาก FontAwesome
import { FontAwesome } from '@expo/vector-icons';


const ChatBoxComponent = () => {

    return (
        <>
            <HStack space={2} p={2}>
                {/* สร้่างช่องแชท กินพื้นที่ใน hstack 7 ช่อง */}
                <Input flex={7} placeholder="Talk to me..."  />
                {/* สร้่างปุ่มส่งข้อความ กินพื้นที่ใน hstack 1 ช่อง */}
                {/* กำหนด Icon FontAwesome ชื่อ send */}
                <IconButton
                    flex={1}
                    borderRadius="sm"
                    variant="solid"
                    icon={<Icon as={FontAwesome} name="send" size="sm"
                    />}
                />
            </HStack>
        </>
    )
}

export default ChatBoxComponent
```

## 2. นำ ChatBoxComponent มาแสดงใน App.js

```jsx
// App.js

import { StatusBar } from 'expo-status-bar';

// import KeyboardAvoidingView
import { NativeBaseProvider, Box, HStack, Text, VStack, KeyboardAvoidingView } from "native-base";

import ChatHistoryComponent from './components/ChatHistoryComponent';

// import component chat box
import ChatBoxComponent from './components/ChatBoxComponent';

export default function App() {

  return (
    <NativeBaseProvider>
     
        <Box safeAreaTop bg Color="violet.800" />
        <HStack bg="violet.800" px="3" py="3" w="100%">
          <Text color="white" fontSize="20" fontWeight="bold">
            Home
          </Text>
        </HStack>

        {/* ครอบ component เพื่อให้จัดการเรื่อง keyboard layout */}
        <KeyboardAvoidingView behavior='padding' flex={1}>
          <VStack w="100%" flex={1}>
            <ChatHistoryComponent flex={1}/>

            {/* แสดง chatbox โดยไม่กำหนดค่า flex เพื่อให้ ChatHistory กินพื้นที่ที่เหลือทั้งหมด */}
            <ChatBoxComponent/>

          </VStack>
        </KeyboardAvoidingView>
        {/* กันพื้นที่ด้านล่างออกจากการตกขอบจอ */}
        <Box safeAreaBottom />

      <StatusBar style="auto" />
    </NativeBaseProvider>
  );
}
```
