
# 11. เรียกใช้ OpenAI API

1. [สมัครใช้งาน OpenAI Developer](https://platform.openai.com/signup)
   - สามารถสร้าง account ใหม่ โดยการใช้ Google Account หรือ Microsoft Account ได้ 
   - จะมีการกรอกเบอร์โทรศัพท์ และรับ SMS-OTP เพื่อยืนยันตัวตน
2. หลังจากได้ account และ login เข้าไปใน Dashboard ได้แล้ว ให้[เข้าไปสร้าง OpenAI API Key ขึ้นมา และ copy มาเตรียมใช้งาน](https://platform.openai.com/account/api-keys) 

## เรียนรู้เพิ่มเติม

- [เริ่มต้นเรียนรู้ ใช้งาน Azure OpenAI Service](https://learn.nextflow.in.th/azure-openai-service)
- [เริ่มต้นเรียนรู้ ทำแอพ AI ด้วย Semantic Kernel ฉบับคนใช้ Python
](https://learn.nextflow.in.th/getting-started-with-semantic-kernel)

## Key 

```
98db6ffc7e3447678d4ee3ba4ec3d45a
```

## 1. สร้าง async thunk ชื่อ askAI

สร้างไฟล์​ `redux/askAIThunk.ts`

```js
// src/redux/askAIThunk.ts


// import createAsyncThunk จาก redux toolkit
import { createAsyncThunk } from '@reduxjs/toolkit';

// import axios เพื่อใช้ส่ง request ไปยัง Azure OpenAI API
import axios from 'axios';

// กำหนด interface ของ request ที่จะส่งไปยัง API
interface AskAIRequest {
  prompt: string;
}

// กำหนด interface ของ JSON response ที่จะได้จาก API
interface AskAIResponse {
  // ในกรณีที่ API ส่งกลับมาเป็น JSON ที่มี key ชื่อ choices ซึ่งเป็น array ของ object
  choices: {
    message: {
      content: string;
    };
  }[];
}

// กำหนด interface ของ error ที่จะได้จาก API
interface AskAIError {
  message: string;
}

// API key
const key = '';

// สร้าง async thunk ชื่อ askAI โดยรับ prompt และส่ง request ไปยัง OpenAI API
export const askAI = createAsyncThunk<string, AskAIRequest, { rejectValue: AskAIError }>(
  // กำหนดชื่อ action thunk และรับ prompt 
  'user/askAI',
  async ({ prompt }, { rejectWithValue }) => {
    console.log('Asking AI...');

    // กำหนด JSON payload ที่จะส่งไปยัง API
    const jsonPrompt = JSON.stringify({
      messages: [
        {
          role: 'system',
          content: 'You are an AI assistant that helps people find information.',
        },
        {
          role: 'user',
          content: prompt,
        },
      ],
      temperature: 0.7,
      top_p: 0.95,
      frequency_penalty: 0,
      presence_penalty: 0,
      max_tokens: 800,
      stop: null,
    });

    console.log('Sending prompt:');
    console.log(jsonPrompt);

    try {
      // ส่ง request ไปยัง API โดยใช้ axios
      const response = await axios.post<AskAIResponse>(
        'https://openai-nextflow.openai.azure.com/openai/deployments/gpt-4/chat/completions?api-version=2024-02-15-preview',
        jsonPrompt,
        {
          headers: {
            'Content-Type': 'application/json',
            'api-key': key,
          },
        }
      );

      // แสดง response ที่ได้จาก API
      console.log('Got response:');
      console.log(response.data.choices[0].message.content);

      // ส่งข้อความที่ได้จาก API กลับไปให้ component
      return response.data.choices[0].message.content;

     // ในกรณีที่เกิด error ในการส่ง request หรือได้ response ที่ไม่ถูกต้อง
    } catch (error: any) {
      console.error('Error fetching OpenAI:', error);
      // Return a rejected value with a custom error message
      return rejectWithValue({ message: error.message });
    }
  }
);
```

## 2. กำหนด extraReducers ลงใน Slice ที่ต้องการให้รับข้อมูลจาก thunk ในกรณีต่างๆ 

```js
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

// import async thunk ที่เราสร้างไว้
import { askAI } from './askAIThunk';

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
        addNewMessageToChatHistory: (state: ChatState, action: PayloadAction<Message>) => {
            console.log(`adding message to history: [${action.type}] ${action.payload}`);
            state.chatHistory.push(action.payload);
        }
    },

    // กำหนด extraReducers ในกรณีที่ thunk ส่งข้อมูลกลับมาเรียบร้อย ผ่าน builder
    extraReducers: (builder) => {

        // ในกรณีที่ thunk ส่งข้อมูลกลับมาเรียบร้อย
        builder.addCase(askAI.fulfilled, (state, action: PayloadAction<string>) => {
            console.log(`adding AI response to history: [${action.type}] ${action.payload}`);

            // เพิ่มข้อความที่ได้จาก AI ลงใน chat history
            state.chatHistory.push({ text: action.payload, isSender: false });
        });

        // ในกรณีที่ thunk ส่งข้อมูลกลับมาแต่เกิด error
        builder.addCase(askAI.rejected, (state, action) => {
            console.error(`AI request failed: [${action.type}] ${action.error.message}`);

            // เพิ่มข้อความแจ้งเตือนลงใน chat history
            state.chatHistory.push({ text: 'AI request failed. Please try again.', isSender: false });
        });
    }
});

export const { addNewMessageToChatHistory } = chatSlice.actions;

export default chatSlice.reducer;
```

## 2. เรียกใช้ async thunk ใน ChatBoxComponent 

```jsx
// app/components/ChatBoxComponent.tsx

import React, { useState } from 'react';
import { HStack } from '@/components/ui/hstack';
import { Input, InputField } from "@/components/ui/input";
import { Button, ButtonIcon, ButtonText } from "@/components/ui/button";
import { ChevronRightIcon } from '@/components/ui/icon';
import { useDispatch } from 'react-redux';
import { AppDispatch } from '@/redux/store';
import { addNewMessageToChatHistory } from '@/redux/chatSlice';

// import async thunk ที่เราสร้างไว้
import { askAI } from '@/redux/askAIThunk';

const ChatBoxComponent = () => {
    const [message, setMessage] = useState('');
    const dispatch: AppDispatch = useDispatch();

    const handleTextChange = (text:string) => {
        setMessage(text);
    }

    const handleSendMessage = () => {
        if (message.trim()) {
            const action = addNewMessageToChatHistory({ text: message, isSender: true });
            dispatch(action);

            // สร้าง action thunk สำหรับส่งข้อความไปให้ AI
            const actionThunk = askAI({ prompt: message });
            // ส่ง action thunk ไปยัง redux store
            dispatch(actionThunk);
            
            setMessage(''); ]
        }
    };

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
