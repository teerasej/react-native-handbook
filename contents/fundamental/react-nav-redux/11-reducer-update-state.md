

# 11. กำหนด Action Type ที่ Reducer ต้องเอาข้อมูลมาอัพเดตใน redux state

เปิดไฟล์ `redux/note.reducer.js`

เราสามารถกำหนด switch case ที่เทียบค่า type ของ action ที่ส่งเข้ามาใน reducer ได้ 

```js
import { actionTypes } from "./actions"

export default (state = initialState, { type, payload }) => {
    switch (type) {

    // เทียบค่า type ของ action 
    case actionTypes.SAVE_NEW_NOTE:

        // ถ้าสำเร็จ เราจะสร้าง redux state object ใหม่ 
        // โดยการโอนค่าเก่าจาก redux state object เดิมลงมาด้วย โดยใช้คำสั่ง ...state 
        // และสำหรับ array ต้องสร้าง instance หรือ object ใหม่เช่นกัน 
        const newState = { ...state }
        newState.notes = [ ...state.notes, { title: payload }]
        return newState
        // หรีือเขียนแบบย่อๆ ด้านล่าง
        //return { ...state, notes: [...state.notes, { title: payload }] }
```

## A. ไฟล์เต็ม `redux/reducer.js`

```js
import { actionTypes } from "./actions"

// ใช้ snippet: rxreducer
const initialState = {
    notes: [
        { title: 'a' },
        { title: 'b' },
        { title: 'c' }
    ]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actionTypes.SAVE_NEW_NOTE:
        const newState = { ...state }
        newState.notes = [ ...state.notes, { title: payload }]
        return newState
        //return { ...state, notes: [...state.notes, { title: payload }] }

    default:
        return state
    }
}
```