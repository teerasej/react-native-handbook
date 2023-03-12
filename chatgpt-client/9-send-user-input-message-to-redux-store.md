
# 9. สร้าง action ที่จะส่งข้อความ User เข้าไปใน Chat history

## 1. สร้าง Reducer function


```js
// redux/chatHistorySlice.js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
    chatHistory: []
}
 
const chatHistorySlice = createSlice({
  name: 'chatroom',
  initialState,
  reducers: {
    // กำหนด function ชื่อ addUserMessage เป็น reducer function
    //   - โดยที่ตัว function จะทำงานเมื่อมีการเรียกใช้จากภายใน component
    //   - ทุกครั้งที่ function reducer ทำงาน จะได้รับ state object ล่าสุด และ action ที่ส่งมาจาก component เสมอ
    addUserMessage: (state, action) => {

      let chatMessage = {
        sender: 'Me',
        text: action.payload
      };

      // แสดงข้อความใน console เพื่อเช็คความถูกต้อง
      console.log(chatMessage);
        
      // เพิ่มข้อความลงไปใน chatHistory ของ slice's state เพื่อที่จะนำไปใช้ใน component
      state.chatHistory.push(chatMessage);
    }
  }
});

// export reducer สำหรับไปเรียกใช้ที่ component ที่ต้องการ
export const { addUserMessage } = chatHistorySlice.actions

export default chatHistorySlice.reducer
```

## 2. ส่งข้อมูล message ที่กดส่งจาก Input ไปที่ slice

เปิดไฟล์ `components/ChatBox.js`


```jsx
// components/ChatBox.js
import { View, Text } from 'react-native'
import React, { useState } from 'react'
import { HStack, Icon, IconButton, Input } from 'native-base'
import { FontAwesome } from '@expo/vector-icons';

// เรียกใช้ useDispatch hook
import { useDispatch } from 'react-redux';
// เรียกใช้ reducer function ในการสร้าง action object เพื่อส่งให้กับ redux
import { addUserMessage } from './../redux/chatHistorySlice';

const ChatBox = () => {

    const [chatMessage, setChatMessage] = useState("")

    // สร้าง dispatch function 
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

                        // ใส่ข้อความที่พิมพ์ลงไปใน action object โดยการเรียกใช้ reducer function
                        const action = addUserMessage(chatMessage);

                        // ส่ง action ไปที่ slice ผ่าน dispatch function
                        dispatch(action);

                        setChatMessage("");
                    }}
                />
            </HStack>
        </>
    )
}

export default ChatBox
```