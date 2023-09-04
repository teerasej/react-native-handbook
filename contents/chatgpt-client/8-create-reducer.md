
# 8. สร้าง Reducer Slice


## 8.1 สร้าง Reducer Slice สำหรับเก็บ chat history และจัดการเกี่ยวกับ action ที่เกิดกับ chat feature ของแอพ

สร้างไฟล์ `src/redux/chatSlice.js`

```jsx
// src/redux/chatSlice.js
// ใช้ snippet rxslice

import { createSlice } from '@reduxjs/toolkit'

// กำหนดค่าเริ่มต้นของข้อมูลที่ชื่อ chatHistory เป็นค่า undefined
const initialState = {
    // สร้าง item ที่เป็นตัวแทนของข้อความ
    chatHistory: [
      { sender: 'Me', text:'Oh yeah!'}
    ]
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


## 8.2 เพิ่ม slice เข้าเป็น Reducer ของ Store

```jsx
// src/redux/store.js

import { configureStore } from '@reduxjs/toolkit'
// import slice ที่ต้องการ
import chatSlice from './chatSlice.js'


export default configureStore({
  reducer: {
    // กำหนด slice ให้เป็น reducer ของ store โดยตั้งชื่อ slice นี้ ว่า chatroom 
    chatroom: chatSlice
  }
})
```