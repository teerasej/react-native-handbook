
# 6. สร้าง Chat box UI


## 1. สร้างไฟล์ `app/components/ChatBoxComponent.tsx`

ใช้ snippet `rnfe` ได้
และย้ายโค้ดมาจาก `app/index.tsx` ไปยังไฟล์ใหม่

```jsx
// app/components/ChatBoxComponent.tsx

import React from 'react'

import { HStack } from '@/components/ui/hstack';
import { Input, InputField } from "@/components/ui/input";
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";
import { ChevronRightIcon } from '@/components/ui/icon'


const ChatBoxComponent = () => {

    return (
        <HStack space="md" className="mt-auto">
            <Input style={{ flex: 1 }}>
                <InputField placeholder="Enter text" />
            </Input>
            <Button>
                <ButtonIcon as={ChevronRightIcon} />
            </Button>
        </HStack>
    )
}

export default ChatBoxComponent
```

## 2. นำ ChatBoxComponent มาแสดงใน `app/index.tsx`

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, KeyboardAvoidingView } from "react-native";

import { Link } from "expo-router";
import { VStack } from "@/components/ui/vstack";
import ChatHistory from "./components/ChatHistoryComponent";

// import ChatBoxComponent จาก app/components/ChatBoxComponent
import ChatBoxComponent from "./components/ChatBoxComponent";


export default function Home() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <KeyboardAvoidingView style={{ flex: 1 }} behavior="padding">
        <Box className="flex-1 bg-white h-[100vh] p-4">
          <VStack space="md" className="flex-1">

            <ChatHistory style={{ flex: 1 }} />
            <ChatBoxComponent/>
          </VStack>
        </Box>
      </KeyboardAvoidingView>
    </SafeAreaView>
  );
}

```
