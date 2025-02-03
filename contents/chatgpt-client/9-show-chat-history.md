
# 9. แสดงข้อความใน Chat History component

## 1. ใช้ useSelector เพื่อดึงข้อมูลจาก slice's state มาใช้กับ FlatList

```jsx
// app/components/ChatHistoryComponent.tsx

import { ScrollView, StyleProp, ViewStyle } from 'react-native';
import { VStack } from "@/components/ui/vstack";
import ChatMessage from "./ChatMessageComponent";

// ใช้ useSelector เพื่อดึงข้อมูลจาก slice's state 
import { useSelector } from 'react-redux';

// ดึง type RootState มาใช้งานสำหรับ state ที่ได้จาก useSelector
import { RootState } from '@/redux/store';

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatHistoryProps {
    messages?: Message[];
    style?: StyleProp<ViewStyle>;
}

export default function ChatHistory({ messages = [], style }: ChatHistoryProps) {

    // ดึงข้อมูล chatHistory จาก chatroom reducer จาก slice's state มาใช้งาน
    const chatHistory = useSelector((state: RootState) => state.chatroom.chatHistory)

    // ถ้าไม่มีข้อมูลใน chatHistory ให้ใช้ข้อมูลที่ส่งเข้ามา
    messages = chatHistory || [];

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

## 2. ควบคุมให้ ScrollView เลื่อนลงมาล่างสุดเสมอ

```jsx
// app/components/ChatHistoryComponent.tsx

// ใช้ useEffect เพื่อทำงานหลังจาก render เสร็จ
// ใช้ useRef เพื่อเก็บค่า ScrollView ไว้ในตัวแปร
import React, { useRef } from 'react';
import { ScrollView, StyleProp, ViewStyle } from 'react-native';
import { VStack } from "@/components/ui/vstack";
import ChatMessage from "./ChatMessageComponent";

import { useSelector } from 'react-redux';
import { RootState } from '@/redux/store';

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatHistoryProps {
    messages?: Message[];
    style?: StyleProp<ViewStyle>;
}

export default function ChatHistory({ messages = [], style }: ChatHistoryProps) {

    // ใช้ useRef เพื่อเก็บค่า ScrollView component ไว้ในตัวแปร
    const scrollViewRef = useRef<ScrollView>(null);

    const chatHistory = useSelector((state: RootState) => state.chatroom.chatHistory);
    messages = chatHistory || [];

    // สร้าง function ที่จะใช้ในการเลื่อน ScrollView ไปด้านล่างสุด เวลาขนาดของ ScrollView มีการเปลี่ยนแปลงจากจำนวน component ที่เพิ่มขึ้น
    const handleContentSizeChange = () => {
        scrollViewRef.current?.scrollToEnd({ animated: true });
    };

    // ส่ง ref object ของ ScrollView ไปให้ ScrollView component เพื่อใช้ในการเลื่อน ScrollView ไปด้านล่างสุด
    // ใช้ onContentSizeChange เพื่อทำงานเมื่อขนาดของ ScrollView มีการเปลี่ยนแปลง
    return (
        <ScrollView style={style} ref={scrollViewRef} onContentSizeChange={handleContentSizeChange}>
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

