
# 8. สร้าง Reducer Slice


## 1. สร้าง Reducer Slice สำหรับเก็บ chat history และจัดการเกี่ยวกับ action ที่เกิดกับ chat feature ของแอพ

สร้างไฟล์ `redux/chatSlice.ts`

```jsx
// redux/chatSlice.js
// ใช้ snippet rxslice

import { createSlice } from '@reduxjs/toolkit'

// กำหนดค่าเริ่มต้นของข้อมูล
const initialState = {
    
}

// สร้าง slice จาก function 
const chatSlice = createSlice({
  // กำหนดชื่อของ slice
  name: 'chatSlice',
  // กำหนด state เริ่มต้นของ slice 
  initialState,
  // กำหนด reducer ที่ตอนนี้ยังไม่มีการกำหนด action อะไร
  reducers: {}
});

// กำหนด action สำหรับส่งไปเรียกใช้ที่ส่วนอื่นของแอพ
export const {} = chatSlice.actions

export default chatSlice.reducer
```

## 2. เพิ่ม slice เข้าเป็น Reducer ของ Store

```jsx
// redux/store.ts
import { configureStore } from '@reduxjs/toolkit'
import chatSlice from './chatSlice' // Import your chatSlice

export const store = configureStore({
  reducer: {
    // กำหนดชื่อของ slice และ reducer ที่เราสร้างไว้
    chatroom: chatSlice
  }
})

export type RootState = ReturnType<typeof store.getState>
export type AppDispatch = typeof store.dispatch
```

## 3. ย้ายข้อมูล chat history จาก ChatHistory component ไปเก็บใน Redux Store

```jsx
// redux/chatSlice.ts

import { createSlice } from '@reduxjs/toolkit';

// สร้าง interface สำหรับเก็บข้อมูลของข้อความแชท
interface Message {
    text: string;
    isSender: boolean;
}

// กำหนดโครงสร้างของ state ของ slice
interface ChatState {
    messages: Message[];
}

// กำหนดค่าเริ่มต้นของ state โดยการกำหนด data type ของ state ให้เป็น ChatState
const initialState: ChatState = {
    messages: [
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
    },
});

export const { } = chatSlice.actions;

export default chatSlice.reducer;
