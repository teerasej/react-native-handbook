
# 11. เรียกใช้ OpenAI API

1. [สมัครใช้งาน OpenAI Developer](https://platform.openai.com/signup)
   - สามารถสร้าง account ใหม่ โดยการใช้ Google Account หรือ Microsoft Account ได้ 
   - จะมีการกรอกเบอร์โทรศัพท์ และรับ SMS-OTP เพื่อยืนยันตัวตน
2. หลังจากได้ account และ login เข้าไปใน Dashboard ได้แล้ว ให้[เข้าไปสร้าง OpenAI API Key ขึ้นมา และ copy มาเตรียมใช้งาน](https://platform.openai.com/account/api-keys) 

## 1. สร้าง async thunk ชื่อ askAI

สร้างไฟล์​ `src/redux/askAIThunk.js`

```js
// src/redux/askAIThunk.js

// import createAsyncThunk 
import { createAsyncThunk } from '@reduxjs/toolkit'
// import axios ในการส่ง request
import axios from 'axios';

// นำ key ของ OpenAI มาใช้งาน
const key = '';

// สร้าง AsyncThunk ชื่อ askAI
export const askAI = createAsyncThunk(
  // ตั้งชื่อ async thunk
  'user/askAI',

  // รับค่าที่เข้ามาใช้่งาน ในที่นี้คือ prompt message
  async (prompt) => {

    console.log('fetching openAI')

    // สร้าง JSON object ในการส่งไปที่ OpenAI API
    const jsonPrompt = JSON.stringify({
      "model": "gpt-3.5-turbo",
      "messages": [{ "role": "user", "content": prompt }]
    });

    console.log('Sending prompt:')
    console.log(jsonPrompt)

    // ใช้ axios ส่ง request โดยการกำหนด key และ json 
    const response = await axios.post(
      'https://api.openai.com/v1/chat/completions',
      jsonPrompt,
      {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${key}`
        }
      });

    console.log('got response:')
    console.log(response.data.choices[0].message.content);

    // ดึงเฉพาะส่วนข้อความที่ API ตอบกลับมา
    return response.data.choices[0].message.content;

  }
);

```

## 2. กำหนด extraReducer ลงใน Slice ที่ต้องการให้รับข้อมูลจาก thunk ในกรณีต่างๆ 

```js
// redux/chatSlice.js
import { createSlice } from '@reduxjs/toolkit'

// import askAI thunk เพื่อมากำหนด case ใน extraReducer
import { askAI } from './askAIThunk';

const initialState = {
  chatHistory: []
}

const chatSlice = createSlice({
  name: 'chatroom',
  initialState,
  reducers: {
    addUserMessage: (state, action) => {

      let chatMessage = {
        id: Math.floor(Math.random() * 1000),
        sender: 'Me',
        text: action.payload,
      };

      console.log(chatMessage);
      state.chatHistory.push(chatMessage);
    }
  },
  // กำหนด reducer พิเศษ ที่จะทำงานตามกรณีของ AsyncThunk
  extraReducers: (builder) => {
    // กำหนด case ที่จะทำงานจากกลไกของ async thunk
    builder
      .addCase(askAI.fulfilled, (state, action) => {
        console.log('succeeded');

        // ถ้าได้การทำงานของ Async thunk สมบูรณ์ ค่าที่ return ออกมาจะอยู่ใน payload 
        // ในที่นี้เราจะเอามาใส่เพิ่มในห้องแชท โดยกำหนดชื่อ sender เป็น GPT
        state.chatHistory.push({ 
          sender:'GPT', 
          text: action.payload, 
          id: Math.floor(Math.random() * 1000)
        });
        
      })
      // ในกรณีที่ Async Thunk ล้มเหลวจะเข้าเคสนี้
      .addCase(askAI.rejected, (state, action) => {
        console.log('failed');
        // แสดงข้อความ error ที่ได้
        console.error(action.error.message);
      });
  },
});

// export reducer สำหรับไปเรียกใช้ที่ component ที่ต้องการ
export const { addUserMessage } = chatSlice.actions

export default chatSlice.reducer
```

## 2. เรียกใช้ async thunk ใน ChatBoxComponent 

```jsx
// components/ChatBoxComponent.js

import { View, Text } from 'react-native'
import React, { useState } from 'react'
import { HStack, Icon, IconButton, Input } from 'native-base'
import { FontAwesome } from '@expo/vector-icons';
import { useDispatch } from 'react-redux';
import { addUserMessage } from './../redux/chatSlice';

// เรียกใช้ async thunk function ในการสร้าง action object เพื่อส่งให้กับ redux
import { askAI } from '../../redux/askAIThunk';

const ChatBoxComponent = () => {

    const [chatMessage, setChatMessage] = useState("")
    const dispatch = useDispatch();

    return (
        <>
            <HStack space={2} p={2}>
                <Input flex={7} placeholder="Talk to me..."
                    onChangeText={(text) => setChatMessage(text)}
                    value={chatMessage}
                />
                <IconButton
                    flex={1}
                    borderRadius="sm"
                    variant="solid"
                    icon={<Icon as={FontAwesome} name="send" size="sm" />}
                    onPress={() => {
                        console.log(`Sending message: ${chatMessage}`);

                        const action = addUserMessage(chatMessage);
                        dispatch(action);

                        // Dispatch Async thunk action โดยการส่งข้อความเป็น prompt
                        const asyncThunkAction = askAI(chatMessage);
                        dispatch(asyncThunkAction);

                        setChatMessage("");
                    }}
                />
            </HStack>
        </>
    )
}

export default ChatBoxComponent
```
