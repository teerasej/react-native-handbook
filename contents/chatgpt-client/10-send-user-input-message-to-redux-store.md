
# 10. สร้าง action ที่จะส่งข้อความ User เข้าไปใน Chat history


## 1. สร้าง Reducer function

```ts
// redux/chatSlice.ts

// import PayloadAction จาก redux toolkit เพื่อกำหนด type ของ action payload
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface Message {
    text: string;
    isSender: boolean;
}

interface ChatState {
    chatHistory: Message[];
}

const initialState: ChatState = {
    chatHistory: [
        { text: 'Hello!', isSender: true },
        { text: 'Hi there!', isSender: false },
        { text: 'How are you?', isSender: true },
        { text: 'I am good, thanks!', isSender: false },
    ],
};

const chatSlice = createSlice({
    name: 'chatSlice',
    initialState,
    reducers: {
        // สร้าง reducer function สำหรับเพิ่มข้อความใหม่เข้าไปใน chat history
        // โดยรับ action object ที่มี payload เป็นข้อความที่จะเพิ่มเข้าไปใน chat history
        // PayloadAction จะทำการกำหนด type ของ payload ให้เป็น Message interface
         addNewMessageToChatHistory: (state: ChatState, action: PayloadAction<Message>) => {
            console.log(`adding message to history: [${action.type}] ${action.payload}`);
        }
    },
});

// export reducer function ที่สร้างไว้ เพื่อให้สามารถเรียกใช้งานจากส่วนอื่นของโปรเจค
export const { addNewMessageToChatHistory } = chatSlice.actions;

export default chatSlice.reducer;
```

## 2. ส่งข้อมูล message ที่กดส่งจาก Input ไปที่ slice

```jsx
// app/components/ChatBoxComponent.tsx

import React, { useState } from 'react';
import { HStack } from '@/components/ui/hstack';
import { Input, InputField } from "@/components/ui/input";
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";
import { ChevronRightIcon } from '@/components/ui/icon';

// import useDispatch เพื่อใช้ dispatch action ไปยัง redux store
import { useDispatch } from 'react-redux';

// import AppDispatch จาก store เพื่อใช้ในการกำหนด type dispatch action
import { AppDispatch } from '@/redux/store';

// import action ที่สร้างไว้เพื่อส่งข้อความไปเก็บใน chat history
import { addNewMessageToChatHistory } from '@/redux/chatSlice';

const ChatBoxComponent = () => {

    // สร้าง state สำหรับเก็บข้อความที่ User พิมพ์เข้ามา
    const [message, setMessage] = useState('');

    // ใช้ dispatch เพื่อส่ง action ไปยัง redux store
    const dispatch: AppDispatch = useDispatch();
    
    // สร้าง function สำหรับเปลี่ยนข้อความในตัวแปร message เมื่อ User พิมพ์ข้อความเข้ามา 
    const handleTextChange = (text:string) => {
        setMessage(text);
    }

    // สร้าง function สำหรับส่งข้อความไปเก็บใน chat history
    const handleSendMessage = () => {

        // ถ้าข้อความไม่ใช่่ค่าว่าง จากการ trim()
        if (message.trim()) {

            // สร้าง action สำหรับเพิ่มข้อความใหม่เข้าไปใน chat history
            const action = addNewMessageToChatHistory({ text: message, isSender: true });

            // ส่ง action ไปยัง redux store
            dispatch(action);

            // ล้างข้อความใน input field หลังจากส่งข้อความไปแล้ว
            setMessage(''); 
        }
    };


    // InputField: สร้าง input field สำหรับ User พิมพ์ข้อความ, ส่งข้อความไปที่ handleTextChange เมื่อ User พิมพ์ข้อความ
    // Button: สร้างปุ่มสำหรับส่งข้อความ, ส่งข้อความไปที่ handleSendMessage เมื่อ User กดปุ่ม
    return (
        <HStack space="md" className="mt-auto">
            <Input style={{ flex: 1 }}>
                <InputField 
                    placeholder="Enter text" 
                    value={message} 
                    onChangeText={handleTextChange} 
                />
            </Input>
            <Button onPress={handleSendMessage}>
                <ButtonIcon as={ChevronRightIcon} />
            </Button>
        </HStack>
    );
};

export default ChatBoxComponent;
```
