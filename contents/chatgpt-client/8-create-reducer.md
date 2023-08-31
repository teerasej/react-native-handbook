
# 8. สร้าง Slice


สร้างไฟล์ `redux/chatSlice.js`

```jsx
// redux/chatSlice.js
// ใช้ snippet rxslice

import { createSlice } from '@reduxjs/toolkit'

// กำหนดค่าเริ่มต้นของข้อมูลที่ชื่อ chatHistory เป็นค่า undefined
const initialState = {
    chatHistory: []
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


## 4. เพิ่ม slice เข้าเป็น Reducer ของ Store

```jsx
// redux/store.js

import { configureStore } from '@reduxjs/toolkit'
// import slice ที่ต้องการ
import chatSlice from './chatSlice.js'


export default configureStore({
  reducer: {
    // กำหนด slice ให้เป็น reducer ของ store 
    chatroom: chatSlice
  }
})
```