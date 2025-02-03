
# 5. สร้าง Chat history UI

## 1. สร้างไฟล์ `components/ChatMessageComponent.tsx`

ใช้ snippet rnfe ได้

```jsx
// components/ChatMessageComponent.tsx

import { VStack } from '@/components/ui/vstack';
import { Box } from '@/components/ui/box';
import { Text } from '@/components/ui/text';
import { HStack } from '@/components/ui/hstack';

// ใช้ useColorScheme เพื่อให้สามารถเรียกดู theme ของระบบได้
import { useColorScheme } from 'react-native';

interface ChatMessageProps { // ประกาศ interface สำหรับ props ของ ChatMessage
  message: string; // ข้อความของแชท
  isSender: boolean; // ตัวบ่งชี้ว่าผู้ใช้เป็นผู้ส่งหรือไม่
}

function ChatMessage({ message, isSender }: ChatMessageProps) { // ประกาศฟังก์ชัน ChatMessage ที่รับ props

  // ใช้ useColorScheme เพื่อดึง theme ของระบบ
  const colorScheme = useColorScheme();
  // ตรวจสอบว่าเป็น dark mode หรือไม่
  const isDarkMode = colorScheme === 'dark';

  // 1. ใช้ HStack เพื่อจัดเรียงแนวนอนและกำหนด class ตาม isSender
  // 2. ใช้ Box เพื่อสร้างกล่องข้อความและกำหนด class ตาม isSender
  // 3. ใช้ Text เพื่อแสดงข้อความ และกำหนด class สีตาม isDarkMode 
  return (
    <HStack className={`flex ${isSender ? 'justify-end' : 'justify-start'}`}>
      <Box
        className={`p-3 m-2 rounded-md max-w-[70%] ${isSender ? 'bg-blue-500' : 'bg-green-300'}`}
      >
        <Text className={`${isDarkMode ? 'text-white' : 'text-black'}`}>{message}</Text>
      </Box>
    </HStack>
  );
}

export default ChatMessage; // ส่งออก ChatMessage 
```

## 2. สร้าง ChatHistory component

```jsx
// components/ChatHistoryComponent.tsx

import { VStack } from "@/components/ui/vstack";
import ChatMessage from "./ChatMessageComponent";

interface Message {
    text: string; // ข้อความในแชท
    isSender: boolean; // ระบุว่าข้อความนี้ถูกส่งโดยผู้ส่งหรือไม่
}

interface ChatHistoryProps {
    messages?: Message[]; // ข้อความในแชทที่ส่งเข้ามาเป็น props
}

export default function ChatHistory({ messages = [] }: ChatHistoryProps) {

    // ถ้าไม่มีข้อความในแชท ให้ใช้ข้อความตัวอย่าง
    if(messages.length == 0) {
        messages = [
            { text: 'Hello!', isSender: true },
            { text: 'Hi there!', isSender: false },
            { text: 'How are you?', isSender: true },
            { text: 'I am good, thanks!', isSender: false },
        ];
    }

    return (
        // ใช้ VStack เพื่อจัดเรียงข้อความในแนวตั้ง
        <VStack space="md" className="p-2">
            {messages.map((msg, index) => (
                // แสดงข้อความแต่ละข้อความโดยใช้ ChatMessage component
                <ChatMessage
                    key={index} // ใช้ index เป็น key เพื่อให้ React สามารถติดตาม element ได้
                    message={msg.text} // ข้อความในแชท
                    isSender={msg.isSender} // ระบุว่าข้อความนี้ถูกส่งโดยผู้ส่งหรือไม่
                />
            ))}
        </VStack>
    );
}

```

## 3. ทำให้ ChatHistoryComponent สามารถรับ Style props ได้

```jsx
// components/ChatHistoryComponent.tsx

import { VStack } from "@/components/ui/vstack";
import ChatMessage from "./ChatMessageComponent";

// ประกาศ interface สำหรับ props ของ ChatHistory
import { StyleProp, ViewStyle } from 'react-native';

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatHistoryProps {
    messages?: Message[];

    // ประกาศ style props ที่รับ StyleProp<ViewStyle>
    style?: StyleProp<ViewStyle>;
}

// กำหนดให้ ChatHistory สามารถรับ style props ได้
export default function ChatHistory({ messages = [], style }: ChatHistoryProps) {

    if(messages.length == 0) {
        messages = [
            { text: 'Hello!', isSender: true },
            { text: 'Hi there!', isSender: false },
            { text: 'How are you?', isSender: true },
            { text: 'I am good, thanks!', isSender: false },
        ];
    }

    // ส่งผ่าน style props ให้กับ VStack
    return (
        <VStack space="md" className="p-2" style={style}>
            {messages.map((msg, index) => (
                <ChatMessage
                    key={index}
                    message={msg.text}
                    isSender={msg.isSender}
                />
            ))}
        </VStack>
    );
}

```


## 4. นำ ChatHistoryComponent มาแสดงใน `app/index.tsx`

```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";
import { SafeAreaView, ScrollView } from "react-native";
import { Text } from "@/components/ui/text";

import { Link } from "expo-router";
import { Input, InputField } from "@/components/ui/input";
import { HStack } from "@/components/ui/hstack";
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";
import { ChevronRightIcon } from '@/components/ui/icon'
import { VStack } from "@/components/ui/vstack";
import ChatHistory from "./components/ChatHistoryComponent";


export default function Home() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Box className="flex-1 bg-white h-[100vh] p-4">
        <VStack space="md" className="flex-1">
          
          {/* แสดง chatHistory โดยให้กินพื้นที่ flex 1 */}
          <ChatHistory style={{flex:1 }}/>
          <HStack space="md" className="mt-auto">
            <Input style={{ flex: 1 }}>
              <InputField placeholder="Enter text" />
            </Input>
            <Button>
              <ButtonIcon as={ChevronRightIcon} />
            </Button>
          </HStack>
        </VStack>
      </Box>
    </SafeAreaView>
  );
}

```

- บันทึกไฟล์และรีเฟรชหน้าแอพ จะเห็น Chat History แสดงข้อความตัวอย่าง

## 5. Virtual Keyboard กับ Input component ที่หายไป 

- ในกรณีที่มี Virtual Keyboard ขึ้นมา และ Input component หายไป ให้ใช้ KeyboardAvoidingView component ครอบพื้นที่ที่มี Input component เพื่อให้ Virtual Keyboard ไม่กินพื้นที่ของ Input component


```jsx
// app/index.tsx

import React from "react";
import { Box } from "@/components/ui/box";

// import SafeAreaView เพื่อให้แอพพลิเคชันทำงานได้บนสมาร์ทโฟนและแท็บเล็ต
import { SafeAreaView, ScrollView, KeyboardAvoidingView } from "react-native";
import { Text } from "@/components/ui/text";

import { Link } from "expo-router";
import { Input, InputField } from "@/components/ui/input";
import { HStack } from "@/components/ui/hstack";
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";
import { ChevronRightIcon } from '@/components/ui/icon'
import { VStack } from "@/components/ui/vstack";
import ChatHistory from "./components/ChatHistoryComponent";


export default function Home() {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      {/* ครอบพื้นที่ เพื่อให้ keyboard ไม่กินพื้นที่จอเดียวกันเวลาใช้งาน */}
      <KeyboardAvoidingView style={{ flex: 1 }} behavior="padding">
        <Box className="flex-1 bg-white h-[100vh] p-4">
          <VStack space="md" className="flex-1">

            {/* แสดง chatHistory โดยให้กินพื้นที่ flex 1 */}
            <ChatHistory style={{ flex: 1 }} />
            <HStack space="md" className="mt-auto">
              <Input style={{ flex: 1 }}>
                <InputField placeholder="Enter text" />
              </Input>
              <Button>
                <ButtonIcon as={ChevronRightIcon} />
              </Button>
            </HStack>
          </VStack>
        </Box>
      </KeyboardAvoidingView>
    </SafeAreaView>
  );
}

```

- บันทึกไฟล์และรีเฟรชหน้าแอพ จะเห็นหน้า Home มี Chat History และ Input box และ Virtual Keyboard ไม่กินพื้นที่ของ Input box

## 6. Chat History ที่เกินขนาดหน้าจอ 

- ในกรณีที่ Chat History มีข้อความมากเกินไป จะทำให้ข้อความเกินขนาดหน้าจอ และไม่สามารถ scroll ได้
- ในกรณีนี้ จะใช้ ScrollView component ครอบส่วนแสดงข้อความ เพื่อให้ Chat History สามารถ scroll ได้

```jsx
// app/components/ChatHistoryComponent.tsx

// import ScrollView เพื่อใช้ในการสร้างพื้นที่ scrollable
import { ScrollView, StyleProp, ViewStyle } from 'react-native';
import { VStack } from "@/components/ui/vstack";
import ChatMessage from "./ChatMessageComponent";

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatHistoryProps {
    messages?: Message[];
    style?: StyleProp<ViewStyle>;
}

export default function ChatHistory({ messages = [], style }: ChatHistoryProps) {

    if(messages.length == 0) {
        // เพิ่มข้อความตัวอย่าง เพื่อทดสอบ layout ที่เกินหน้าจอของ chat history
        messages = [
            { text: 'Hello!', isSender: true },
            { text: 'Hi there!', isSender: false },
            { text: 'How are you?', isSender: true },
            { text: 'I am good, thanks!', isSender: false },
            { text: 'What are you up to?', isSender: true },
            { text: 'Just working on a project.', isSender: false },
            { text: 'Sounds interesting!', isSender: true },
            { text: 'Yeah, it is quite challenging.', isSender: false },
            { text: 'Do you need any help?', isSender: true },
            { text: 'I think I got it, but thanks!', isSender: false },
        ];
    }

    // ทำให้พื้นที่ของ chat history scroll ตามความยาวได้ 
    // และเอา style props ใส่ให้กับ ScrollView แทน Vstack
    return (
        <ScrollView style={style}>
            <VStack space="md" className="p-2">
                {messages.map((msg, index) => (
                    <ChatMessage
                        key={index}
                        message={msg.text}
                        isSender={msg.isSender}
                    />
                ))}
            </VStack>
        </ScrollView>
    );
}

```

- บันทึกไฟล์และรีเฟรชหน้าแอพ จะเห็น Chat History สามารถ scroll ได้

## 7. เสร็จสิ้นขั้นตอน

